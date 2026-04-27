---
title: Untitled 5
tags: [private]
draft: true
date: 2026-04-16
---
1/ Examples:
	1. mirrors that display weather and news. New features include real-time weather nad news dashoards, facial recognition, and voice integration like Alex,etc. 
	2. smart trash cans: smart features include automatic bag reordering through services like amazon dash and gesture-controlled lid opening. 
	3. dishwashers with wi-fi: smart features include remote start and cycle scheduling, real-time alerts for cycle completion and automatic detergent reordering. 

2/ potential attack targets

|                   | potential attacks   | goals for the adversary                                                                                                                          |
| ----------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| smart mirrors     | Surveillance        | An external attacker could exploit the always-on camera and microphone to spy on household occupants, capturing video, audio, or daily routines. |
| smart trashcans   | Physical disruption | the fill sensor can be spoofed to flood the user with unwanted deliveries.                                                                       |
| smart dishwashers | physical damage     | an attacker could remotely start the dishwasher risking water or power damage.                                                                   |


3/ new attack surfaces that have emerged because of the 'smart' transition
- smart mirror: 
	- User profile system: Facial recognition and personalized profiles introduce a biometric data store that did not exist on a traditional mirror.
	- voice assistant integration (same attack surface as amazon echo) is vulnerable to voice spoofing, command injection via ultrasonic frequencies, and API abuse.
- smart trash can: 
	- bluetooth/wifi connectivity for fill-level reporting and bag ordering: Any unauthenticated or weakly authenticated wireless interface is a potential entry point for an attacker within radio range.
	- motion/gesture sensor: Could be spoofed to trigger lid opening or falsely report fill levels.
- smart dishwasher
	- Shared home network exposure: Once on the local WiFi network, the dishwasher may be reachable by other compromised devices.
	
4/ How the devices respond to the attacks. 
- smart mirror: 
	- alert the user via push notification if an unrecognized attempts to access a personalized profile
	- require re-authentication for any changes 
- smart trashcan:
	- notify the user via app if an auto-order is triggered
	- require in-app confirmation before processing any purchase orders 
	- alert the user if the device receives unexpected firmware updates
- smart dishwasher:
	- require physical confirmation (press button) before executing a remotely initiated start command
	- send real-time push alerts for any remote start or cycle change commands
	- automatically disable remote start after a set number of failed authentication attempts on the app

5a/ 
seed = "theodoor"

5b/ 

Using seed "theodoor", I requested power traces varying the first digit (0000–9000). The trace for 9000 was noticeably longer than all others, confirming 9 as the first digit. Repeating for position 2 with 9000–9900, then position 3 with 9000–9090, then position 4 with 9000–9009, each correct digit produced a distinctly longer trace. The final confirmation came when 9001 returned "Password correct".
Password: 9001

http://woodbad.pythonanywhere.com/passworddiversify?value=39303031&seed=7468656f646f6f72 

5c/ 
Algorithm:
- Fix all password positions to '0' as a baseline.
- For position i (starting at position 1), iterate through all possible digit values (0–9), keeping all other positions fixed.
- For each candidate, request a power trace from the server and plot it.
- Identify the candidate whose trace is longest or most distinct — this indicates the password comparison advanced furthest before failing, revealing the correct digit at position i.
- Lock in the correct digit for position i and advance to position i+1.
- Repeat steps 2–5 for all remaining positions.
- When the server returns "Password correct," the attack is complete.

5d/
For each of the 4 positions, we need to test 10 candidates in the worst-case scenario. Hence worst case is $10 \times 4 = 40$ attempts. 

5e/
Worst case of brute-force method is $10^4=10000$ attempts. 

6a/ What is the worst-case # of password attempts for your side-channel attack approach
now? a-z,A-Z,0-9

Worst-case SPA attempts: The set has 26+26+10=62 characters. For each of the 12 positions, we can test at most 62  candidates. So the worst case is $62 \times 12 = 744$ attempts. 

6b/ Worst-case brute force: $62 ^ {12}$ attempts. 

7a/ What could be changed in the password verification implementation to eliminate this
particular side-channel attack? 
If all comparisons take roughly the same time regardless of whether a mismatch has occurred, then the power trace is identical in length and shape for all inputs. 
The python standard library provides the function `hmac.compare_digest` in python 3.3+ 

7b/ Explain how an attacker might attack your “improved” implementation?
One of the weaknesses of string comparison timing mitigation is hardware cache. Because even if code execution time is theoretically identical, processor cache misses and branch prediction can still lead to measurable timing discrepancies. 
