---
title: Untitled
tags: []
draft: true
date: 2026-04-16
---
rectified linear unit 

non-linear activation function 
computes function $f(x) = max(0,x)$ --> outputs the input directly if it is positive, outputs 0 therwise 
- default choice for hidden layers in many convolutional neural networks CNNs and deep neural networks
- trains faster than sigmoid or tanh functions because its derivative is 0 or 1 --> mitigates the vanishing gradient problem 
- deying reLU problem: neurons can become "dead" (always outputting 0) if they become inactive such as when inputs are consistently negative 
- 