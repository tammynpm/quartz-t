---
title: Untitled
tags: []
draft: true
date: 2026-05-04
---
determine largest possible packet size (max transmission unit) 
- packets split into smaller pieces -> more likely to loss -> packets should be kept within the mtu size
- avoids IP fragmentation -> set DF flag in IP header -> router has to send ICMP "fragmentation needed" messages if packets are too large

- usually 1500 bytes for ethernet 


safe UDP size: recommended below 508 or 576 bytes or to use path MTU rdiscoverty 