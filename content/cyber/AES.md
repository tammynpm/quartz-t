---
title: Untitled 5
tags: []
draft: true
date: 2026-04-15
---
https://braincoke.fr/blog/2020/08/the-aes-key-schedule-explained/#s-box

Realized I haven't written any post about AES. 
AES intends to replace the DES and doesn't use the Fiestel Structure like DES. 

## what? 
- a synmmetric-key algo
- aes-128: means input & output 128 bits = 16 bytes
- aes-128 consists of 10 rounds:
	- first 9 rounds have 4 stages: SubBytes, ShiftRows, MixColumns, AddRoundKey
	- 10th round: the MixColumns operation is not performed

4 functions in AES with key: 
- byte substitution
- permutation
- arithmetic operations over a finite field
- xor with a key 


[DPA](DPA&HWassumption.md)


