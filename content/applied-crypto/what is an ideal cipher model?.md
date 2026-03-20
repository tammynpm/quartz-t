%% what is block cipher? CBC?
ideal cipher model assumes a block cipher behaves like a perfect random permutation for every key

for each key k, the encryption function E_k: {0,1}^n --> {0,1}^n is a random permutation over n-bit blocks. 

the ideal cipher model: E_k(.) <- uniform random permutation 

similar to a giant lookup table: for each key, there is a completely random bijection between inputs and outputs



E_k(x) ~~ F_k(x)


a random function map multiple inputs to the same output while a permutation cannot. but if the block size is large like 128 bits, collisions are likely,  so the distinction doesn't matter in practice. 



an ideal cipher assumes that for every key the block cipher is a completely random permutation over the message space, and that different keys correspond to independent permutations. an adversary can query both encryption and decryption but learns nothing beyond the oracle responses. 

modeling a block cipher as a secure PRF is weaker: the cipher is assumed to behave like a random function rather than a random permutation and usually only encryption queries are considered. 

a real block cipher such as AES is not an ideal cipher, since it has a fixed deterministic structure and key schedule. AES is designed to behave like a pseudorandom permutation meaning it should be computationally indistinguishable from a random permutation for practical adversaries. 

 %%

how is an ideal cipher different from the ROM (random oracle models)?


to read: https://cs.nyu.edu/~dodis/ps/ic-ro.pdf 
