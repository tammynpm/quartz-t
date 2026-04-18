---
title: Untitled 3
tags: []
draft: true
date: 2026-04-07
---

cgroup v1 vs v2
v1: 


## seccomp

- a system call 
- restricts what other system calls the user can execute 
- commonly used with docker containers to protect instances 
- 2 scopes to which seccomp can be implemented: on entire machine or in a running program


3 primary modes:
- seccomp_mode_strict: turn all security measures that seccomp provides on
- seccomp_mode_filter: allows developer/user to restrict certain actions via filters
	- berkeley packet filter: 
- seccomp_mode_disabled: disable seccomp on teh machine 



### chroot
- shell command and a syscall 
- early use of term "jail" applied to chroot comes from Bill Cheswick creating a honeypot to monitor a hacker in 1991

#### process CWD vs ROOT

linux /proc filesystem manages 2 paths for the currently running process:
- cwd --> current working directory of process
- root --> root directory of the process 

- root directory --> symlink to the linux filesystem where actually the program is running
	- usually set to / 
	- but for chrooted environment it is set to path of the directory passed as the first argument to chroot command --> cannot be changed by process directly, but chroot() syscall does this 
- cwd --> symlink to the directory where the process is currently running from its root directory --> can be easily changed by calling cd shell utility or chdir() syscall 



### privilege separation
- programs carry open files descriptors (for files, pipelines, and network connections) into the chroot which can simplify jail desgn by making it unnecessar to leave working files inside the chroot directory. 



### namespaces
currently 6 namespaces:
- mnt (mount points, filesystems)
- pid process
- net
- ipc
- uts
- user


3 syscalls for namespaces
- clone()
- unshare()
- setns(int fd, int nstype)


|                           |                                                                                              |
| ------------------------- | -------------------------------------------------------------------------------------------- |
| clone()                   | creates new process + new namespace --> process is attached to the new namespace             |
| unshare()                 | does not create a new process but create a new namespace, attaches the current process to it |
| setns(int fd, int nstype) | a new syscall was added for joining an existing namespace                                    |


#### uts namespace
UTS (unix timesharing) 


#### netwokr namespace



### cgropus 
 namespaces provide a per process resource isolation solution 
 cgroups --> provides resource management solution (handling groups) 
systemd --> replacement for SysV init scripts --> uses aggressive parallelization capabilities --> start services


#### VFS
virtual file system 
- all entries created in it are not persistent, deleted after reoot
- all cgroup actions are performed via filesystem actions
- cgroups are mounts on /sys/fs/cgroup 





---
types of virtualization: 
- running VMs (Xen,KVM) another OS instance by Hardware virtualization / para virtualization solutions 
- lightweight process level (containers) aka virtual environment (VE) and virtual private server (VPS) 

will VMs disappear from clouds infrastructure?



### lxc containers
- no data structure in the kernel representing a container
- a container = a userspace construct 
- container =linux userspace process
- created by clone() syscall
- when a new container is created, ALWAYS wiht 2 namespaces:
	- PID (CLOND_NEWPID is set)
	- MNT (CLONE_NEWNS flag is set) 
	- if lxc.network.type = none in the container config file, then CONFIG_NEWNET is not set --> same network namespace as host 


#### templates for creating a container
- fedora (under /usr/share/lxc/templates)
- oracle 

name of bridge interfaces are different for Fedora (vibr0) and ubuntu (lxcbr0)



### CRIU 
checkpoint/restore for linux in  userspace

why do we need checkpoint/resotre?
- installing a new kernel
- hw addition/fixes or maintenance
- load balancing
- recovery from disaster by snapshots

