---
title: assignment 9
tags: []
draft: true
date: 2026-04-08
---
## Stick Figure Guide to AES

1. Read AES Stick figure walk-through: http://www.moserware.com/2009/09/stick-figure-guide-to-advanced.html
a. How large is the AES block size? 
b. How many bytes of key are used for AES-256?
c. How many bytes of that key are used in the first round? (i.e., how large is an AES round
key?)
d. How many times does the “mix columns” operation occur within each round?
e. How many bytes of the round key are used within a single “mix columns” operation?


a. AES-256 block size is 128 bits (16 bytes) 
b. AES-256 uses a key size of 32 bytes. 
c. Each AES-256 round key is 128 bits (16 bytes) in size. 

d. the columns mix happen once in every round except for the final round. 
e. 0 byte of the round key are used during the Mix Columns operation. 
## cipher modes
2. CBC mode
	1. CBC mode uses an Initialization Vector (IV) to introduce randomness into the first block of encryption ensuring that identical plaintext messgaes encrypted with teh same key produce entirely differently ciphertext blocks. Without an IV, the first ciphertext block becomes fully deterministic. All subsequent blocks are still chained, so they benefit from prior block randomness, but the first block si exposed. If an attacker can submit plaintexts for encryption they can submit a known plaintext and observe the first ciphertext block. Then submit it again, if the first block is identical the adversary can confirm the IV si fixed. By varying the plaintext one byte at a time to deduce the key's effect on that block, the adversary can eventually recover information about the plaintext or key. 
	2. if CBC mode doesn't use an IV, the first plaintext block is encrypted drectly with the key and no randomization. This means identical plaintexts always produce identical ciphretext. This would allow the attacker to build a ciphertext dictionary mapping known first blocks to their plaintext.  

## AES Python Scripting
3. python script that demonstrates a working AES implementation: 

**answer**
An open source implementation of AES in python3: https://gist.github.com/lovasoa/a0b384ca0e9c0a485f212caab68d98ec originally written by Daniel Miller and converted to python3 by lovasoa.

4. For the given ciphertext and key:
Key = 0x54656c6c206e6f206f6e652074686973
Ciphertext = 0x780b47028aac157577cfa72928f4eac2
What is the intermediate state of the AES block immediately after the inverse-sbox in the first round of the decryption?

**answer**
State after InvSubBytes (first round): 4c158adb72025fef7509ec02cd69a9e4

python3 script: 
```python3
# with the support of claude 
from aes import AES, xor

key=bytes.fromhex("54656c6c206e6f206f6e652074686973")
cipher=bytes.fromhex("780b47028aac157577cfa72928f4eac2")

aes = AES(key)
n=aes.nb * 4
aes.state = bytearray(cipher)
keys=aes.key_schedule()

k=keys[aes.nr*n:(aes.nr+1)*n]
aes.add_round_key(k)

r=aes.nr-1
aes.inv_shift_rows()
aes.inv_sub_bytes()
print("State after InvSubBytes (first round):", bytes(aes.state).hex())

```

5. AES key schedule
a. Reverse the AES-128 key schedule of the following round-10 subkey to derive the
original 128-bit AES key. (Note – the Python package ‘aeskeyschedule’ will do this for
you.) Round-10 subkey: 0xcd17d2fe48e50be680f3e035a7928027
b. What are the implications of the AES key schedule being reversable (from an attacker’s
standpoint)?


a. Original AES-128 key: 50757265206d61706c65207379727570
Supported Python3 script:
```python
from aeskeyschedule import reverse_key_schedule
round10=bytes.fromhex("cd17d2fe48e50be680f3e035a7928027")
original_key=reverse_key_schedule(round10,10)
print("Original AES-128 key:", original_key.hex())
```

