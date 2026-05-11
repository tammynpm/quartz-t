---
title: bluetooth
tags: [bluetooth]
draft: false
date: 2026-04-28
---

wireless personal area networks (WPANs) aka piconets (coverage < 10m diameter, no more than 8 dvices per piconet)
- 1 central role
- 7 peripheral roles 

operates in unlicenseced ISM(industrial, scientific, medical) band: 2.4Ghz -> a lot of interference
BT device has unique 48-bit address

wireless ad hoc netowrk: no APs 

-> the need for bluetooth 




- is packed with many link-level networking technqieus: 
	- [time division multiplexing](TDM.md)
	- frequestion division multiplexing
	- randomized backoff


channel allocation technqiues: frequency hoping -> according to patterns 


bluetooth stack 


classic came before ble 
- no layer 4 or layer 3
- no routing in classic BT(ble has managed flooding -> multiple devices in a bluetooth network )


bluetooth packets


### HCI packets : host controller interface packets 
- host controller exchange HCI packets across internal interface 


L2CAP similar to the transport layer 
- createa of end-end L2CAP channels btw 2 devices  (end-end network == no storage capabilities)
	- channes for E2E application/user data
	- reliability 1 bit (se number, acknowledgement) flow control, segmentation

channels types:
- connection oritented
- connectionless
- synchronous 



is the sequence number here 1 bit ? same as tcp? (channel utilization)
- ack0, ack1 
- when multiple packets are sent out simultaneously 



### bt link layer
- link manager
	- create, modify, release logical links
	- update PHY link parameters btw evices
- baseband resoruce manager
	- 
- link controller
	- encoding/ decodeing(up_ bt packets from data payload)

wireless channel
	2.4-2.5 GHz ISM radio band
	- classic bt: 79 channles/frequencies
	- ble: 40 channels
- BT channel: TDM, 625 $\mu sec$ slot length 
- channel access via  --> no collision btw networks -> no collision btw bt devices 
- frequency hopping 
	- spread spectrum 
	- stems from military's idea
	- is there retransmission ????????
	- why 4g,5g not use frequency hopping???
- recovery mechanism at a higher layer 



polling
- central sends packet to peripheral, peripheral responds
	- except 
- interaction style : 


creating BT network: 



ad-hoc network: 
- 

scheduler? 

5g fsm? 