---
title: practice-answers
tags: [private]
draft: true
date: 2026-05-11
---
### problem 1. 
*Adversary $A$:*
1. Pick an arbitrary x, then query $x$ and $x \oplus 1^{128}$
2. receive output $f_{K}(x\oplus 1^{128})=AES_{K}(x\oplus 1^{128})\oplus AES_{K}((x\oplus 1^{128})\oplus 1^{128})=AES_{K}(x\oplus 1^{128})\oplus AES_{K}((x)$  
3. Return 1 if $f_{K}(x)=f_{K}(x\oplus 1^{128})$ or 0 if otherwise

*Advantage:* 
Real world: $G_{K}(x)=G_{K}(x \oplus 1^{128})$ always true so the adversary returns 1 with probability 1. 
Random world (oracle is truly random function $f$): $f_{K}(x) = f_{K}(x\oplus 1^{128})$ are independent uniform random values in $\{0,1\}^{128}$ so they are equal with probability $2^{-128}$. The adversary returns 1 with probability $2^{-128}$. 
Therefore $Adv(\cal A)=|1-2^{-128}|=1-2^{-128}$
This is negligibly close to 1 so $G$ is not a secure oracle. 

*Resource usage:* 
2 queries, 4 XOR's for both queries, 4$T_{AES}$, which are all $O(1)$ each. The running time is $O(1)$.
### problem 2.
*Adversary*. 

1. Query 1. Submit the pair $m_{L}=0^{128}||0^{128}$ and $m_{R}=0^{128}|| 0^{128}$ to receive $c'=r'||C_{1}'||C_{2}'$ where $C_{2}'=AES_{K}(0^{128}\oplus 0^{128})=AES_{K}(0^{128})$
2. Query 2. Submit the pair $m_{L}=0^{128}||0^{128}$ and $m_{R}=0^{128}|| 1^{128}$ to receive output in the format $c^*= r||C_{1}^* || C_{2}^*$. 
3. output $b'=0$ if $C_{2}^*=C_{2}'$, else $b'=1$.

*Advantage* = $Adv_{SE}^{ind-cpa}(A)=|Pr[b'=1|b=1]-Pr[b'=1|b=0]|$

when $b=1$ the oracle encrypts the right message $m_{R}=0^{128}|| 1^{128}$ so $C_{2}'=AES_{K}(0^{128})$ after the first query, and $C_{2}^*=AES_{K}(0^{128})\oplus 1^{128}$ after the second query. $C_{2}^* \neq C_{2}'$ so $A$ outputs $b' = 1$ and $Pr[b'=1|b=1]=1$.

when $b=0$ the oracle encrypts the left message $m_{L}=0^{128}|| 0^{128}$ so $C_{2}'=AES_{K}(0^{128})$ after the first query, and $C_{2}^{*}=AES_{K}(0^{128})\oplus 0^{128}=AES_{K}(0^{128})$ after the second query. $C_{2}^* = C_{2}'$ so $A$ outputs $b' = 0$ and $Pr[b'=1|b=0]=0$. Therefore $Adv(A)=|1-0|=1$.

*Resource usage*
2 queries, 1 comparison . Running time: O(1)
### problem 3. 
*Adversary $A$*
1. Pick any $M_{1}$ and $M_{1}' \neq M_{1}$
2. Pick any $M_{2}$
3. Compute  $M_2'=AES_K(M_{1})\oplus AES_K(M_{1}') \oplus M_2$
4. Output the collision $(M_{1},M_{2})$ and $(M_{1}', M_{2}')$

*Advantage*: $Adv(A \text{ wins})=1$

*Resource usages:* 2 AES evaluations, a few XORs, O(1) time.
### problem 4.
*Adversary $A$*
1. query a single-block message $M_{1}$ and receive tag $t_{1}= T_{K}(M_{1})=CBCMAC_{K}(M_{1}[1])\oplus M_{1}[1]=AES_{K}(M_{1}) \oplus M_{1}$
2. Forge $(M_{1}||t_{1}, 0^{128})$ where $M_{1}||t_{1}$ was never queried before. 

Explanation: 
So we know that if we query a single-block message $M_{i}$ then we always receive tag $t_{i}=AES_{K}(M_{i})\oplus M_{i}$. 

