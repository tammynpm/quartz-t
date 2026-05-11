---
title: time-division multiplexing
tags: [tdm, wireless]
draft: true
date: 2026-04-28
---
purpose: enable multiple signals / multi data streams -> share 1 single communication channel efficiently 

use case: digital transmission, optical fiber systems, single optical fiber links, often with dense wavelength division multiplexing (DWDM) in optical networks 

- in time domain multiplexing -> signals operate on the same frequency + same transmission medium but trnasmitted sequentially using assinged time slots in a repeating sequence 
- time slots allocated for each input signal/data stream -> simultanous tranmission in a logical sense while avoiding interference. 


### synchronous TDM, asynchronous
-> determines hwo to allocate time lsots 

- synchronous -> assgns fixed time slots to each input channel __regardless of activity 
- statistical TDM __dynamically__ alloates time slots __based on demand


### comparision with [FDM](fdm.md) 
TDM keep all signals on the same carrier frequency 
