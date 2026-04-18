---
title: Untitled 1
tags: []
draft: true
date: 2026-04-06
---
i have seen this so many times. probably the most famous method to encode data in an image. 

explain a little bit why LSB is unreliable on jpg files. 
unlike PNG, JPG use lossy compression. While you can embed data in the pixel bits, the JPG compression process alters those bits, likely destroying the hidden information. For reliable LSB hiding, lossless formats like PNG or BMP are better preferred. 

https://medium.com/@andrew.petrus/hiding-in-plain-sight-lsb-steganography-7f451ff4c191
This article seems to explain well how to implement LSB on a PNG file but i haven't read. 

a few methods that work on JPG: 
- hide data in the frequency domain (DCT coefficients) before entropy encoding, rather than directly in the pixel's spatial LSB
- cosine-transform 

jfif (jpeg) compression
- 

the art of analyzing steganography is called steganalysis. 


dct coefficients 
discrete cosine transform 

### jpeg quantization-distribution steganalytic method against jsteg


### reversible discrete cosine transform 
### lossy vs lossless compression
