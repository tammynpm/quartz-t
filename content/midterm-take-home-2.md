2a. Line of sight delay between the sender and receiver: $\text{delay = }\frac{1.2 \times 10^3}{3\times 10^{8}}=4\times10^{6}=4\mu \text{ sec }$
2b. The minimum coherence time. First we compute the arrival times for all paths: 

the coherence time must be long enough so that all multipath reflections from pulse N arrive before pulse N+1's LOS signal arrives. This is also the gap between the LOS arrival and the last multipath arrival of the same pulse. 

| Distance (km) | Delay ($\mu$ s) |
| ------------- | --------------- |
| 1.2           | 4               |
| 1.4           | 4.67            |
| 1.6           | 5.33            |
| 1.8           | 6.00            |
| 2.0           | 6.67            |
| 2.2           | 7.33            |
$T_{c}\geq 7.33-4=3.33 \mu$s
The minimum coherence time is 3.33 $\mu$s

2c. 
The condition for a multipath reflection to cause interference is $P_{multipath} > \frac{1}{2}P_{LOS} \Rightarrow \frac{1}{d_{mp}^{2}}> \frac{1}{2}\frac{1}{d_{LOS}^{2}}\Rightarrow d_{mp}^{2}< 2d_{LOS}^{2}= 2(1.2^{2})=2.88\text{km}^{2}\Rightarrow d_{mp} < \sqrt{2.88} \approx 1.697 \text{km}$
With path loss, the reflections from objects at 1.8, 2.0, 2.2 km are less than half the power of the LOS signal and can be treated as noise. Only the echoes at 1.4km and 1.6km are strong enough to interfere. 

The last interfering multipath signal travels 1.6km. Apply the same logic as 2b: $$T_{c}\geq 5.33-4=1.33 \mu \text{s}$$
