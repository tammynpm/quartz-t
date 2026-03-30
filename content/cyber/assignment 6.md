---
tags:
  - private
---
## part 1
1. Communication protocol: message format + how requests/responses flow (brief)
	- message format: json string with the fields like `type, cmd, req_id, payload`
	- flow: controller will open one tcp connection to 127.0.0.1:8765. And for each action, controller sends one frame consist of 4 byte and obfuscated payload bytes. Implant then will read the frame, decodes the JSON, sends one obfuscated reply frmae with the same `req_id`. Controllers will wait until it receives a message with the same id as the message it sent or it will raise timeout errors. 
2. Command set: list commands and what each does
	1. REGISTER
	2. HEARTBEAT
	3. WRITE_DATA
	4. READ_DATA
	5. TIME

| command        | what it does                                                           |
| -------------- | ---------------------------------------------------------------------- |
| REGISTER<br>   | returns service name, version                                          |
| HEARTBEAT      | returns current sleep_ms                                               |
| WRITE_DATA<br> | writes base64 data to a path under the allowed directory               |
| READ_DATA<br>  | reads a file under the allowed directory only, returns base64 body<br> |
| TIME           | returns UTC timestamps                                                 |

3. describe both obfuscation methods and where they’re applied
	- convert a python dictionary into a json formatted string, encode the string as UTF-8 bytes, then base64 the bytes
	- same as first method, but instead of base64 the bytes, we XOR each byte with constant 0x5A, then Base64 the result. 
4. Weakness #1 in your design: what it is, why it matters, how you’d improve it
	- reversible encryption method. This is not encryption. Anyone can intercept the traffic and decode the full JSON --> solution: use TLS, or implement RSA or AES like Cobalt Strike.
5. Weakness #2 in your design: what it is, why it matters, how you’d improve it
	* because there is no authorizatoin check, anyone can inject a payload to the client if  the client is listening on the specified port.  --> solution: 
6. Include your source code (in a small but readable font, please)
	-  I'm sorry I couldn't change the font of this specific part to be smaller. Please accept the link to Google Drive here https://drive.google.com/drive/folders/1mohyyzf9tJPa5lf7aReridMhHSQEBarq?usp=drive_link 

## part 2
2 CVEs from NVD 

![[Pasted image 20260325003140.png]]

![[Pasted image 20260325003149.png]]

### CVE-2025-10894 Nx npm supply chain attack

1. 
	* what it does? malicious code was inserted into the Nx package and several related plugins. The tampered package was published to the npm software registry. Affected versions contain code that scans the file system, collects credentials, and posts them to Github as a repo under user's accounts. 
	* https://github.com/advisories/GHSA-cxm3-wv7p-598c affected versions of nx: 21.5.0, 20.9.0, 20.10.0, 21.6.0, 20.11.0, 21.7.0, 21.8.0, 20.12.0; affected versions of @nx/devkit, @nx/js, @nx/workspace, @nx/node: 21.5.0, 20.9.0; affected versions of @nx/eslint: 21.5.0; affected versions of @nx/key and @nx/enterprise-cloud: 3.2.0
	* vulnerability type: Embedded Malicious Code
	* impact: affected versions scan the user's file system upon installation, stealing sensitive information which is later posted to a public github repo under the user's account. 
2. how would you use it? 
	* We can install Nx via npm. Because 
3. how would you test it: 
	* high-level plan: set up a VM, install a vulnerable version of the package, execute npm install or some build commands. Then monitor system behavior including file access and network activity. 
	* evidence i would collect: 
		1. i would set up auditd for file access auditing.
		2. github activity of any commits sensitive data
### CVE-2025-9074
1. 
	- what it does?  Docker Escape on Windows Docker Desktop
		- affected product: Windows & macOS especially Windows with WSL2 backend
	-  how would you use it? 
	- vulnerability type: Exposure of Resource to Wrong Sphere
	- impact:  attackers can have full control of the docker containers and can backdoor by mounting the volumes which then gives them read/write permission to the database. on Windows, attackers can mount the entire C:\ drive and escalate privilege. 
2. how would you use it? 
	* high-level purpose: 
		* an attacker would start from a compromised container, 
		* send API requests to the exposed docker engine endpoint
		* use those privileges to launch new containers with elevated access, or inject malicious payloads to persistent volumes. 
3. how would you test it: 
	* high-level plan: 
		* set up a windows VM running Docker Desktop
		* connect to the Docker Engine API (192.168.65.7:2375) from within the container
		* observe if the API is accessible without authentication, new containers can be created, or host directories can be accessed. 
	* evidence i would collect:
		* network traffic from container to Docker API using tcpdump or wireshark
		* docker daemon logs to monitor if any new containers are created 
		* 