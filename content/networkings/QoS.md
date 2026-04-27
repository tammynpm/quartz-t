---
title: QoS
tags: []
draft: true
date: 2026-04-21
---
#private5G

quality of service in wireless networks, slicing, private 5G

approaches
- over-provision resources
- allocate resources, QoS-aware


[circuit switching](circuit-switching.md)
## QoS in 802.11
qos sensitive but no guarantees
### sensitivity: 3 bit field QoS added to frames
- higher priority frames --> MAC scheduled at a given node before lower prority frames
- CSMA mechanism generalized --> higher priority frames awaiting transmission acoss wlan statistically more likely to be transmitte dthan lower priority frames

end-end QoS sensitivity: 
- end-end netowrk layer QoS (differentiated services) --> maps to local WLAN QoS

[CSMA](CSMA.md)

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
