---
title: Untitled 3
tags: []
draft: true
date: 2026-04-09
---


user-after-free vuln --> memory safety flaw occurs when the kernel continues to use a pointer to a memory location after that memory location has been freed. --> allows attackers to trigger a privilege escalation 


workflow
race conditions within subsystems > a kernel object is freed > pointer to the memory location is not cleared or set to NULL leaving a dangling pointer > attacker triggers another kernel action that re-allocates the same memory area for a different purpose > dangling pointer is used to read or modify the new data 


heap spraying 


