![[Pasted image 20260309173751.png]]

fundamentally all system have weaknesses



stuxnet was expensive to build



capstone: demonstrate end-end exploit, complete with command and control and execution 


3 step 
- understand the systems you are attacking
- understand the vulnerabilities that exist
- determine how to achieve the effect / execute the vuln


tools: CVE, NVD, CWE, metasploit (shell loaded with working exploits, let you play with them, understand how tehy work, test systems using metasploit)

building the exploit: what you want to attack, how you want to attack 



interesting possibilities: 


one exploit can't do everything
very few exploits are self contained usually there is a chain of exploits, each one takes an action that enables the next one

![[Pasted image 20260309180153.png]]

![[Pasted image 20260309181202.png]]

how t get data from system A to B via the internet? raw socket programming 
easy to be detected, send data over http 


ways communicate that dont involve internet: fan modultaion in the lab to exfiltrate data, near field communication, much harder block than internet protocol 

command language: defines the contracts btw C2 and implant 


reduce fingerprint: if see suspicious strings, binaries, 

obsfucation techniques: data masking, false flag, code/name obsfucation (add complexity)



Tear down: exploits should be transient

implant by definition is something that should not be there


