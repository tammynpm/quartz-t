---
title: Untitled 6
tags: []
draft: true
date: 2026-05-10
---
1.1/
- Physically connect to flash memory chips using a JTAG debugger or flash reader to read data directly. 
- download firmware update executable files and use utilities like binwalk to extract the raw binary
- Monitor network traffic during an OTA update to intercept the firmware download URL and grab the package directly

1.2/ 
- assume secure boot is implemented, 
	- we can use voltage glitching to skip the verification instruction at the right moment. 
	- exploit a software vulnerability to get a root shell and dump firmware from the live filesystem with `dd`, bypassing secure boot entirely.
- assume JTAG is disabled, 
	- we can re-enable it through test pads or by manipulating configuration fuses. 
	- use an alternative debug interface like UART to get a serial console, since disabling JTAG doesn't necessarily lock down UART.
- assume the firmware on the website is encrypted 
	- we can sniff the update traffic between the router and the update server to capture the decrypted image in transit. 
	- xtract the decryption key from the device's flash memory or running memory via a UART shell, then decrypt the downloaded image offline.

1.3/ 

2/ 
first, we need to find the fault values. I wrote a python script to do this. 
```
def delta(correct_cipher):
    with open('outputs.txt', 'r') as file:
        lines=[line.rstrip() for line in file]
    file.close()
    for line in lines:
        correct_int = int(correct_cipher, 16)
        faulty_int = int(line, 16)
        delta = correct_int ^ faulty_int
        if (delta != 0):
            print(f"{delta:032x}")

if __name__=="__main__":
    delta("c360e28e60f173af16a1889efd678f33")
```

The results are: 
```
000000d6000000000000000000000000
000000000000000000000000f5000000
000000000000000000000000f5d083fc
00000000000000000047000000000000
000000000000fd000000000000000000
00000000000000000000e80000000000
00000000000000000000000000d00000
00000000008900000000000000000000
000000000000003e0000000000000000
000000000000000000000000000000fc
00f30000000000000000000000000000
00000000750000000000000000000000
b6000000000000000000000000000000
0000000000000000c100000000000000
0000b400000000000000000000000000
0000000000000000000000f900000000
```

so the round 10 key is 
b6 f3 b4 d6 75 89 fd 3e c1 47 e8 f9 f5 d0 83 fc

The next step is to reverse the AES-128 key schedule to recover the original key using this public tool https://github.com/fanosta/aeskeyschedule. 
The original key is `556e627265616b61626c654b65797321`

the plaintext is `UnbreakableKeys!`