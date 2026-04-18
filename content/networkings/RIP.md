---
title: routing-information-protocol
tags: []
draft: false
date: 2026-04-18
---
RIP myself. Sitting in the networking lab at 4 in the morning. I can't stop not liking this protocol. I've been doing this lab for maybe more than 3 times and keep missing screenshots. I wonder why I have to learn such an old routing protocol? Why can't I just know OSPF and BGP and call it a day? 

Its like studying history but not in a cool way, not through war stories. I blame the lab instruction for part of the reason why i'm developing hatred torward a concept. 

hours, days, weeks later, i'm still here not understanding what i do, but to rush through the end of the lab that's been the source for my mental exhaustion. And also because i don't like that you have to be in the lab physically to do this. The building is usually locked on the weekend. No undergrads can come in and neither do I. It doesn't help that the lab is so inaccessible for no good reason. 


## RIPv1
- time: late 1980s 
- only classful routing
- disadvantages: incompatible with VLSM (variable length subnet masking), relied on broadcast communication to send updates (ie all devices on teh local network received the data even if they don't need it), security concerns

## RIPv2
- time: 1990s
- support classless routing
- multicts for updates
- send data only to devices that actually needed it 
- basic autehntication feature, giving admins more control over which devices could send or accept routing updates, + 1 security layer 


## RIPng
- same core mechanism: use hop count as a metric, 
- requires a different transport mechanism & message format to it ipv6 standards 
- still lightweight, simple to implement


disadvantage: 
- scalability: 15-hop limit 
- slow convergence: when a chnage happens in the network, RIP takes time to update **all routers** --> cause delays or routing issues
- metric is not advanced: only counts hops doesn't consider bandwidth, latency, or load --> result: might choose a shorter path but much slower or unstable
- security concerns: RIPv1 have 0 authentication mechanisms --> vulnerable to malicious route updates. 
- 

[OSPF](OSPF.md)

[BGP](bgp.md)