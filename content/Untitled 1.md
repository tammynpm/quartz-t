Answer the following questions:
1) There is an overwhelming reason why this attack won’t work on your current ARM system as-is. What is it?

Stack smashing requires injecting shellcode directly onto the stack and then redirecting execution to that shellcode via a corrupted return address. Modern operating systems implement NX (no execute) or DEP (data execution prevention) protections that mark the memory pages like stack and heap as data-only regions, especially on aarch64, the MMu enforces strict page-level permissions such that a memory page marked as writable cannot simultaneously be executed. When the CPU tries to execute instructions from those regions, the processor raises an exception and the program crashes. Even if a buffer overflow overwrites the return addresses and redirects execution ot injected shellcode in the stack, the processor will refuse to execute that code. 

2) Assuming it would work, how would the shell code operate using AARCH64? Provide a sketch of the AARCH64 assembly code (it does not need to be runnable) with the following elements:
a) Code for the system calls you will have to leverage (same as the paper but implemented
via ARM). The toolchain you have set up Ghidra may be of help here.
b) Code for the control-flow of the gadget at the bottom of page 11.
You do not need to have the exact stack arrangements worked out.


assuming NX/XN protections were absent, the shellcode would need to replicate what the paper demonstrates on x86, invoke execve() to spawn a shell, and optionally invoke exit() to clean up. On aarch64, system calls are made via the svc aka supervisor call instruction rather than the int 0x80 approach used in x86. arguments are placed in registers x0 through x5, the syscall number goes in x8, and svc #0 transfers control to the kernel . 

```shell
.section .text
.global _start 

_start:
	adr x0, binsh ; x0 points to sring "/bin/sh\0"
	adr x1, argv  ; x1 points to argv array [x0, NULL]
	mov x2, #0    ; x2 = envp = NULL 
	mov x8, #221  ; syscall number for execve 
	svc #0        ; invoke kernel 
	
binsh:
	.ascii "/bin/sh\0"
	
argv: 
	.quad binsh
	.quad 0
```

Aarch64 passes all arguments in registers rather than pushing them on the stack. Aarch64 also don't use push/pop. The ADR instruction computes pc-relative addresses for the position-independent shellcode since the payload may land at an unpredictable address at runtime. 

2b/ in the paper, a gadget allows execution to jump into attacker-controlled data. On Aarch64 an equivalent control-flow redirection could look like this 
```shell
shellcode_start:
	bl get_pc 
	
get_pc:
	mov x10, LR
	add x0, x10, #(binsh - get_pc) ; x0 <-- "/bin/sh\0"
	add x1, x10, #(argv - get_pc) ; x1 <-- argv array
	mov x2, #0
	mov x8, #221
	svc #0 
	
binsh:
	.ascii "/bin/sh"
	
argv: 
	.quad binsh
	.quad 0
```

Aarch64 uses BL/get_pc instead of the call/pop like in x86. Aarch64 doesn't have a direct way to read the PC into a general purpose register, this sequence is a clean method to determine where the shellcode currently lives. 

3) If you wrote a version of the exploit overflow1.c using your code, you would run into
another issue right away. What is it?


even if the exploit is rewritten for AArch64 the exploit would encounter stack canary. ASLR randomizes the memory addresses of key program regions like the stack, heap, shared libraries, and executable segments. because of the randomization, the attacker cannot predict the address of the injected shellcode on the stack. This method places a randomly generate canary value between the local variables and the saved frame record at function entry. Before the function returns, the runtime checks whether the canary value has been modified. If it has been altered by the overflow, the program calls `__stack_chk_fail()` terminates immediately ad prints a stack smashing detected message. 


## problem 2 ROP gadgets 

1/ An example of an Arithmetic ADD gadget in aarch64 assembly code. 
```shell
gadget_add:
	add x0, x0, x1
	ldr x30, [sp], #16
	ret
```

the ADD instruction performs the operation within registers and doesn't touch memory in a way that would trigger a fault. The LDR instruction then pops the next gadget address off the stack into LR (x30). The ret intrustion execute whatever address was loaded into x30 which is the next gadget in the chain. Through this method, an attacker can chain up short sequences like this one without needing to inject their own code. 

2/ an example of Branh-on-Equal in aarch64 assembly 

```shell
gadget_beq:
	cmp x0, x1
	b.eq target
	ret
```

This gadget replicates an if-equal branch within a ROP chain. The cmp instruction compares the values in registers x0 and x1 and sets the process condition flags. The branch instruction branches if the two values are equal. If the branch is not taken, execution continues and returns. In a ROP chain, this allows attackers to choose whether the branch occurs. 

3/ ASLR makes ROP more difficult because ROP chain works on knowing the exact virtual addresses of gadgets inside target binaries at the time of exploitation. Each gadget must be computed precisely. Even a single byte of offset error will redirect execution to the wrong location and crash the process rather than running the intended chain. 

When ASLR is active, the base address of the stack, heap, and all mapped libraries inlcuding libc are randomized at each process invocation. The randomization makes brute forcing infeasible. 
The common approach to bypass ASLR in the contect of ROP is to first exploit a separate information disclosure vulnerability that leaks a pointer to a known location within a target library. Once one absolute address is known, the attacker can compute the randomized base address of the entire library by subtracting the known static offset of the leaked symbol. Without such a leak, it is more difficult to construct a working ROP chain. 


## problem 3 micro-architectural attack vendors

Micro-architectural attacks are devastating because they operate beneath the abstraction layer that the software security model. 
- Micro-architectural attacks exploit the gap between what the architecture specifies and what the implementation actually does. For example, speculative executive performs memory reads and computations before permissions are fully verified then discards results that would have caused a fault. But these discarded computations leave measurable side effects in micro-architectural state such as cache lines. An attacker can see these side effects through timing measurements to recover data. Software patches cannot fully patch hardware behavior, especially with the cost of performance penalties. 
- micro-architectural attacks are a traditional exploit requires that the attacker gain code execution at a target privilege level whereas a micro-architectural attack like Spectre allows unprivileged user-space code to read kernel memory or even hypervisor memory. The attacker doesn't need to corrupt a return address or hijack control flow at all.
- they are difficult to detect because they leave no trace in process memory or no unusual system calls. The attack channel is a timing side channel conducted through shared hardware resources like caches, branch predictors, or translation lookaside buffers, those that cannot be monitored with a software-layer tool. 
- the attack surface is very broad. new variants of speculative execulation attacks continue to be discovered years after the initial Spectre and Meltdown disclosures because the root cause is the design of modern out-of-order speculative processors. 



