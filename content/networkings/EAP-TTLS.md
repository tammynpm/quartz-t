---
title: EAP-TTLS
tags: [wifi, wireless, TLS]
draft: false
date: 2026-04-27
---
Extensible Authentication Protocol-[Tunneled Transport ](TLS.md)

tunneled transport layer security 

- can be used in both enterprise [wifi](wifi-authentication.md) + [5G](5G-authentication.md)

2 phases of 802.1x EAP-TTLS -> authentication 


arriving client authenticate the network 

phase 1: tls tunnel establishment -> sets up a secure, encrypted link using server-side certificates (handshake phase)

phase 2: inner authentication -> validates user creds (like username/password) through the established tunnel (data phase)


|         | phase 1 aka handshake phase, tls tunnel phase                                                            | phase 2 aka data phase, inner authentication                                               |
| ------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| goal    | seure connection                                                                                         | authenticate user                                                                          |
| process | client (supplicant) authenticates the authentication server using its certificate -> create tls tunnel , | client sends creds through the created secure tunnel -> server validates these credentials |
|         |                                                                                                          |                                                                                            |

weakness: 
- trust model: client must rust the authentication server's certificate -> if server's cert is compromised or issued by an untrusted or unauthorized CA 
- certificate verification: EAP-TTLS requires client to verify the server's cert during the tls handshake -> if client doens't perform proper cert validation -> MITM attacks: intercepting the communication btw the cleint and server and poses as the server to gain unauthorized access or obtain sensitive info. 
- cred vulnerabilities

is EAP-TTLS > EAP-TLS??

### EAP - extensible authentication protocol 
- authentication framework 
- not a single protocol 
- supports multiple methods for validating network users, including apsswords, digital certs, and smart cards. 
- 