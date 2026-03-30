---
title: hash function
tags: []
draft: false
date: 2026-03-26
---
hash function breaks more often than block ciphers. 

collisions happen because of the pingeon principle but hard to find 

### formalism 


how to build a hash function from a block cipher? 


keyless hash functions : if keys = $\{\epsilon\}$ consists of just empty string. $H_\epsilon(x)$ or $H(\epsilon, x)$
MD, SH2, SH3 are keyless

any block cipher is vulnerable to eks 
any hash function is 

birthday attack 
$Adv_H^{cr}(Aq) \geq 0.3 \cdot \frac{q(q-1)}{2^n}$

if throw 


if you have a finite set, what kind of distribution to minimize collision? 


for sha256, output 256 bits, if pick 128 bit input, 
not likely to be collision resistant (?) 

#### attack times 

### compression functions
keyless function 


merkle damgard is a  common tway to turn hash function to collision resistent compression fucntion (?)


#### MD transform
have a compression unction with block length b. $h:\{0,1\}$
think of D as unbounded. D: set of all strings at most $2^b$-1
 blocks
 
> if h is CR, then so is H 
> hashing long inputs redcued to hasing fixed-length inputs 

H is secure asummming h is secure. 



drawback: sequential blocks, can
--> merkle tree 


quiz on hash function 


