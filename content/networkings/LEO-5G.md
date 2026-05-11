---
title: LEO-5G
tags: [5G, LEO,rf]
draft: false
date: 2026-04-25
---
Low earth orbit satellites communicate directly with standard smartphones bypassing the need for traditional cell towers. 
LEO satellites move fast and are only visibile for a few minutes, 
operate in massive, connected constellations to ensure continuous service

trnasmit data to and from earth to provide internet connectivity 
- type of fixed wireless access solution requiring a satellite dish to be mounted to buildings
- 
- primarily used in remote locations that are unserved by wired internet access or cell towers
- spaceX has amassed more than 1.5 million subscribers since 2019 
- 

uses 3GPP 5G standards ([Non-Terrestrial network](NTN.md) or NTN features) --> satellites are treated asa standard base station by phones


LEO satellites vs 5G
leo: quick to deploy, high speed, low latency internet, accessible around the world
5G: ultra-low latency, high throughput, __[network slicing](network-slicing.md)

|                   | leo satellites                                                                           | 5g                                                                                                                                |
| ----------------- | ---------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| coverage          | require a clear view of sky without any obstructions                                     | can service populated areas. different types of 5g optimize performance and propagation differently<br>dense area                 |
| usage             | business continuity, part of network resilience strategy                                 | mobile, IoT                                                                                                                       |
| performance       | download speeds: 50-250 Mbps range in optimal conditions where there are no obstructions | average download speeds up to 2Gbps                                                                                               |
| capacity          |                                                                                          | virtually unlimited capacity<br>optimizing traffic: network slciing (multiple networks created on top of a common physical infra) |
| security features | lacking<br>can connect a satellite gateway to a cellular router                          |                                                                                                                                   |
| cost              |                                                                                          | cost grow as customer base grows                                                                                                  |

## combining 5G-LEO satellite? 

https://www.qorvo.com/design-hub/blog/how-modern-leo-satellite-technologies-are-changing-the-space-race

https://www.qorvo.com/design-hub/blog/advancing-communication-the-role-of-leo-satellites-in-the-wireless-expansion



[satellite networks](satellite-networks.md) 



data sensing in space
- satellites collect more data than their downlink 
- can't send down everything they sensing
- have to compute data -> then send down 

GEO: 
- geosynchronous: sationary with respoect to ground
- 800ms RTT 
- 35768 km above earth 

leo: 550-1200km above earth 
- within line of sight of ground station for 5-15mins
- 30msec RTT 
- inter-satellite links (ISL) instead of up-down bed?????




#### bluetooth cannot do storage



how do governments jam signals from LEO to their 


is there any protocol to communicate btw satellites, like in 