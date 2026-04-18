---
title: Untitled
tags: []
draft: true
date: 2026-03-31
---



$C_1=C_2$
then $AES_{K}(B_1[i]\oplus M_1[i])=AES_{K}(B_2[i]\oplus M_2[i])$ 
Since $C_{0}=0^{128}$ , $B[1]=AES_{K}(M[i])$, $C[1]=AES_{K}$






---

let E be a blockcipher $E:\{0,1\}^{b}\times \{0,1\}^{n} \rightarrow \{0,1\}^n$ 
compression function: 
$h(x||v) = E_x(v) \oplus v$

hwo to find a more random function? 


collision resistanat is NOT the same as reversability ? 

supposed a hash function is compressing, can pick a random input hash it, reverse it, if u can reverse it than it s a collision

all hash function has collisions (?) but whether or not you can find the collision 



---
CK: second preimag eressistantt

one message is chosen randomly  for the adversary. it has to find 

universal hashing: 

all sha hash functions 

this has been a dominant way to build hash functions: 

Blockcipher to Compression function via Davies Meyer transform 

once you have a compressed function, turn it to a hash function with Merkle Damgard


sha3 did not use merkle damguard. sha256 is merkle damgard

in the transform compression -> hash fucntion:
the IV starts at 0 for merkle damgard. the IV and chaining variable n bits. 

the blockcipher ahs large parameter
which determines the vsalue of b nad n in the compression function


if you can find collision in dam vangard hash, cna find collision in compression (?) 
