---
title: block cipher
tags: []
draft: false
date: 2026-03-26
---
block cipher vs stream cipher

symmetric encryption algorithms: 
- encrypting info in chunks
- encrypting info bit by bit


|                    | block cipher                                                                      | stream cihper                                                                                                                                       |
| ------------------ | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
|                    | encrypt in chunks                                                                 | encrypt bit by bit                                                                                                                                  |
|                    | plaintext --> fixed-size blocks --> convert to ciphertext using a key             | a plaintext --> single bits --> convert to ciphertext with key bits                                                                                 |
| usage              |                                                                                   | SSL/TLS connections, bluetooth, cellular and 4G                                                                                                     |
| types              |                                                                                   | 2 types: <br>synchronous stream ciphers (key auto-key, KAK)<br>self-synchronizing stream ciphers (async stream ciphers, ciphertext autokey or CTAK) |
| modes of operation | some can operate as stream ciphers --> b/c of some modes operation: CFB, OFB, CTR | cannot operate as block ciphers                                                                                                                     |


how does game theory play a role in cryptanalysis? 