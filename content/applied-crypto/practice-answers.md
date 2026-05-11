---
title: Untitled
tags: []
draft: true
date: 2026-05-11
---


problem 1. 
Adversary $A$:
1. Pick a input $x \oplus 1^{128}$
2. query the oracle with this input 
3. receive output $f_{K}(x\oplus 1^{128})=AES_{K}(x\oplus 1^{128})\oplus AES_{K}((x\oplus 1^{128})\oplus 1^{128})$  
4. Return 1 if $f_{K}(x)=f_{K}(x\oplus 1^{128})$ or 0 if otherwise

Advantage: 
$Adv(\cal A)=1$

Resource usage: 
2 queries, 4 XOR's for both queries, 4$T_{AES}$, which are all $O(1)$ each. The running time is $O(1)$.

problem 2. 



problem 3. 