---
title: Assignment 7
tags: [private, cyber]
draft: true
date: 2026-03-29
---
## problem 1 runtime exploit mitigations 
### question 1
how ASLR reduces the reliability of a ROP exploit by disrupting the attacker's ability to predict the addresses of gadgets and useful code: 
- based on the nature of ASLR (address space layout randomization), the memory locations are scrambled each time an applciation runs. 
- attacks that target memory like buffer overflow or ROP rely on knowing the exact address of the data or data in memory. With ASLR, these addresses are shifted around randomly making it harder for attackers to predict those locations to inject malicious payloads. Hence, disrupting the attacker's ability to predict the addresses of gadgets and useful code. 

### question 2
Describe one practical technique attackers use to bypass ASLR in real systems, and explain why that technique restores exploit reliability.
- one of the most used bypass technique is information leak. ASLR works by randomizing the base addresses of memory regions like stack, heap, executable, libraries at load time. This security assumption is that the attacker doesn't know where anything lives. But these addresses will stay fixed for the process lifetime, because the OS cannot keep relocating things while code is running. This gives the attacker a large window to extract the layout once and use it.  An info leak causes the program to reveal a runtime address to the attacker before the exploit payload is delivered. A typical info leak exploit includes 2 stages: 
	- stage1 : leak a pointer. this is when the attacker find a vulnerability that causes the program to read and return memory it shouldn't. The leaked value is a raw virtual address from the current process run. 
	- stage 2: Because ASLR randomizes the base of a region but not the internal layout within it, the attacker can compute the base address of a region by subtracting the offset of a symbol within a library from the leaked address. 
- This technique converts a probabilistic defense into a deterministic one. The attacker now has a measurement of the actual memory layout rather than a guess. after the leak, the exploit is as reliable as it would have been on a system with no ASLR at all. 
### question 3
Explain how DEP/NX changes the set of viable memory-corruption payloads and why it tends to push attackers from code injection toward code-reuse (e.g., ROP/JOP)

- DEP/NX marks memory regions as non-executable ensuring data stored in memory cannot be executed as code, utlizing the NX bit to prevent exploitation. This feature eliminates code injection as a primitive: shellcode placed on the stack, heap, or buffer is treated as data by the CPU and causes a fault if the instruction pointer reaches it. This makes attackers reuse code instead of creating new instructions, they chian together existing executable code already loaded in the process (gadgets from libc, main binary or any linked library). In ROP, the attacker overwrites the return address and populates the stack with a sequence of gadget addresses. Each gadget executes a few instructions and returns, popping the next address from the attacker-controlled stack. DEP is not triggered becasue every instruction executed lives in a legitimately executable page. 


### question 4
Choose one control-flow defense (stack canaries, CFI, shadow stack/CET, or pointer authentication) and explain what concrete control-flow invariant it enforces.
- stack canaries is one of the most popular security defense for buffer overflows. A canary is a known value placed between the local variables and control data on the stack. On Linux, the canary is a random value drawn from the kernel at process startup. Before reading the return address, the canary is checked against the known value. Because a successful buffer overflows needs to overwrite the canary before reaching the return address, but the attacker cannot predict its value, the canary validation will fail and invalidate the return address. 
- stack canaries enforce the invariant that the return address has not been overwritten by a linear stack buffer overflow. 

### question 5
In a program protected with ASLR + DEP/NX + stack canaries, explain what *classes* of attacker goals can still succeed (e.g., data-only attacks, logic abuse, info leaks) and why those goals are not directly prevented by these mitigations.

- data-only attacks (Data-oriented programming DOP) can modify the program's data to change its behaviour without changing the program's control flow. Stack canaries only protect the return address adn saved frame pointer. They don't protect other local variables or adjacent data structures on the stack. If an attacker can overwrite a flag or function pointer elsewhere, the program will execute maliciously wtihotu ever triggering the canary check. 
- stack canaries do not protect against direct overwrites, so they are prone to info leaks and data attacks. ASLR is also prone to info leaks. DEP/NX stops code injection attacks but doesnot stop code reuse attacks. 
### question 6
Explain why “fail-fast” behavior (aborting/crashing when invariants are violated) can reduce
exploitability compared to attempting to recover and continue execution after detecting a
memory-safety error.

- fail-fast behavior reduces exploitability by immediately terminating a program upon memory-safety violations like buffer overflows, dangling pointers, preventing the corrupted state from being exploited to execute arbitrary code or leak information. Attempting to recover and continue after detecting a memory-safety error allows attackers to misuse this corrupted state, whereas immediate termination coverts a exploitable vulnerability into a harmless, non-exploitable crash. 
### question 7
Propose a CI/CD pipeline design that meaningfully improves resistance to memory-corruption and input-driven vulnerabilities by preventing regressions over time (describe three automated checks/gates and what each one is intended to catch).

