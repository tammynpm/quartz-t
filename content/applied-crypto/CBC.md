---
title: CBC
tags: [AES, CBC]
draft: true
date: 2026-04-21
---
Cipher BlockChaining 
with a random Initialization Vector (IV) random + unpredictable,  used on first block 
- prevents distinguishability 
- each time encryption runs, a new IV is generated 
- ciphertext of previous block XOR with current plain block --> current ciphertext block 
- ciphertext different every time under same key same plaintext

the IV generated new + randomized makes this IND-CPA secure 

of course to understand more about CBC mode, we have to know about the other mode of AES implementation [ECB](ECB.md)


![[Pasted image 20260510152417.png]]

## CBC-MAC

![[Pasted image 20260510152924.png]]



3 main techniques to construct a MAC (message authentication code)
- cbc-mac (cipher block chaining mac)
- hmac (hash based mac) 
- cmac (cipher based mac )


|     | cbc-mac                                               | hmac                                                                 | cmac |
| --- | ----------------------------------------------------- | -------------------------------------------------------------------- | ---- |
|     | using a block cipher in cbc mode                      | calculate a MAC involving a cryptographic hash function + secret key |      |
|     | final cipheretxt block is used as hte mac (final tag) | $HMAC(K,m)=H((K'\oplus opad) \|\|H((K'\oplus ipad) \|\|m))$          |      |
|     | secure only for fixed length                          |                                                                      |      |
