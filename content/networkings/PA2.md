---
title: Untitled
tags: []
draft: true
date: 2026-04-04
---
## part 1

**Question 1:** In a previous lab, we learned that the UE establishes a PDU session with the SMF and is provided with an IP address that it uses to communicate with the outside world via the UPF. What is this IP address? To find the PDU IP address, you can SSH (from the o-ran-in-a-box command line) into the Kubernetes UE pod, where in the command below, ### is obtained from the listing from the “kubectl get pods -A” command you did above.
Here’s the command to ssh into the UE:
kubectl exec -it oai-nr-### -- /bin/bash
Then run the ifconfig command from the command line in the UE and look at the IP
address under the “oaitun_ue1” network interface. Remember, you are executing a
command line in the emulated UE in a real 5G network!
You may also find it interesting to look up what kubectl exec does in Kubernetes.

**answer** the IP address UE uses: `12.1.1.2`

```shell
o-ran-in-a-box@ip-172-31-0-150:~$ cat /usr/local/bin/o-ran-connectivity-test
#!/bin/bash

NAMESPACE="blueprint"
TARGET_IP="12.1.1.1"
PARTIAL_NAME="nr-ue"

# Find the matching pod name
POD_NAME=$(kubectl get pods -n "$NAMESPACE" --no-headers | grep "$PARTIAL_NAME" | awk '{print $1}' | head -n 1)

# Check if the pod was found
if [ -z "$POD_NAME" ]; then
  echo "No pod found in namespace '$NAMESPACE' with name containing '$PARTIAL_NAME'"
  exit 1
fi

echo "Executing ping from pod: $POD_NAME"
echo

# Run the ping command
kubectl exec -n "$NAMESPACE" "$POD_NAME" -- ping -c 4 "$TARGET_IP"

o-ran-in-a-box@ip-172-31-0-150:~$ 
```
![[Pasted image 20260404041309.png]]
```
o-ran-in-a-box@ip-172-31-0-150:~$ o-ran-connectivity-test
Executing ping from pod: oai-nr-ue-7c6bf5fd7d-hrnxx

Defaulted container "nr-ue" out of: nr-ue, nr-ue-init (init)
PING 12.1.1.1 (12.1.1.1) 56(84) bytes of data.
64 bytes from 12.1.1.1: icmp_seq=1 ttl=64 time=29.2 ms
64 bytes from 12.1.1.1: icmp_seq=2 ttl=64 time=26.5 ms
64 bytes from 12.1.1.1: icmp_seq=3 ttl=64 time=26.5 ms
64 bytes from 12.1.1.1: icmp_seq=4 ttl=64 time=25.6 ms

--- 12.1.1.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 25.620/26.940/29.178/1.339 ms
o-ran-in-a-box@ip-172-31-0-150:~$ 
```

**Question 2:** Based on the terminal output, who (the FlexRIC controller or the gNB) receives an E2 SETUP-REQUEST to initiate control of the FlexRIC over the E2 nodes?

**Answer**
the FlexRIC controller receives the E2 set up request.  
The FlexRIC controller receives the E2 SETUP-REQUEST. The gNB (as the E2 node) initiates the setup by sending the request to the FlexRIC, which then accepts and registers the gNB's supported RAN functions.
![[Pasted image 20260404033230.png]]


**Question 3:** What is the type of this E2 node, as declared in this E2 Setup Request message?

**answer:** type: ngran_gNB

**Question 4:** Based on the terminal output, what are the two service models supported by this gNB (out of the four initial O-RAN-standard service models)?

**answer:** two service models supported by this gNB: KPM and RC
## part 4

Question 5: Now, attach a screenshot below of your terminal output that shows the name of
the metrics alongside its value over at least 3 reports. Your output should look something
like the output above, just with two additional measurements reported per indication.

show the modified xApp reporting 3 metrics RRU.PrbTotD1, DRB.UEThpD1, DRB.UEThpU1 

![[Pasted image 20260413231233.png]]
![[Pasted image 20260413231307.png]]

Question 6: On any given indication, what was the value of the UE throughput on the downlink when you ran the updated python xApp?

the value of UE throughput is 0 based on the metric `DRB.UEThpDl=0.00`.

Question 7: Since there was no traffic on the UE, we couldn’t observe anything interesting
when measuring the UE throughput. Now, generate traffic on the UE. First, SSH into the UE
container using the same command as in Question 1. Then, ping gaia.cs.umass.edu via the
oaitun_ue1 interface (ping -I oaitun_ue1 gaia.cs.umass.edu). Attach a screenshot below
of your terminal output that shows the name of three metrics alongside its non-zero values
over at least 3 reports.

![[Pasted image 20260413232857.png]]

![[Pasted image 20260413232927.png]]


/home/player1/quartz-t/content/networkings/Pasted image 20260413231233.png


/home/player1/quartz-t/content/networkings/Pasted image 20260413231307.png

