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


**create join tokne**
`sudo tctl tokens add --type=node --ttl=1h`


install teleport agent on nodes: 


config /etc/teleport.yaml


tsh login 
`tsh login --auth=local --proxy=localhost:443 --user=tamnguyen290204`


ssh agent is not running
SSH_AUTH_PSOCK points to a bad socket 


tsh stores certs under ~/.tsh/...
by default it uses ssh-agent integration 
you can disable local agent use with the --no-use-local-ssh-agent flag or set 

## ssh into a node with join token (already joined) 
tsh ssh user@node-name

# teleport auth + porxy 



![[Pasted image 20260320221855.png]]

service cannot compleete tls handshake with the proxy (teleport service is running, config looks mostly correct, but node is failing to join cluster)

if this is prod, we have a domain and valid cert, so dns works 
should not skip TLS 

on service-nodes:
sudo nano /etc/hosts
add 136.115.29.105 teleport.ccdc.local

edit /etc/teleport.yaml on public-node
proxy-service:
	enabled: true
	web_listen_addr: 0.0.0.0"443
	public_dr: teleport.ccdc.local:443

sudo systemctl restart teleport

on service-node
update agent config
proxy_server: teleport.ccdc.local:443

sudo systemctl restart teleport


tls validation = encryption + identity verification 
control plane = teleport uath + proxy server (public-node0 
)

