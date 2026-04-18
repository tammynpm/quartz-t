---
title: Untitled
tags: []
draft: true
date: 2026-04-07
---
## mobility principles 

how ISP know which packets to route to where, 
what if an ISP cover a wider range than another ISP?
--> always route to the most specific header packet 


everytime a mobile moves, a gnb(?) has to send out advertisement that it has this mobile (?) 
this is not sustainable: when move from 1 carrier ot another carrier, all routers have to know that --> not scalable, the amount of states have to keep --> network will not handle it

--> end-system handles: at the "edge"
- indirect routing: communication from correspondent to mobile goes through home network --> then packets go to whereever the mobile is remote
- direct routing: get the address, send directly to the mobile 

### home network, visited network
- SIM card: global identify info including home network
	- hoemnetwork: paid service plan wtih cellular provider
	- homenetwork HSS stores identify & services info
	- whenever mobile is attached to another network (roaming or visited network) --> any network other than home network, some business relationships with home network, home network paid the visited network to let mobile join the visited network 
	- we dont have this business on the internet
- isp/wifi: no notion of global "home"
	- creds from ISP stored on device or with user
	- ISPs may have national, international 



- the handoff between wifi and cellular network 
	- depends on home network


-  mobility with indirect routing
	- traingle routing --> inefficient, mobile in the same network, as 1 mobilel moves from 1 network to other networ --> mobility is transparent to the correspondent --> TCP connection can be maintained
- mobility with direct routing: 
	- mobility *not transparent t*o the correpondnet 

### mobility in a wifi 

- layer 2 ONLY NETWOKR 
- 802.11 takes link-layer aproach towards mobility 
- extended service set (ESS) --> allows mobility between BSSs in SAME ESS --> fast reauthentication, AP can suggest new APs to device 

### mobility in 5G
- major mobility tasks
	1. association: establish communcation with gNB, AMF to join the network, identifying itself
	2. control-plane configuration: AMF go through gateway that allows 5G core to talk to another 5G core --> AMF .... (hand wavy) --> to establish control-plane state: 
	3. data plane configuration: AMF, SMF configures forwarding tunnels
	4. mobile handover 
		1. initiated by the curent BS --> sends handover request message to taret BS
		2. target BS pre allocates radio time slots, responds with HR ACK with info for mobile
		3. source BS informs mobile of new BS
		4. source BS stops sending datagrams to mobile, instead forwards to new BS
why handover?? 
- stronger signal from target base station
- performnace reasons 
- 