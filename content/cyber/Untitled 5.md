---
title: Untitled 5
tags: []
draft: true
date: 2026-04-22
---
  ---             
  Full Workflow
               
  1. Land on box (web shell, RCE, SSH)
           |                                                                                                                                                                                                    
           v
  2. Write payload to tmpfs (/run/shm/ or /dev/shm/)                                                                                                                                                            
     OR use memfd_create() for zero disk touch                                                                                                                                                                  
           |
           v                                                                                                                                                                                                    
  3. Fork a child process
           |                                                                                                                                                                                                    
           v
  4. In child: rename self via prctl + argv[0] spoof                                                                                                                                                            
           |                                                                                                                                                                                                    
           v
  5. Unlink the binary from /run/shm/ immediately                                                                                                                                                               
           |                                                                                                                                                                                                    
           v
  6. Child runs payload — appears as [kworker/0:2] in ps                                                                                                                                                        
           |                                                                                                                                                                                                    
           v
  7. Parent exits cleanly                                                                                                                                                                                       
                  
  ---

```
cp /tmp/malware /run/shm/kworker
chmod +x /run/shm/kworker

/run/hsm/kworker & 
rm /run/shm/kworker


```




memfd inejction is code that runs on the victim machine at runtime. 
dockerfile builds the artifacts 9loader binary, implant) on the c2/atcker side

dockerfile.c2 builds loader.c --> loader binary sent to victim -> loader runs memfd code on victim 



create anonymous file in-memory file --> implant never touches disk

wget implant directly into the memfd via /proc/self/fd path 

fork and exec implant from memfd 


uild ythn comnand with the runtime mfd_path 
