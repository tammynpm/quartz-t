![[Pasted image 20260228234910.png]]




|                           | explanation                                                                                                                          |     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --- |
| `stp x29, x30, [sp,-16]!` | decrement sp by 16 first, then store x29 <-- sp, x30 <-- sp + 8                                                                      |     |
| `mov x0,0`                | x0 <-- 0                                                                                                                             |     |
| `add x29, sp, 0`          | x29 <-- x29+0 = x29                                                                                                                  |     |
| `bl time`                 | branch to label `time`                                                                                                               |     |
| `mov x1, x0`              | x1 <-- x0 = 0                                                                                                                        |     |
| `ldp x29, x30, [sp],16`   | this is post index load pair of pointer: sp <-- x29, sp + 8 <-- x30, and then increment sp <-- sp + 16                               |     |
| `adrp x0, .LC0`           | adrp is address of page. this instruction takes the address of `.LC0`, rounds it down to a 4KB page, loads that page address into x0 |     |
| `add x0,x0, :lo12:.LC0`   | :lo12 means take the lower 12 bits of the symbol address. x0 <-- x0 + the lower 12 bits of symbol address .LC0                       |     |
| `b printf`                | branch to the label `printf`                                                                                                         |     |
| `.LC0:`                   |                                                                                                                                      |     |
| `.string "%d\n"`          |                                                                                                                                      |     |



why split into page + offset 
adrp can encode large pc-relative distances
add handles the 12-bit offset
this works with PIE (position independent executables)



gdb-multiarch + QEMU


launch QEMU with a built-in gdb server, connect to the gdb server using gdb-multiarch client. 



user-mode debugging used for only 1 program. 

start qemu with the gdb server in 1 terminal
second terminal for gdb multiarch 


### level 2. 

|                         |                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------- |
| `0: ldrb w1, [x0]`      | take the byte address in x0, load only 1 byte to w1, upper bits are filled with zeros |
| `4: mov x3, x0`         | x3 <-- x0                                                                             |
| `8: cbz w1, 2c`         | compare branch zero , if w1, 2c                                                       |
| `c: sub w2, w1, #0x41`  | w2 <-- w1 - 0x41 <--                                                                  |
| `10: add w1, w1, #0x20` | w1 <-- w1 - 0x20                                                                      |
| `14: uxtb w2, w2`       |                                                                                       |
| `18: cmp w2, 0x19`      | compare between w2 and 0x19                                                           |
ldrb : load register byte 

### level 3

```
f:
	stp x29, x30, [sp, -16]!
	add x29, sp, 0
	ldrb w0, [x0]
	cmp w0, 89
	beq .L6
	bls .L20
	cmp w0, 110
	beq .L5
	cmp w0, 121
	bne .L2
.L6:
	mov w0, 1
	ldp x29, x30, [sp], 16
	ret
.L20:
	cmp w0, 78
	beq .L5
.L2:
	adrp x0, .LC0
	add x0, x0, :lo12:.LC0
	bl puts
	mov w0, 0
	bl exit
.L5:
	mov w0, 0
	ldp x29, x30, [sp], 16
	ret
.LC0:
	.string "error!"
```


|                             |                                               |
| --------------------------- | --------------------------------------------- |
| `	stp x29, x30, [sp, -16]!` |                                               |
| `add x29, sp, 0`            | x29 <-- sp + 0                                |
| `ldrb w0, [x0]`             | load register byte, load 1 byte from x0 to w0 |
| `cmp w0, 89`                | compare w0 to 89                              |
| `beq .L6`                   | branch if equal to label .L6                  |
| `bls .L20`                  |                                               |
| `cmp w0, 110`               | compare w0 to 110                             |
| `beq .L5`                   | if w0=110 branch to label .L5                 |
| `cmp w0, 121`               | compare w0 to 121                             |
| `bne .L2`                   | branch if not equal to label .L2              |
| mov w0, 1                   | w0 <-- 1                                      |
| `ldp x29, x30, [sp],16`     | sp <-- x29, sp+8 <-- x30, sp=sp+16            |
| ret                         | return value in x0                            |
| cmp w0,78                   | compare w0 and 78, if equal set Z flag =1     |
| beq                         | if Z=1 then jump to label .L5                 |
| adrp                        | add the page .LC0 to x0                       |
| add                         | x0 <-- x0 +                                   |

## level 4
![[Pasted image 20260302141000.png]]

input: 
output
types 


|                       |                          |
| --------------------- | ------------------------ |
| lsr w1, w0, #1        | w1 <-- w0 >> 1           |
| `and w1, w1, 0x55555` | w1 <-- w1 AND 0x55555555 |
| `sub w0, w0, w1`      | w0 <-- w0 - w1           |
| `and `                | w1<-- w0 AND 0x33333333  |
| lsr                   | w0 <-- w0 >> 2           |
| and                   | w0 <-- w0 AND 0x33333333 |
| add                   | w0 <-- w0 + w1           |
| mov                   | w1<-- 0x1010101          |
| add                   | w0<--w0 + (w0 >> 4)      |
| and                   | w0 <-- w0 AND 0xf0f0f0f  |
| mul                   | w0 <-- w0  x w1          |
| lsr                   | w0 <-- w0 >> 24          |
|                       |                          |

## problem 2; 


```
aarch64-linux-gnu-gcc -O0 -g -o listing1 listing1.c 

file listing1
listing1: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=83ecc8ab251261a039ae30df1dcef08be02906df, for GNU/Linux 3.7.0, with debug_info, not stripped



readelf -h listing1
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Position-Independent Executable file)
  Machine:                           AArch64
  Version:                           0x1
  Entry point address:               0x680
  Start of program headers:          64 (bytes into file)
  Start of section headers:          69752 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         10
  Size of section headers:           64 (bytes)
  Number of section headers:         35
  Section header string table index: 34
```


