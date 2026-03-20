---
title: tctl
tags: []
draft: false
date: 2026-03-19
---
client gui (teleport-connect) , tctl (teleport server, admin cli)



```
sudo systemctl status teleport
journalctl -u teleport -n 50
```

sudo tctl users add player1 --roles=editor,access

tsh login --proxy=136.115.29.105
tsh ls


sudo ss -tulpn | grep -E ':(443|3022|3023|3025|3080)\b'

What you want to see is at least:

- `443` for web/proxy
    
- `3025` for auth
    
- usually `3022` for node SSH service
    
- `3023` for proxy SSH, depending on listener mode/version


sudo tctl users ls
sudo tctl get roles/node-access
id tamnguyen 


create role file node-access.yaml
```
cat > ~/node-access.yaml <<'EOF'
kind: role
version: v7
metadata:
  name: node-access
spec:
  allow:
    logins: ["tamnguyen "]
    node_labels:
      "*": "*"
EOF
```

create role `sudo tctl create -f ~/node-access.yaml`
confirm `sudo tctl get roles/node-access`



tsh status 
tsh ssh --login=tamnguyen  public-node


## update teleport

```
teleport version
tctl version
tsh version
```

check running service version `sudo journalctl -u teleport | grep version | tail -n 5`

check binary path `which teleport`

```
https://goteleport.com/download/

cd /tmp
curl -LO https://cdn.teleport.dev/teleport-v15.3.2-linux-amd64-bin.tar.gz
tar -xzf teleport-v15.3.2-linux-amd64-bin.tar.gz
cd teleport

sudo systemctl stop teleport


sudo cp teleport tctl tsh /usr/local/bin/
sudo chmod +x /usr/local/bin/teleport /usr/local/bin/tctl /usr/local/bin/tsh


sudo systemctl start teleport


teleport version
tctl status
```

data (users, roles, certs, tokens) is safe because it is in `/var/lib/teleport`

config remains `/etc/teleport/teleport.yaml`

users still exist but existing login sessions (certs) may expire or break