- enable compiler sanitizers like ASan to avoid memory corruption detection
- continuously fuzz critical input surfaces (APIs, file handlers) using AFL++ 
- use static analyzers like SonarCloud

```
# pipeline was aided by ChatGPT 
name: Secure CI Pipeline

on:
  pull_request:
  push:
    branches: [ main ]

jobs:
  asan-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install dependencies
        run: sudo apt-get update && sudo apt-get install -y build-essential clang

      - name: Build with ASan
        run: |
          export CC=clang
          export CXX=clang++
          CFLAGS="-fsanitize=address,undefined -fno-omit-frame-pointer -g"
          CXXFLAGS="$CFLAGS"
          make clean && make

      - name: Run tests (ASan enabled)
        run: |
          ASAN_OPTIONS=detect_leaks=1 ./run_tests.sh

  fuzz:
    runs-on: ubuntu-latest
    needs: asan-test
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install AFL++
        run: sudo apt-get update && sudo apt-get install -y afl++

      - name: Build with AFL instrumentation
        run: |
          export CC=afl-clang-fast
          export CXX=afl-clang-fast++
          make clean && make

      - name: Run fuzzing (time-limited)
        run: |
          mkdir -p findings
          timeout 600 afl-fuzz -i fuzz_inputs -o findings -- ./fuzz_target @@

      - name: Check for crashes
        run: |
          if [ -d findings/default/crashes ] && [ "$(ls -A findings/default/crashes)" ]; then
            echo "Fuzzing found crashes!"
            exit 1
          fi
  sonarcloud:
    runs-on: ubuntu-latest
    needs: asan-test

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0  # required for SonarCloud

      - name: SonarCloud Scan
        uses: SonarSource/sonarcloud-github-action@v2
        with:
          args: >
            -Dsonar.projectKey=your_project_key
            -Dsonar.organization=your_org
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

## problem 2: software supply chain (xz utils incident)

### question 1 
Identify the exact point(s) in the software supply chain where trust was subverted in the xz
incident (e.g., maintainer identity/permissions, release artifact creation, downstream packaging) and explain why that point creates high leverage for an attacker.
- maintainer identity/permissions
	- the attacker started contributing to the XZ project for almost 2 years util they were given maintainer responsibilities. 
- release artifact creation
	- 
- downstream packaging
	- the backdoor affects with version 5.6.0 and 5.6.1
	- the CISA recommended to downgrade to an uncomprised version like 5.4.6
- explanation why the point creates high leverage: 
	- because this backdoor is added to the SSH daemon 


### question 2
2. Explain, at a high level, how the malicious changes propagated from developer-controlled inputs to end-user systems (focus on the “path” through artifacts and build/distribution steps).
 - the attacker spent two years making legitimate contributions to XZ utils, enough social trust to others to become a co-maintainer of the project. The malicious payload was not loaded in readable C source. It was injected as binary test files (.xz archives for testing) and activated through `configure.ac` build script. When downstream distributors ran `./configure && make` the build system extracted and linked the backdoor into `liblzma.so`. The malicious code was not present in the object files in any obvious form. On linux distros, sshd is launched via systemd's notification socket, which pulls in `libsystemd` which links `liblzma`. This meant the backdoor was loaded into the SSH daemon's process memory without `sshd` ever directly depending on XZ utils, making it harder to trace. Once inside `sshd`, the backdoor hooked the RSA pub key authentication path. An attacker with the matching private key could send a malicious payload during the SSH handshake to achieve unauthenticated remote code execution. 

### question 3
Explain why the xz backdoor was hard to detect with conventional source review and routine testing (focus on one core evasion idea, such as artifact mismatch, conditional activation, or indirection/obfuscation). 
- the xz backdoor was difficult to detect with conventional source review and routing testing because it employed artifact mismatch, where the malicious payload was absent from the public source code repository but injected during the build process. 
- the malicious payload was not directly injected to the source code files in the xz github repo. Instead it was added to the code tarball (release packages) through the obfuscation and injection techniques: 
	- hidden binary payloads in test files: the attacker included two binary test files (good-large_compressed.lzma and bad-3-correupt_lzma2.xz) in the repo. 
	- the malicious code was embedded in `m4` macro scripts used during the `configure` build phase. These scripts were designed to detect if the build was occuring on an x86-64 linux system and was part of a package build. when the `configure` script ran, the M4 macros extracted and decrypted the malicious binary data hidden within the test files. This extracted data was then used to replace legitimate object files during compilation injecting the backdoor directly into `liblzma.so` without it ever appearing in the git commit history. 

### question 4
Describe one concrete engineering control that could have detected the compromise earlier (e.g., reproducible builds, verifying release tarballs against VCS tags, artifact signing + transparency logs, provenance attestations) and explain the detection signal it would produce.

- one solution could be reproducible builds. 
	- the control: the reproducible builds ensure that a given source code when built in a controlled environment, produces byte-for-byte identical binary artifacts. 
	- in the XZ utils attack, the backdoor code was not present in the public Git repo source code. instead the attacker only included the obfuscated code in the release tarballs they built and signed. 
- the detection signal: if downstream packagers (linux distributions) had used a reproducible build system, they would have attempted to rebuild the package from the source code, compared it to the attacker-provided tarball. 
	

### question 5
Design a dependency-consumption policy for an organization that would reduce the blast radius of a similar event (focus on one policy that combines integrity verification, provenance, and staged rollout/canarying).

- policy: verified provenance + trusted artiface gate + staged rollout
	- integrity verification and provenance attestation: before any third-party dependency is consumed, the build system verifies 2 things: the artifact's cryptographic digest matches a pinned value in a lockfile, and a singed SLSA provenance attestation exists proving the artifact was built froma specific source commit by a known builder identity. The attestation is checked against a transparency log entry so that the claim is publicly auditable and cannot be silently revoked. A dependency that arrives without a valid attestation or whose digest doesn't match the lockfile is rejected before it ever enters the build. 
	- trusted artiface gate: the gate checks is the new version signed by a known maintainer key? does the provenance attestation chain back to a VCS commit that was reviewed and tagged? has the package crossed a minimum age threshold? 
	- staged rollout with automated rollback: depedency updates that pass the gate are not deployed to production immediately. They are first rolled out to a small fraction of production traffic and held there for an observation windows. Automated monitors watch for anomalies. If any sgnal fires during this canary window, the update is automatically rolled back and flagged for manual review. This limits blast radius. 

## problem 3 
### question 1
common structural feature shared by microarchitectural side channels is the sharing of hardware resources across different isolated and execution contexts including cache hierarchy, branch prediction, bufferes, translation lookaside buffers. Because these components are shared, one can influence and another can observe state changes, leaking information. 

### question 2
Describe one “general-purpose” mitigation strategy based on eliminating or strictly partitioning shared state (e.g., disabling SMT, core pinning/isolation, cache partitioning, flushing on context switch) and explain why it mitigates many different microarchitectural attacks.
- one of the mitigation strategies is disabling Simultaneous Multithreading (SMT). SMT is the component that allows 1 physical core to act as 2 logical cores, allowing 2 threads to execute simultaneously. But these threads share same caches and translation lookaside buffers which is vulnerable to side-channel attacks. Disabling SMT makes sure that only 1 thread runs on a physical core at a time. This means the attacker thread cannot observe other benign threads from the target system.  
### question 3 
Explain the **main reason** this general-purpose strategy is not universally deployed (choose the most important constraint in practice—performance, cost, utilization/scheduling, compatibility—and justify).

- Many modern workloads especially in cloud environments, servers, and parallel applications are optimized to take advantage of SMT to maximize hardware utilization. Without SMT, CPU resources may sit idle when a single thread cannot fully utilize them. Because of this, teams often prioritize performance and cost-efficiency over the security benefits especially when the microarchitectural attacks are considered lower risk in their threat model. 

### question 4
Explain what constant-time (data-oblivious) programming means and why it is an effective defense against timing/cache-based leakage even when the hardware is shared.

- constant-time programming is a secure coding technique ensuring code executes in the same amount of time regardless of secret inputs. 
- When a program reads or writes data in memory, the time it takes can depend on where that data is stored. If the data is already in the CPU's cache, it's fast. If not, it takes longer to fetch it. Attackers can measure these tiny time differences to figure out which memory locations are being accessed. And from that, they can guess the secret keys. Constant-time programming defends against timing-based leakage by ensuring a program's execution time is independent of secret data like cryptographic keys.  In other words, the code is designed so that all possible secret values cause the same sequence of operations to be executed, taking approximately the same amount of time. The purpose of this technique is to remove any measurable timing differences that could allow an attacker to infer secret information, such as cryptographic keys. 
### question 5
In a cloud setting with mutually untrusted tenants, explain which is typically more practical for the provider to deploy at scale—(a) isolation/partitioning-based mitigations or (b) requiring constant-time software—and justify your answer in terms of incentives and enforceability.

- In my opinion, isolation-based mitigations like cache partitioning, virtual machine isolation, etc. are more practical over constant-time software. Isolation-based mitigations can be implemented at the OS or hardware level wihtout modifying existing application code. Whereas constnat-time software is more helpful n crptographic areas. It can be challenging to implement correctly. This method requires ensuring no branch conditions or memory accesses depend on secret data often needing techniques like bitslicing (instead of storing a n-bit data element in one n-bit variable, you split it across n separate variables, each holding 1 bit, then use bitwise operations like AND, OR, and XOR to do all the computations) or masking. 