This is ELF 64-bit LSB pie executable, 
Size of .text is 00000000000001f4

size of .data is 0000000000000010

size of .rodata 0000000000000019

```shell
readelf -S listing1
There are 35 section headers, starting at offset 0x11078:

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .note.gnu.bu[...] NOTE             0000000000000270  00000270
       0000000000000024  0000000000000000   A       0     0     4
  [ 2] .interp           PROGBITS         0000000000000294  00000294
       000000000000001b  0000000000000000   A       0     0     1
  [ 3] .gnu.hash         GNU_HASH         00000000000002b0  000002b0
       000000000000001c  0000000000000000   A       4     0     8
  [ 4] .dynsym           DYNSYM           00000000000002d0  000002d0
       00000000000000f0  0000000000000018   A       5     3     8
  [ 5] .dynstr           STRTAB           00000000000003c0  000003c0
       0000000000000094  0000000000000000   A       0     0     1
  [ 6] .gnu.version      VERSYM           0000000000000454  00000454
       0000000000000014  0000000000000002   A       4     0     2
  [ 7] .gnu.version_r    VERNEED          0000000000000468  00000468
       0000000000000030  0000000000000000   A       5     1     8
  [ 8] .rela.dyn         RELA             0000000000000498  00000498
       00000000000000c0  0000000000000018   A       4     0     8
  [ 9] .rela.plt         RELA             0000000000000558  00000558
       0000000000000078  0000000000000018  AI       4    22     8
  [10] .init             PROGBITS         00000000000005d0  000005d0
       0000000000000018  0000000000000000  AX       0     0     4
  [11] .plt              PROGBITS         00000000000005f0  000005f0
       0000000000000070  0000000000000000  AX       0     0     16
  [12] .text             PROGBITS         0000000000000680  00000680
       00000000000001f4  0000000000000000  AX       0     0     64
  [13] .fini             PROGBITS         0000000000000874  00000874
       0000000000000014  0000000000000000  AX       0     0     4
  [14] .rodata           PROGBITS         0000000000000888  00000888
       0000000000000019  0000000000000000   A       0     0     8
  [15] .eh_frame_hdr     PROGBITS         00000000000008a4  000008a4
       0000000000000044  0000000000000000   A       0     0     4
  [16] .eh_frame         PROGBITS         00000000000008e8  000008e8
       00000000000000d0  0000000000000000   A       0     0     8
  [17] .note.ABI-tag     NOTE             00000000000009b8  000009b8
       0000000000000020  0000000000000000   A       0     0     4
  [18] .init_array       INIT_ARRAY       000000000001fdc8  0000fdc8
       0000000000000008  0000000000000008  WA       0     0     8
  [19] .fini_array       FINI_ARRAY       000000000001fdd0  0000fdd0
       0000000000000008  0000000000000008  WA       0     0     8
  [20] .dynamic          DYNAMIC          000000000001fdd8  0000fdd8
       00000000000001e0  0000000000000010  WA       5     0     8
  [21] .got              PROGBITS         000000000001ffb8  0000ffb8
       0000000000000030  0000000000000008  WA       0     0     8
  [22] .got.plt          PROGBITS         000000000001ffe8  0000ffe8
       0000000000000040  0000000000000008  WA       0     0     8
  [23] .data             PROGBITS         0000000000020028  00010028
       0000000000000010  0000000000000000  WA       0     0     8
  [24] .bss              NOBITS           0000000000020038  00010038
       0000000000000008  0000000000000000  WA       0     0     1
  [25] .comment          PROGBITS         0000000000000000  00010038
       0000000000000012  0000000000000001  MS       0     0     1
  [26] .debug_aranges    PROGBITS         0000000000000000  0001004a
       0000000000000030  0000000000000000           0     0     1
  [27] .debug_info       PROGBITS         0000000000000000  0001007a
       0000000000000110  0000000000000000           0     0     1
  [28] .debug_abbrev     PROGBITS         0000000000000000  0001018a
       00000000000000c0  0000000000000000           0     0     1
  [29] .debug_line       PROGBITS         0000000000000000  0001024a
       000000000000008c  0000000000000000           0     0     1
  [30] .debug_str        PROGBITS         0000000000000000  000102d6
       0000000000000083  0000000000000001  MS       0     0     1
  [31] .debug_line_str   PROGBITS         0000000000000000  00010359
       000000000000004a  0000000000000001  MS       0     0     1
  [32] .symtab           SYMTAB           0000000000000000  000103a8
       0000000000000930  0000000000000018          33    74     8
  [33] .strtab           STRTAB           0000000000000000  00010cd8
       0000000000000247  0000000000000000           0     0     1
  [34] .shstrtab         STRTAB           0000000000000000  00010f1f
       0000000000000153  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  D (mbind), p (processor specific)
```


### question 3
function graph for the X() function 

code-flow function graph of X() from Ghidra:
![[Pasted image 20260302115128.png]]
### question 4
the format string in main() stored in the address  
![[Pasted image 20260301015236.png]]


![[Pasted image 20260301015836.png]]

`<_init-0x5d0>` is at address 
`adrp` loads the 4KB-aligned page base of the target address. `add` instruction loads the page offset then printf is called with x0 as the format string. 
The string is stored at address of the page from `adrp` + 0x890.  
## problem 3

library calls 
![[Pasted image 20260302121642.png]]

there is 1 library call of printf

### question 2
![[Pasted image 20260302122125.png]]

there is no parameter 
the signature of C program has argc and argv. The function signature should have been `int main(int argc, char* argv[])`


- what the input paramt
### question 3
