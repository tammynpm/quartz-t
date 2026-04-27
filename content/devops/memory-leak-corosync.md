---
title: memory-leak-corosync
tags: []
draft: true
date: 2026-04-20
---
```
root@w1:~# free -h 
               total        used        free      shared  buff/cache   available
Mem:            15Gi        15Gi       277Mi        20Mi       114Mi       156Mi
Swap:          8.0Gi       983Mi       7.0Gi
root@w1:~# px aux --sort=-%mem | head -20
-bash: px: command not found
root@w1:~# ps aux --sort=-%mem | head -20
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1176  2.6 95.8 15996200 15603336 ?   SLsl Feb27 1946:01 /usr/sbin/corosync -f
www-data  586845  0.0  0.1 256788 18200 ?        S    00:00   0:00 pveproxy worker
www-data  586847  0.0  0.0 256692 14108 ?        S    00:00   0:00 pveproxy worker
root        1208  0.2  0.0 172884 13028 ?        Ss   Feb27 203:10 pvestatd
root        1103  0.3  0.0 781752 11088 ?        Ssl  Feb27 226:24 /usr/bin/pmxcfs
root         336  0.0  0.0 237364  9756 ?        Ss   Feb27  22:28 /lib/systemd/systemd-journald
www-data  586846  0.0  0.0 256776  9628 ?        S    00:00   0:00 pveproxy worker
root        1197  0.1  0.0 171228  8756 ?        Ss   Feb27 144:05 pve-firewall
root      597661  0.0  0.0  18008  5632 ?        Ss   00:46   0:00 sshd: root@pts/0
root        2658  0.0  0.0 232148  5052 ?        Ss   Feb27  17:38 pve-ha-lrm
www-data   33946  0.0  0.0 248188  4736 ?        Ss   Feb28   1:55 pveproxy
root      597934  0.0  0.0  11084  4736 pts/0    R+   00:47   0:00 ps aux --sort=-%mem
root        1008  0.0  0.0  15436  4480 ?        Ss   Feb27   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root        1230  0.0  0.0 232700  4332 ?        Ss   Feb27   9:29 pve-ha-crm
www-data    2655  0.0  0.0  80904  4096 ?        Ss   Feb27   1:31 spiceproxy
root     2976188  0.0  0.0 255788  3896 ?        S    Apr14   0:36 pvedaemon worker
root     2682737  0.0  0.0 255952  3888 ?        S    Apr13   0:51 pvedaemon worker
root     1351204  0.0  0.0 255960  3696 ?        S    Mar28   2:41 pvedaemon worker
root           1  0.0  0.0 169716  3312 ?        Ss   Feb27   4:19 /sbin/init
root@w1:~# qm list
```


```
root@w1:~# qm list
      VMID NAME                 STATUS     MEM(MB)    BOOTDISK(GB) PID       
       110 anthony-vm           stopped    2048              32.00 0         
       114 alex-roommate-vm     stopped    2048              32.00 0         
root@w1:~# corosync -v
Corosync Cluster Engine, version ''
Copyright (c) 2006-2021 Red Hat, Inc.

Built-in features: dbus monitoring watchdog systemd xmlconf vqsim nozzle snmp pie relro bindnow
Available crypto models: nss openssl
Available compression models: zlib lz4 lz4hc lzo2 lzma bzip2 zstd
```

This line 
```
root        1176  2.6 95.8 15996200 15603336 ?   SLsl Feb27 1946:01 /usr/sbin/corosync -f
```

corosync is consuming 95.8% of RAM and has been running since Feb 27 accumulating 1946 hours of CPU time. That is absolutely a memory leak in corosync. 

- 4/21/26: 1AM w1 and w10 memory leak 
![[Pasted image 20260420010025.png]]



## why only some nodes have memory leaks? 

- some nodes experience memory leaks due to specific combinations of hardware, software, or configuration 


## solution ?