b. Implications of the AES key schedule being reversable: If an attacker obtains any round key, they can mathematically derive the original key and every other round key. 
## security architectures
6. What potential vulnerabilities exist without employing any security – what, specifically could a defender or opportunistic 3rd party do to your system?
a. Describe your exploit and C2 scheme
b. Identify at least one way a defender could find your exploit on the system, provide
sufficient detail to describe how the defender would do this
c. Identify at least one way a defender could detect and/or interpret your C2 via network,
provide sufficient detail to describe how the defender would do this
d. Identify at least one way an opportunistic 3rd party could leverage your system, provide
sufficient detail to describe what the 3rd party would be attempting to do and how they
would do this



a. Exploit and C2 scheme
Loader Stage: `loader.c` is a no-libc, direct-syscall binary that 
1. wgets the implant from the C2 file server to `/dev/shm/Agenda_3-31-26`
2. chmod 700 the implant
3. forks the implant into the background (redirecting stdio to `/dev/null`, calling `setsid`)
4. self-deletes by reading `/proc/<pid>/exe` then calling `unlink` on itself

implant `implant.py`: connects back to the C2 TCP listener in a while True loop (reconnecting every 300s on failure). Commands are exchanged as RSA-OAEP encrypted, base64-encoded, length-prefixed binary messages. Support operations are `HEARTBEAT, READ_FILE, WRITE_DATA, RUN_COMMAND, EXFIL_FILE, SELF_DESTRUCT`.

Exfil channel `exfilt_server.py`: on the command `EXFIL_FILE`, the implant reads the file and POSTs raw bytes to separate http channel: `http://<EXFIL_HOST>:<EXFIL_PORT>/upload?name=<filename>`


b. A way that a defender could find the IMplant is to inspect `/dev/shm` and running processes. They can do this using commands like `ls -la /dev/shm` or `ps aux`. 

c. A way a defender could detect C2 via Network: monitor outbound TCP connections from the Confluence servers. The defender could implement a network monitor using NDR, firewall logs, etc. watching for outbound traffic from the Confluence host, especially if the the connection renewed every 300s to the same destination. That is a common beaconing pattern.

d. A way an opportunistic 3rd party could leverage the system:
The C2 port is bound on 0.0.0.0 meaning it's reachable from outside.  A 3rd party who discovers the C2 port via a simple port scan can connect to that port before the implant beacons back. The 3rd party can then observe the operator's commands being sent, preventing the real implant from ever connecting to the C2 server. 

7. How could you overcome these threats to your system? How will keys be established, are their secrets or trusted values? Where will these be stored / validated?
a. List at least one use of a cryptographic protocol to eliminate one vulnerability identified
in question 6, detailing its use and how it will work to overcome the vulnerability
b. Describe how cryptographic keys will be established, including details on any protocols
used and where the keys are stored and/or validated
c. List at least one obfuscation technique to eliminate one vulnerability identified in
question 6, detailing its use and how it will work to overcome the vulnerability

a. A 3rd party can connect to the C2 port before the implant does. Standard TLS only authenticates the server to the client. Mutual TLS reqires both sides to present a certificate. A solution is to use mutual TLS that requires both sides to present a certificate. Before deployment, we can generate a CA keypair with the server certificate for C2 and the client certificate baked into the implant at build time. 

b. the current problem is that both `implant.py` and `c2.py` contain hardcoded base64-encoded RSA private keys directly in source code. Anyone who recovers the implant binary gets the private key and can decrypt recorded traffic. 



c. Obfuscation: masquerading implant path. 
Right now the implant is written to the hardcoded path `/dev/shm/Agenda_3-31-26`. A defender running `ls /dev/shm` immediately finds it. 
At build time, the attacker can generate a randomized filename that mimics a legitimate system process name. The name is then compiled into the loader as a `#define` and into the implant via the `config.py` pattern already in use. 

One thing we could improve on is to use `memfd_create` instead of `/dev/shm` to run the implant as an anonymous file descriptor with no path in the filesystem at all. 


