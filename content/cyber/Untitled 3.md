---
title: Untitled 3
tags: []
draft: true
date: 2026-04-11
---
sandbreak that implements a toy SFI runtime. 
the runtime loads "untrusted modules" raw x8-64 shellcode submitted by the player and executes them inside a software sandbox enforced by 2 mechanisms
- an alignment verifier run at local time
- a runtime masking instruction inserted before every indirect branch

the flag is read into a buffer in the trusted runtime's memory. 
sandbox gives the module a read/write data region and a scratch stack, but the flag bufer is outside both

goal: escape the sandbox and read flag from the trusted heap

