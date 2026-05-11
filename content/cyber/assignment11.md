---
title: Untitled 7
tags: [private]
draft: true
date: 2026-04-24
---
#### CS564 - assignment 11 
Due 5:30PM Apr 29, 2026
Tammy Nguyen

1/ 
1.1/ AES key is `5e451b4ffd7878180c6ee2b54e549cbe`
1.2/ round 10 subkey `0xed3285639d69654fbd119a0d8b10a80d`

1.3/  round 9 subkey `0x12a4d686ee88c8487c8aa8bdf3a8253c`

1.4/ 

1.4.1/ if perform the DPA over the entire power trace without using trim, I expect key hits to occur around 2000-4000 sample indices because the peaks at the beginning correspond to round 10. 

1.4.2/ 
`trace = trace[:trim]` means the values are kept from sample 0 up to the trim value. 
A good trim value is around 5000-6000 to isolate the first decryption round only and avoid noise from round 9. 
Setting trim value to be 6000 in the python script: 
`result = dpaCiphertext("aes_decrypt_powertraces_test_target.hdf5", trim = 6000)`

1.4.3/ 
In the original version, the peaks are around positions 2000 - 4000. In the trimmed version, the peaks are at around the same positions because the trim only cuts off the end. The trimmed version shows cleaner peaks. 

1.5 / Having the data file with a known key helps because now we can verify that the DPA code correctly recovers the known key before applying it to the unknown-key dataset. It also helps with debugging before handling the real target.


2.1/ 
round10 subkey is:  `0xb52e5c8314dc8a4e3ebe5e893e580e8b`

2.2/ full AES key: `00003141592653589793238462643383`

2.3/ To validate the key, I ran the DPA attack on the test file for the test data from `aes_decrypt_powertraces_test_file.hdf5`. The recovered key matched the `dut_key` value stored in metadata. This means the attack method is correct. 


3/ 
- To speed up the DPA attak we can reuse the trim range. For example, in the first question, as we know that round 10 happens in roughly 6000 samples. Since it is the same hardware, the timing will roughly be the same.  Hence, trimming down from around 60000 to 6000 speeds up 10 times as we have 10 times fewer data points. 

4/ 
4.1/ potential sources of noise in a power trace: 
- noise from the hardware setup that is used to collect the traces 
- noise from the target hardware's clock.
4.2/ 
- the electrical noise can reduce the clarity of DPA peaks. Because it is random, so  can average out traces. 
- if the target's clock cycles are inconsistent, the same operation might not line up at the same index across all traces, and the correlation gets smeared out and the peak is weaker.   
4.3/ 
- trimming removes noise from other rounds. More irrelevant data is trimmed off to avoid false peaks. 
- trace alignment to match traces ensuring the receiver can accurately sample the data and reducing the deterministic jitters.
- 
4.4/ 
When multiple DPA byte hits overlap at the same sample indices, it means the hardware is processing multiple bytes simultaneously at the point in time. 
The power consumption at the sample where multiple bytes are processed simultaneously creates noise. This makes the attack harder because the DPA peak for a byte is weaker because it is averaged out by the other bytes. This results in guessing wrong key or more traces to separate the byte's contribution from the overlapping bytes.

5/ 
- before processing, we can XOR the sensitive data with a random value. This method is called masking. The computations are done on the masked data so the power consumption is randomized and uncorrelated wtih the actual key. We remove the mask to get the result. 
- We can randomizes the order in which bytes are processed. This spreads each byte's DPA signal across different sample indices, smearing out the peaks and making them harder to be detected. This method is also known as the shuffling method. 

