
physical layer coding
- duplicate
- shuffling --> so duplicates are far apart to avoid when burst error happen, it would wipe out consecutive bits so if 2 copies sit next toeach other, they could be killed together

to recover: at least 1 copy must survive the burst error
failure to recover happen when a burst deletes both copies of a bit


the smallest distance between copies of any bit is 8
if a burst has length >= 8 then can erase both copies of a bit



Quadrature phase shift keying QPSK
![[Pasted image 20260226180652.png]]

10 00 10 11 00 01 



QAM 16 modulation 



multiple access protocols 
3 classes: 
- channel partitioning
- random access
- taking turns

Time division multiple access: share channel is partitioned in time 
- pros: - collisions, can be perfectly fair: each node can receive a dedicated transmission rate of R/N bps when it is allocated 1 slot per framee
- cons: node is limited to an average rate of R/N bps even when 1 node is sent
	- node must alway wait for its turn in the transmission queue




code division multiple access: 
- assigns a different code to each node
- each node uses unique code to encode the data bits
- diff nodes can transmit simultaneously 
	- have their respective receivers correctly receive a transmitter's encoded data bits inspite of interfering transmissions by other nodes




### random access protocols:

pure aloha:
