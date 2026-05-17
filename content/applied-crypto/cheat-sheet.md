---
title: Untitled
tags: []
draft: true
date: 2026-05-10
---

how is int-ctxt different to uf-cma security?


uf-cma unforgeability under chosen message attack =>for MACs => adversary has access to a tagging oracle, wins if it produces a valid pair (message,tag) that it never queried. 

int-ctxt integrity of ciphertexts => for authenticated encryption schemes => adversary has access to an encryption oracle, wins if it produces a valid cipheretext that was never output by the encryption oracle. 


