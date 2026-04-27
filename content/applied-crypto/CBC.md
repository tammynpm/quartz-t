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