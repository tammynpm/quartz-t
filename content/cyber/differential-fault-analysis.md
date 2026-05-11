---
title: DFA
tags: [DFA, cyber,MITRE]
draft: true
date: 2026-04-27
---
program flow fault targets
- password checking 
- privilege / permission checks
- firmware signing checks (e.g xbox firmware validation attack )

mmeory dump via loop count underflow 



- dfa is like a buffer overflow /stack smash /rop chain/ nop sled for fault attacks 


### dfa on aes 
common pattern:
- proosing countermeasures 
- attack countermeasures? 
- reduce @ of faults
- reduce offline work
- reduce math, theory 
- relax the fault model 



doing encryption multiple times --> increatse side channel leakages 

### faulting key addition 
- what is some fault effects of aes round key 
	- corrupt i init ()
	- corrupt state[i] ^ roundkey[i] calculation
	- skip state[i] update
	- corrupt i calculation / update
	- fault i < 16 checks
	- function skipped altogether 



which key addition within aes would be most interesting for an attackto cause the fault: 
0

confusion and defusion 

fault exploitation 


## remote implementation security 
- remote implementation attack 
	- trigger impleentaton -vuln via remoe interface
		- timing side channel against openssl 
- side 



- cheating at CS
	- leaking side-channel info base on rendering hiddne figure 
	- volP 
	- leakage picked up by microphone's amplifier
	- 


bitsquatting = registering domains that are one bit different from a popular domain  -> undetected bit errors cause a connection to the invalid domain -> setting a trap 
- it's typosquatting but for computer typos



## semi-invasive attacks 
