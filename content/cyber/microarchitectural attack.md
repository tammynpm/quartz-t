---
title: Untitled
tags: []
draft: true
date: 2026-03-29
---

microarchitectural side channels

how ASLR work? 
- applied as a random constant $\Delta$ added to every virtual page number in the program's address space. 
- different $\Delta$ for different memory parts. ($\Delta_{stack}, \Delta_{heap}, \Delta_{code}$)
- distnaces are preserved under ASLR --> only need one leaked pointer to a memory part is sufficient to find other parts 
