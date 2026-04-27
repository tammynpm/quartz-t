---
draft: false
tag: [applied-crypto, crypto]
---
indistinguishability under chosen-plaintext attacks 

Indistinguishability means that when two plaintexts of equal length are encrypted under the same key, the resulting ciphertexts are computationally indistinguishable. In other words, an attacker should not be able to tell which ciphertext corresponds to which plaintext, even with chosen plaintext access.

in plain terms, an attacker cannot distinguish between the encryptions of 2 different messages even if they can choose the plaintexts to be encrypted 
what does this says about the encryption? --> the encryption must be probabilistic (randomized) -> same plaintext encrypts to different ciphertexts each time.

aka semantic security (??? needs verify)
### why IND-CPA matters?
- basic requirement weaker than IND-CCA2 (chosen-ciphertext attack)
- protects against an attacker who can also ask for the decryption of modified ciphertexts
- ensures an adverssary cannot gain any info about a message from **its ciphertext** --> confidentiality even when the adversary can influence the encryption process


### the game
- adversary chooses 2 messages M0, M1 sends to a challneger
- challenger flips a fair coin > encrypts on e M_b > sends ciphertext back
- adversary wins if they can guess which message was encrypted with a probability higher than 50% (random guessing) 

## LR oracle 

the experiment:
1. adversary can query encryption oracle $E_K(\cdot)$
2. adversary sends 2 equal length messages $M_{0}, M_1$
3. challenger picks random $b$
4. challenger returns $C*=E_K(M_b)$
5. adversary outputs guess $b'$

advantage: $Adv = |Pr[b=b']-1/2|$

to break IND-CPA, we need advantage **noticeable larger than 0**


weaknesses: 
1. deterministic encryption

2. part of ciphertext leaks information
3. message-dependent AES input 


strategy options




template
```text
choose some message M
query encryption oracle to obtain C
construct challenge messages M0, M1 using information from C[0]
receive challenge ciphertext C*
check a relation between blocks
output guess
```

goal: produce a test that reveals b 


## how to show a symmetric key encryption is not IND-CPA? 




