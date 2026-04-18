---
title: Untitled 3
tags: []
draft: true
date: 2026-04-13
---

set to the right project: 

`gcloud config set project umass-ctf-26`

```
gcloud compute disks remove-resource-policies misc-osint-rev-rf-challenges --resource-policies=default-schedule-1 --zone=us-west3-a
```






maybe we can shutdown the VMs and 


migrating ctfd 
- back up database, uploaded files 


```
docker exec ctfd-db-1 mysqldump -u ctfd -pctfd ctfd > ctfd_backup.sql

ls -lh ctfd_backup.sql
head -5 ctfd_backup.sql  # should show MariaDB dump header



tar -czf ctfd_uploads_backup.tar.gz /home/umasscybersec/CTFd/.data/CTFd/uploads


tar -czf ctfd_config_backup.tar.gz /home/umasscybersec/CTFd/

ls -lh ctfd_backup.sql ctfd_uploads_backup.tar.gz ctfd_config_backup.tar.gz

docker exec ctfd-ctfd-1 env | grep SECRET_KEY


```