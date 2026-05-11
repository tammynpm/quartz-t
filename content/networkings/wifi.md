---
title: Untitled
tags: [wifi, rf]
draft: true
date: 2026-04-27
---
aka wireless lan 

physical layer

link layer

wifi data link layer (2)

- MAC sublayer -> manages how devices access the crowded airwaves
	- [CSMA/CA](CSMA.md) -> instead of collision detection like Ethernet, Wifi uses avoidance to reduce collisions
	- MAC addressing -> unqieu hardware addresses to ensure data reaches the correct device
	- ACK/Retransmission -> __wireless is prone to errors__ --> MAC layer sends acknowledgements for received packets + requests retransmissions if necessary 
- Logical Link Control sublayer (LLC) --> interface btw MAC sublayer nad upper-layer protocols (like IP )