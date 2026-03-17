#applied-crypto #crypto 
what is it? indistinguishability under chosen-plaintext attacks 

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




