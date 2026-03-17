![[Pasted image 20260228234910.png]]

This function print out the input string. 

|                           | explanation                                                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `stp x29, x30, [sp,-16]!` | decrement sp by 16 first, then store x29 <-- sp, x30 <-- sp + 8                                                                      |
| `mov x0,0`                | x0 <-- 0                                                                                                                             |
| `add x29, sp, 0`          | x29 <-- x29+0 = x29                                                                                                                  |
| `bl time`                 | branch to label `time`                                                                                                               |
| `mov x1, x0`              | x1 <-- x0 = 0                                                                                                                        |
| `ldp x29, x30, [sp],16`   | this is post index load pair of pointer: sp <-- x29, sp + 8 <-- x30, and then increment sp <-- sp + 16                               |
| `adrp x0, .LC0`           | adrp is address of page. this instruction takes the address of `.LC0`, rounds it down to a 4KB page, loads that page address into x0 |
| `add x0,x0, :lo12:.LC0`   | :lo12 means take the lower 12 bits of the symbol address. x0 <-- x0 + the lower 12 bits of symbol address .LC0                       |
| `b printf`                | branch to the label `printf`                                                                                                         |
| `.LC0:`                   |                                                                                                                                      |
| `.string "%d\n"`          | load the string in .rodata                                                                                                           |
|                           |                                                                                                                                      |

### level 2. 
![[Pasted image 20260302165908.png]]

| instruction             | explanation                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| `0: ldrb w1, [x0]`      | load only 1 byte from address x0 to w1, upper bits are filled with zeros    |
| `4: mov x3, x0`         | x3 <-- x0                                                                   |
| `8: cbz w1, 2c`         | compare w1 to 0, if Z=1 then jump to instruction 2c                         |
| `c: sub w2, w1, #0x41`  | w2 <-- w1 - 0x41                                                            |
| `10: add w1, w1, #0x20` | w1 <-- w1 - 0x20                                                            |
| `14: uxtb w2, w2`       | extract least significant 8 bits from w2 and extend zeroes to assign to w2  |
| `18: cmp w2, 0x19`      | compare between w2 and 0x19                                                 |
| `1c`                    | if w2 < 0x19 then jump to 24                                                |
| 20                      | store least significant 8 bits from w1 to x3                                |
| 24                      | load  a byte from the address 1 byte after x3 to w1                         |
| 28                      | compare w1 and 0, if equal set Z=1, else Z=0. If Z=1 jump to instruction c. |
| 2c                      | return                                                                      |


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

| instruction                | explanation                                                   |
| -------------------------- | ------------------------------------------------------------- |
| `stp x29, x30, [sp, -16]!` | sp = sp - 16, x29 <-- sp, x30 <-- sp + 8                      |
| `add x29, sp, 0`           | x29 <-- sp + 0                                                |
| `ldrb w0, [x0]`            | load register byte, load 1 byte from x0 to w0                 |
| `cmp w0, 89`               | compare w0 to decimal 89 which is 'Y'                         |
| `beq .L6`                  | branch if equal to label .L6                                  |
| `bls .L20`                 | if w0 <= decimal 89 then jump to label .L20                   |
| `cmp w0, 110`              | compare w0 to 110 or 'n', set Z=1 if equal                    |
| `beq .L5`                  | if Z=1 branch to label .L5                                    |
| `cmp w0, 121`              | compare w0 to 121 'y', set Z=0 if not equal                   |
| `bne .L2`                  | branch to label .L2 if Z=0                                    |
| mov w0, 1                  | w0 <-- 1 return register w0 with 1                            |
| `ldp x29, x30, [sp],16`    | sp <-- x29, sp+8 <-- x30, sp=sp+16                            |
| ret                        | return value in x0                                            |
| cmp w0,78                  | compare w0 and 78, if equal set Z flag =1                     |
| beq                        | if Z=1 then jump to label .L5                                 |
| adrp x0, .LC0              | x0 <-- base address of .LC0                                   |
| add x0, x0, :lo12:.LC0     | x0 <-- lowest 12 bits of full address of base address of .LC0 |
| bl puts                    | branch link to `puts`                                         |
| `mov w0, 0`                | w0 <-- 0, return register w0 with 0                           |
| bl exit                    | branch link to exit                                           |
| mov w0, 0                  | w0 <-- 0 return register w0 with 0                            |
| `ldp x29, x30, [sp], 16`   | x29 <-- sp , x30 <-- sp + 8, sp= sp + 16                      |
| ret                        | return                                                        |
| .string "error!"           | load  the string "error!" in .rodata                          |

