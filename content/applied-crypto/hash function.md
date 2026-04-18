---
title: hash function
tags: []
draft: false
date: 2026-03-26
---
hash function breaks more often than block ciphers. 

collisions happen because of the pingeon principle but hard to find 


### one way 
- feistel network 
- trap-door function 
- merkle-damgard hash function 
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

### inputs, outputs of hash function 

- hash functions are deterministic functions
- used to check if 2 long inputs are the same by comparing obtained smaller-size output (digest). if the digest is the same for both inputs then both inputs are equal. 

#### how hash functions affect the outputs: 
- intput x, y leads to output H(x),H(y) because H is deterministic
- it is impossible that all the inputs will have a unique output. hte space of input is incredibly large but only so many outputs that can be generated. there will be collisions. 
- 
#### collision 
if 2 different inputs lead to the same out put -->there is a collision 

#### salt 
- generated using the keyGen algorithm: choosing a random lambda bit long string 
- it is not called key because salt is visible to the adversary 
- generating the salt is equivalent to generating a table / hash function 

##### importance of salt 
- if use same salt for every user in the database and more than 1 person ahs the same password --> the adversary would know more people's passwords --> each user has unique salt --> if 2 users have same password, the value stored in teh password would be different because different salts are used with hash function. 
- if salt didn't exist, and all hte companies used hash functions to store password --> the adversary be able to identify if the user used the same passwords on different websites 
![[Pasted image 20260403163415.png|514]]


### hash function vs. cipher
- major differnce> the adversary can view the salt in Hash Function 
- 
### collision resistance
> a hash function is CR if it is HARD to compute any collision $x \neq x'$ such that $H(x) = H(x')$

> second-preimage resistance: given x, it should be HARD to compute any collision involving x. it shoudl be HARD to compute $x \neq x'$ such that $H(x) = H(x')$


### uses of hash function 
- git is named with hash function
	- hashes allow us to come up with new names where we don't have to check the system if it has been used earlier 
		- SHA-1 used by git , no longer CR
- SHA-256: no collison found yet
- companies store the hashes of users' usernames and passwords in db. so if there is a data breach, the passwords won't be leaked the adversary would have to calculate the second pre-image which is assumed to be difficult. 

#### sha-256 hashing long strings, Merkle Damgard Transformation 
- input: 512 bits
- output: 256 bits
- if input string has a fixed amount of bits then how can longer strings be represented? 
	- longer message is broken down to chunks
	- input: n+t bits , output: n bits --> split message in t-bit chunks, use copies of hash function (little h) -> pass t-bit chunk and n-bit chunk to the first copy of the hash function 
	- pass the output (n) from the first/ previous hash function and the t-bit chunk to the next function and the t-bit chunk to the next function and the t-bit chunk --> after going through all the copies the output will be H(x) where x is the concatenation of all the strings
- if h is CR then so is H(x) 
![[Pasted image 20260403164021.png|556]]

### birthday attack 
- as the  number of people increases, the probability that 2 people have the same birthday also increases. 



### formal proof template for not CR
theorem: function $H: X \rightarrow Y$ is not CR
proof: provide an efficient algorithm to find a collision. let $\cal{A}$ be an algorithm that outputs a pair (x1, x2) as: 
- describe how to select x1, how to select x2
- show that $x1 \neq x2$ and $H(x1) = H(x2)$ 
- since this algorithm runs in polynomial time, H is not CR. Q.E.D



### HMAC in practice
- HMAC-MD5 remained unbroken after MD5 was broken
- HMAC is efficient and unbroken
- CBC-MAC was not widely deployed because it is too slow
- --> instead practitioneres used heuristic constructions



*is hash function essentailly compression function ????????????????????????????????*


