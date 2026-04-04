---
title: Untitled
tags: []
draft: true
date: 2026-03-31
---



```bash
gcloud compute firewall-rules create deny-all-egress --network=VC --direction=EGRESS --action=DENY --rules=all --destination-ranges=0.0.0.0/0 --priority=65534 --description="deny all outbound traffic" 
```


- write script that simulates tens of sessions at the same time. 



- windows VM with forensics tools installed on it. 


ubuntu VM with 100 isolated sessions logon to the VM with read only data
the tools will be implemented in binaries form 

how is the artifacts going to be presented on the VM?


- allow what kind of traffic? tdp 



too limit bandwidth per user session must implemnet traffic shaping tools within os of the VM 
(tc for linux, windows group policy)

- identify the network interface
- create root queueing discipline
- create classes to define bnadwidth limits
- use iptables or cgroups to mark packets from a specific user and assign them to a class 