If we query a two-block message like $M'=A||B$, then we receive tag $AES_{K}(AES_{K}(A)\oplus B)\oplus A \oplus B$. We need to choose $A,B$ so that $AES_{K}(A) \oplus B$ equal to something we already know. Let $AES_{K}(A) \oplus B=M_{1}$, then $B=M_{1}\oplus AES_{K}(A)$ . For simplicity, choose $A=M_{1}$, then $B=M_{1}\oplus AES_{K}(M_{1})=t_{1}$. 
Compute the tag of $M'=M_{1}|| t_{1}$: $T_{K}(M')=CBCMAC_{K}(M') \oplus M_{1}\oplus t_{1}=AES_{K}(AES_{K}(M_{1})\oplus t_{1})\oplus M_{1}\oplus t_{1}=AES_{K}(M_{1})\oplus M_{1}\oplus AES_{K}(M_{1})\oplus M_{1}=AES_{K}(M)=0^{128}$. 

*advantage*: The forgery always works. We query $M_{1}$, compute $t_{1}$ and $(M_{1}||t_{1}, 0^{128})$ is always a valid forgery $\Rightarrow Adv(A) =1$.

*resource usage:* 1 query, some XOR operations and concatenation. Running time: O(1)
### problem 5
$\cal{E_{K}}(M)$$=r || C || AES_{K}(r\oplus C )$

*Adversary.* 
Query the encryption oracle on message $M=0^{128}$ to receive $(r,C,T)$.
$E_{K}(0^{128})=r||C||AES_{K}(r \oplus C)$ where $C=AES_{K}(r) \oplus 0^{128}=AES_{K}(r)$ and tag $T=AES_{K}(r\oplus C)$

The adversary need to forge new values $(r',C',T')$ that passes the verification, meaning $T'=AES_{K}(r' \oplus C')$. The two known values are $C=AES_{K}(r)$ and $T=AES_{K}(r \oplus C)$. So we need $r' \oplus C'$ to equal either $r$ or $r \oplus C$ because those are the only inputs where we know the outputs. 

case 1: $r' \oplus C' = r$ then $T'=C$. Set $r'=r\oplus C'$, $C'=C$, so $r' \oplus C'=r$, therefore $T'=AES_K(r)=C$ and forge ($r \oplus C,C, C$). This forgery verifies. 
Case 2: $r' \oplus C' = r\oplus C$. Then $T'=T$. Set $C'=C \oplus \delta$ where $\delta \neq 0$,  $r'=r\oplus C \oplus C'=r \oplus \delta$, then forge ($r\oplus \delta, C\oplus \delta, T$ ).  

In both cases, the forgeries verify. Thus, AE is not INT-CTXT secure. 

*Advantage:*
$Adv_{AE}^{int-ctxt}(A) =Pr[A \text{ wins}]$ 
The forgery always passes verification. It only fails when the forged ciphertext equals the original, meaning 

*Resource usage:* 
The adversary makes 1 encryption query (encrypting $0^{128}$), and 1 decryption query (submitting the query). Running time is $O(1)$.
### problem 6. 
A/
Since the $IV=m_{1}$, the hash is computed as  $H(M)=h(\dots h(h(m_{1},m_{2}),m_{3}),\dots), m_{k})$
For a 2-block message $M_{1} = m_{1}|| m_{2}$, its hash is $H(M_{1})=h(m_{1},m_{2})$. 
For a 3-block message $M_{2}=m_{1}||m_{2}||m_{3}$, its hash is $H(M_{2})=h(h(m_{1}, m_{2}), m_{3})$.

*Adversary A.*
- pick any 2 blocks $m_{1}, m_{2}$. Let $v=h(m_{1}, m_{2})$. 
- query $M_{1}=v||m_{3}$ and get its hash $H(M_{1})=h(v,m_3)$
- query 3 block message $M_{2}=m_{1}||m_{2}||m_{3}$ and get its hash $H(M_{2})= h(h(m_{1},m_{2}),m_{3})=h(v,m_{3})$. 
So $H(M_{1})=H(M_{2})$ when $M_{1}\neq M_{2}$. Thus, H is not collision resistant. 

B/ 
when add back MD-strengthening, the messages would be $M_{1}=v||m_{3}||pad||<2>$ for $<2>$ is the length of the 2 blocks, and $M_{2}=m_{1}||m_{2}||m_{3}||pad ||<3>$ for $<3>$ is the length of 3 blocks. 

We will prove the attack above cannot be applied in this case; thus, H is collision resistant when MD is applied. 

The MD chain for $M_{1}$ is $v_{1}=h(m_{1}, m_{2}), v_{2}=h(v_{1}, m_{3})$,  $v_{3}= h(v_{2},pad || <3>)$
The MD chain for $M_2$ is $u_{1}=h(v_{1},m_{3}), u_{2}= h(u_{1},pad || <2>)$.
Although the two chains share an intermediate step at $v_{2}=u_{1}$, the final outputs are different because of the length field $v_{3}\neq u_{2}$. Therefore, H is collision resistant when MD-strengthening is applied. 

### problem 7. 
A/ 
*Adversary.* 
Query 1. $LR(0^{128}, 0^{128})$ and receive $(\sigma_{0}, C_{1})$ where $C_{1}=AES_{K}(\sigma _{0}\oplus 0^{128})=AES_{K}(\sigma_{0})$.
So the next state is set to $\sigma_{1}=C_{1}=AES_{K}(\sigma_{0})$.
Query 2, $LR(M_{0}, M_{1})$ where $M_{0}=0^{128}$ and $M_{1}=\sigma_{0}\oplus \sigma_{1}$. 
If oracle choose to encrypt $M_{0}=0^{128}$ then the next ciphertext is $C_{2}=AES_{K}(\sigma_{1}\oplus M_{0})=AES_{K}(\sigma_{1}) \neq C_{1}$.
If oracle chooses to encrypt $M_{1}=\sigma_{0}\oplus \sigma_{1}$ then the next ciphertext is $C_{2}=AES_{K}(\sigma_{1}\oplus \sigma_{1}\oplus \sigma_{0})=AES_{K}(\sigma_0)=C_{1}$

*Advantage* $Adv(A)=Pr[b'=1|b=1]-Pr[b'=1|b=0]$.
When $b=1$, $C_{2}=C_{1}$ always true, so $Pr[b'=1|b=1]=1$.
When $b=0$, $C_{2}=C_{1}$ only when $AES_{K}(\sigma_0)=AES_{K}(\sigma_{1})$ only if $\sigma_{0}=\sigma_{1}$ which has probability of at most $\frac{1}{2^{128}}$. 
Therefore $Adv(A)=|1-\frac{1}{2^{128}}|$

*Resource usage:*
- 2 queries, 1 XOR, 1 comparison (we only count the adversary's computation)
- running time: $O(1)$

### problem 8. 

A/ 
when $M$ is a single block, then $CBCMAC_{K}(M)=AES_{K}(M)$. So $t_{1}=T_{K}(M) = CBCMAC_{K}(M ||CBCMAC_{K}(M))= CBCMAC_{K}(M || AES_{K}(M))$

We can think $M || AES_{K}(M)$ as a 2-block long message, then the above equation induces $t_{1}=AES_{K}(AES_{K}(M) \oplus AES_{K}(M))= AES_{K}(0^{128})$.

*Adversary $A$*
1. Query $T_{K}(M_{1})$ and receive tag $t_{1}=AES_{K}(0^{128})$.
2. Output the forgery $(M_{2},t_{1})$ for any $M_{2}\neq M_{1}$ where $M_{2}$ is also a single block. 
This is valid forgery since $M_{2}$ was never queried, yet $T_{K}(M_{2})=t_{1}$. Hence, $T$ is not UF-CMA secure. 

B/
$\hat{T_{K}(M)} = CBCMAC_{K}(CBCMAC_{K}(M)||M)$

using the same idea from part A, if M is a single block, then $\hat{T_{K}(M)}= CBCMAC_K (AES_{K}(M) || M)= AES_{K}(AES_{K}(AES_{K}(M)) \oplus M)$ . There is no easy cancellation. So Part A attack fails on this tag algorithm. 
