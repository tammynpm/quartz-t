---
title: 5G core
tags: []
draft: false
date: 2026-03-31
---
- 4g core: control plane components, user plane, tunnneling
- 5g core: functional components, user identity, registration, session ebstablishment


- contains the network functions (NFs) 
- uses a service-based architecture (SBA) -> virtualized functions communicate via APIs:
	- AMF (access & mobility management function)
	- SMF (sesssion management fucntion)
	- UPF (user plane function)
	- AUSF (authentication server function)
	- PCF (policy control function)
	- NRF (network repository function)
	- NEF
	- UDM
	- NSSF

### vs 5g RAN 
- Radio access network, where gNB is 
- gNB links users' 5G NR device to the 5G core via the NG interface 


ip tunnel
- abstract of logical link



3G (void+ceata)
at radio network connolllter,


| 3g  | 4g       |
| --- | -------- |
|     | vendoor  |
![[Pasted image 20260505012331.png]]

![[Pasted image 20260504164049.png]]



joining a 5g network 
![[Pasted image 20260504164229.png]]


this slide is really important ![[Pasted image 20260504191006.png]]

multi tenancy ???????????????????



this shows shared functions 
single core function like AMF, NSSF
both slices have UPF in user plane, SMF, PCF, NRF in control plane 
![[Pasted image 20260504191933.png]]