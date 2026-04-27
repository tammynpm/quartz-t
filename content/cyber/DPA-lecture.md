---
title: Untitled 6
tags: []
draft: true
date: 2026-04-24
---

EM or photon emission bcan be used in side channels
- when you flip the transistor it releases photon 

hamming weights used in leakage model 



partitioning 

correlation 

class distinguisher: mutual info, use ML to build up those profiles, profile attack 


DPA on a known value s--> key attack should be fairly simple 

__TODO: read paper
![[Pasted image 20260424175933.png]]


![[Pasted image 20260424180000.png]]


## prereq for DPA data
- ![[Pasted image 20260424180103.png]]
- metadata in this case an mean a lot of things for each time capture the trace, whats the ciphertext, plaintext 
- capture everything u can: temperature (big deal), time date, any input/output, 

### HDF5 

![[Pasted image 20260424180234.png]]
- store metadata along with data --> nice way to store traces 

```
ipython
import h5py
test_file = h5py.File('aes_decrypt_powertraces_test_target.hdf5')
list(test_file)

plot(np.array(test_file['data_000000]))

#list metadata
list(test_file['data_000000'].attrs)
test_file['data_0000000'].attrs['dut_input']
```

![[Pasted image 20260424180558.png]]
dataset appears similar to numpy array 

looking at metadata ![[Pasted image 20260424180625.png]]

![[Pasted image 20260424180649.png]]

dut_input looks like ciphertext, but just a hex string , can convert to bytee object ![[Pasted image 20260424180716.png]]

or list of integers (esasire to work with algorithm) ![[Pasted image 20260424180735.png]]

![[Pasted image 20260424180823.png]]

>hvaing the output is nice because you can test against it??? -- jeffrey hamalainen


#### what does it mean to do DPA on known values????
 known values here means you either know plaintext or ciphertext and try to recover the secret key 

adversary work flow: 
- you have many power traces captured during encryption/decryption of known inputs
- guess a small piece of the key (one byte -> 256 possible guesses)
- for each guess --> compute the intermediate vlaue would be at some target operation (like the s-box output): `intermediate = s_box[plaintext_byte XOR key_guess]`
- use a powermodel (hamming weight of that intermediate value) -> predict power consumption 
- correlate prediction against the actual power traces 
- key guess with the highest correlation is most likely the correct key byte 
- 

![[Pasted image 20260424180920.png]]

the python script dosn't look too different 
![[Pasted image 20260424181015.png]]

diffmeans: averages each parittion and subtracts them --> at hte correct time sample, the partitions will have differnt power consumption --> spike appears 
if wrong key guesses --> difference averages to zero/flat 

highly recommend doing this in ipython: 
![[Pasted image 20260424181237.png]]

for convenience provided sbox full values 

big thing to be used dpaCiphertext ![[Pasted image 20260424181306.png]]

after running the python script in ipyton (?) 
![[Pasted image 20260424181436.png]] we can see the 

talk about how to do aginst key bytes ![[Pasted image 20260424181746.png]]
what is key bytes? 
![[Pasted image 20260424181909.png]] the hw is taking this and convert to python 
what does the vectors nad flows in the dpa results mean: ![[Pasted image 20260424181946.png]]
what does dpa vector mean? 

which key byte is correct --> candidate produces the largest spike
when the device processes tat byte --> 


when we look at data, correlation is from -1 to 1 can have negative values, we don't care abt the size, magnitude matters 
![[Pasted image 20260424182119.png]]