---
title: Untitled 1
tags: []
draft: true
date: 2026-04-21
---

integrity, privacy --> [IND-CPA](IND-CPA-game.md) + INT-CTXT

plain encryption deosn't provide integrity (example CBC$)

attacks on encryption with redundancy 

WEP attack 

encrypt-then-MAC --> validates tag first, always works if encryption IND-CPA secure + MAC unforgeable --> provides IND-CCA
MAC-then-encrypt --> used in SSL/TLS
encrypt-and-MAC --> used in SSH, can reveal info about plaintext


generic composition ? 
- authenticated encryption (AE) = stnadard encryption scheme (confidentiality) + message authentication code (MAC) (integrity) 
- most secure: encrypt hten mac (EtM) using separate keys for encryption & authentication 

required security: encryption should be IND-CPA security + MAC must be strongly unforgeable 
key usage: independent keys for encryption and MAC calculation 



## INT-CTXT 
integrity of text 

what the fuck am i supposed to do?


