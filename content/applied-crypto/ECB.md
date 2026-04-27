---
title: ECB
tags: [AES, ECB]
draft: false
date: 2026-04-21
---
Electronic CodeBook 
- deterministic --> encrypt same plaintext with same key yields the same ciphertext
- **isn't IND-CPA secure**


Suppose you perform an IND-CPA test on AES-ECB with the following two messages:

- **M₀:** Two blocks of all zeros.
- **M₁:** One block of all zeros followed by one block of all ones.

An attacker submits both plaintexts for encryption but receives only one ciphertext. Because ECB encrypts each block independently and deterministically, the ciphertext for M₀ will consist of two identical blocks (since both blocks of zeros produce the same ciphertext). In contrast, the ciphertext for M₁ will consist of two different blocks. By analyzing whether the two blocks in the ciphertext are identical or different, the attacker can determine which message was encrypted. This example illustrates why ECB mode does not meet the IND-CPA security criterion.

https://www.reddit.com/r/ComputerSecurity/comments/1ikexgm/indcpa_feels_counterintuitive_am_i_missing/#:~:text=Suppose%20you%20perform%20an%20IND%2DCPA%20test%20on%20AES%2DECB%20with%20the%20following%20two%20messages%3A

