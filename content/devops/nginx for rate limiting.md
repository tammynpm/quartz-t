---
title: Untitled 2
tags: []
draft: true
date: 2026-04-09
---


We will install nginx binaries on the VM. 
```
cat install-nginx 
!#/usr/bin/bash
sudo apt update && sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx

#create site config
sudo nano /etc/nginx/sites-available/myapp
```



Required permissions:  
orgpolicy.customConstraints.create  
OR  
All of policysimulator.orgPolicyViolationsPreviews.create, orgpolicy.customConstraints.get, orgpolicy.constraints.list, orgpolicy.policies.list, cloudasset.assets.searchAllResources, cloudasset.assets.listResource, and cloudasset.assets.exportResource


```
gcloud organizations list
# Get your org ID, then check your permissions:
gcloud organizations get-iam-policy ORG_ID \
  --filter="bindings.members:YOUR_EMAIL"
  
gcloud organizations add-iam-policy-binding ORG_ID \
  --member="user:your-email@domain.com" \
  --role="roles/orgpolicy.policyAdmin"
  
```


