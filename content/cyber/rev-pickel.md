---
title: rev-pickel
tags: [python]
draft: false
date: 2026-04-24
---

`pickle` ctf challenges --> insecure deserialization --> takes 

RCE -> use the `__reduce__` method in a crafted python class 

identify pickle usage: 


subclass Unpickler --> create a snadboxed interpreter 
over

pckle.pkl: serialized program that gets deserialized, executed in a sandboxed environment. 

pickle.pkl file contains a validation program that checks flag input -> extract, analyze the nested pickles embedded in the binary data to understand the validation constraints 


`BINBYTES` part of the binary format that makes pickle non-human readable, effective for serializing python specific types 
BINBYTES: opcode used in binary serialization protocols to store bytes 
- reads a length from the pickle stream, reads next n bytes from the stream, pushes that byte string onto the  stack 


data 
pickled = pickle.dumps(data)
pickled_again = pickle.dumps(pickled)

to get original data back: 
inner_pickle = pickle.loads(pickled_again)
data=pickle.loads(inner_pickle)


pickle: high-level serialization format, not compiled binary 
python's pickletools module is the disassembler



pickle.pkl is a black box validator 
