phrase `bits 16` as assembler directive --> instructs the assembler to generate code intended to run on a processor operating in a 16-bit mode
- 2 bytes 
- common for MS-DOS or bootloaders

```
bits 16
org 0x100

mov ax, 0x0e41 ; load 'A' into AL and use interrupt to print
mov bx, 0 ; set page number to 0
int 0x10 ; bios interrupt to print character

mov ah, 0x4c ; DOS exit function
int 0x21 ; call DOS interrupt 

```



## loadit LD_PRELOAD


## dns pwning 

tunneling tcp via a dnscat


this tool creates an encrypted and control channel over the dns protocol which is an effective tunnl out of almost every network 


the client run on a compromised machine, written in c, has the minimum possible dependices. 

/home/player1/.local/share/gem/ruby/3.4.0/bin