## level 4
![[Pasted image 20260302141000.png]] 

| instruction | explanation              |
| ----------- | ------------------------ |
| 0           | w1 <-- w0 >> 1           |
| 4           | w1 <-- w1 AND 0x55555555 |
| 8           | w0 <-- w0 - w1           |
| c           | w1<-- w0 AND 0x33333333  |
| 10          | w0 <-- w0 >> 2           |
| 14          | w0 <-- w0 AND 0x33333333 |
| 18          | w0 <-- w0 + w1           |
| 1c          | w1<-- 0x1010101          |
| 20          | w0<--w0 + (w0 >> 4)      |
| 24          | w0 <-- w0 AND 0xf0f0f0f  |
| 28          | w0 <-- w0  x w1          |
| 2c          | w0 <-- w0 >> 24          |
| 30          | return the value in w0   |

## problem 2


```
aarch64-linux-gnu-gcc -O0 -g -o listing1 listing1.c 

file listing1
listing1: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=83ecc8ab251261a039ae30df1dcef08be02906df, for GNU/Linux 3.7.0, with debug_info, not stripped
```


This is ELF 64-bit LSB pie executable, 
Size of .text is 00000000000001f4
size of .data is 0000000000000010
size of .rodata 0000000000000019

![[Pasted image 20260302162641.png]]
### question 3
function graph for the X() function 

code-flow function graph of X() from Ghidra:
![[Pasted image 20260302115128.png]]
### question 4
From the disassembly in Ghidra, the format string locates at address 100890
![[Pasted image 20260302162950.png]]
## problem 3

![[library-calls.png]]
There is only one library call which is `printf()`
![[Pasted image 20260302161000.png]]

The location of `printf` is at 004009cc. 
### question 2
![[Pasted image 20260302122125.png]]

The issue is that there is no parameter. 
The signature of C program has argc and argv. The function signature should have been `int main(int argc, char* argv[])`
### question 3

pseudo code for the algorithm 

mix1 returns `(b0+b1+b2) * 10 + b3 + b0 * b7 - b6`
![[mix1.png]]

mix2 returns `b6 * 10 + 99999 + b4 * b0 - b4`
![[mix2.png]]

mix3 returns `b7 + b0 - b5 + 2 * b3`
![[mix3.png]]
zero() returns `(x1 * 0x14 - 7) * x1`


intmix() returns `((((y1 + 99U ^ y2) & y2 + x1 + y2) - x1) - x2 ^ x1 ^ x2`
where `y1 = (x1 * 0x14 -7) * x1` and `y2 = y1 + 99U + x2`



![[main-default-generate.png]]
```shell
function main(int argc, char** argv):
	y1 <-- (x1 * 0x14 -7) * x1
	y2 <-- y1 + 99U + x2
	b0, b1, b2, b3, b4, b5, b6, b7 <-- *argv[1], argv[1][1], argv[1][2], argv[1][3], argv[1][4], argv[1][5], argv[1][6], argv[1][7]
	x1 <-- mix1(argv) <-- (b0+b1+b2) * 10 + b3 + b0 * b7 - b6
	x2 <-- mix3(argv) <-- b7 + b0 - b5 + 2 * b3
	x5 <-- intmix(x1,x2) <-- ((((y1 + 99U ^ y2) & y2 + x1 + y2) - x1) - x2 ^ x1 ^ x2 
	x4 <-- mix2(argv) <-- b6 * 10 + 99999 + b4 * b0 - b4
	x3 <-- mix3(argv) <-- b7 + b0 - b5 + 2 * b3
	x1 <-- mix1(argv) <-- (b0+b1+b2) * 10 + b3 + b0 * b7 - b6
	x2 <-- mix2(argv) <-- b6 * 10 + 99999 + b4 * b0 - b4
	x6 <-- intmix(x1,x2) <-- ((((y1 + 99U ^ y2) & y2 + x1 + y2) - x1) - x2 ^ x1 ^ x2
	
	return the value as the format w1w2.w3w4 where the w1 is x5 AND 0xffffffff, w2 is x4, w3 is x3, w4 is x6 AND 0xffffffff
```

