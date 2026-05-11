---
title: quality of service
tags: [wireless, 5g]
draft: false
date: 2026-04-21
---
#private5G

vs [network slicing](network-slicing.md)

- a rule-based system -> prioritizes certain traffic flows (likea voice call or live stream) over others on a shared network infra -> assigns a priority ID (like a 5QI in 5G) -> determine who goes first when the network is busy 
- all traffic shares same traffic -> performance can degrade severly during heavy congestion -> a surge from one large user can slow down everyone else

quality of service in wireless networks, slicing, private 5G

approaches
- over-provision resources
- allocate resources, QoS-aware


[circuit switching](circuit-switching.md)
## QoS in 802.11
qos sensitive but no guarantees
### sensitivity: 3 bit field QoS added to frames (in 802.11 frame, qos takes up 2 bytes)
- higher priority frames --> MAC scheduled at a given node before lower prority frames
- CSMA mechanism generalized --> higher priority frames awaiting transmission acoss wlan statistically more likely to be transmitte dthan lower priority frames

8 level of priorities for 802.11


end-end QoS sensitivity: 
- end-end netowrk layer QoS (differentiated services) --> maps to local WLAN QoS

[CSMA](CSMA.md)

wifi through tifi 6: qos sensitive but no guarantees (soft, hard) 
wifi 7: 
4g/5g: 
- no specific admission decision policies standardized 
- lots of qos mechanism 
- 
## QoS in 5G 

- QoS flow: unit of QoS in 5G
	- each flow carries a UE's packets with same QoS requirements 
	- 2 types of flows:
		- guaranteed bit rate
		- non-guaranteed bit rate
	- flow's QoS requirements specified by 5QI (5G QoS identifier)
	- 

hard sharing: each UE gets dedicated   
vs statistical sharing: each UE uses shared link bandwidth as needed (no hard allocation)


SLA: service level agreement btw physical resource owner/operator and "user" quantifies rsource requirements (traffic, computational loads) and extent of permissible queueing, delays, non-availability of resources 


#### 5QI
- aka 5G QoS identifier -> scalar value in 5g -> pointer to specific QoS characteristics (priority, packet delay budget, error rates) -> how data packets are handled
- 5QI table mapping: standardized 5QI values (80,82,84) -> maps to distinct performance targets including 5ms to 30ms packet delay budgets


### traffic policing --> limits traffic to not exceed declared parameters
- long term average rate: # of packets can be sent per unit time in the long run 
- peak rate: 
- burst size: max # of packets sent consecutively with no intervening idle

policing mechanism: --> limit number of packet arrivals to an average arrival rate and burstiness 
- leaky bucket policer 




## slicing, virtualization in 5G
slicing --> enables multiple independent logical (virtual) 5G networks to operate simultaneously using the same physical network infra


![[Pasted image 20260421151959.png]]




- ?????????????? why not concrete?
- oversubscription example
	- ![[Pasted image 20260421152244.png]]

### traffic policing 
