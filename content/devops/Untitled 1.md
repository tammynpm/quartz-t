---
title: Untitled 1
tags: []
draft: true
date: 2026-04-07
---
# setting up a Windows VM for Windows Containers

This is a documentation on the fly on how to set up a Windows VM to support Windows containers. 

Test Environment and Performance Metrics:
- OS: Windows Server 2022 (Azure VM )
- Instance Type: Standard_D2s_v3 with 2vCPU, 8GB RAM, 127GB SSD. 
- Average CPU Load: 7.24% 
- Uptime: 100%, stable, no lag  


Original guide: [https://oneuptime.com/blog/post/2026-02-08-how-to-get-started-with-windows-containers-in-docker/view](https://oneuptime.com/blog/post/2026-02-08-how-to-get-started-with-windows-containers-in-docker/view) 

Note that this applies to Windows server 2022, 2019 with Containers feature enabled. To be tested on Windows server 2025.

1/ Enable the Containers feature
```
# Install the Containers feature on Windows Server

Install-WindowsFeature -Name Containers

# Install Docker Engine
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force


Install-Package -Name docker -ProviderName DockerMsftProvider -Force
#incase the command on line 9 doesn't work, try manually install Docker Engine as showed below 

# Restart the server to complete installation
Restart-Computer

# After restart, verify Docker is running
docker version
docker info 
```

Manual Docker Engine Install on Windows Server:

```
# 1. Install the containers feature first
Install-WindowsFeature -Name Containers -Restart

# 2. After restart, download and run the Docker install script
Invoke-WebRequest -UseBasicParsing "https://raw.githubusercontent.com/microsoft/Windows-Containers/Main/helpful_tools/Install-DockerCE/install-docker-ce.ps1" -OutFile install-docker-ce.ps1

# 3. Run it
.\install-docker-ce.ps1
```

other ways to verify that Docker is running: 
```
get-service docker
start-service docker
set-service -name docker -startuptype automatic #auto-start on boot
```

2/ Transfer Dockerfile and source code from a Linux host machine to a Windows VM 
- there are many ways to do this, I used `scp` this time. This requires port 22 to be open on the Windows VM's NSG
- Enable SSH on Windows VM

```
# Run this on the Windows VM
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

- Open port 22 in Azure Portal > VM > Networking > NSG rules 
- copy file (forward slashes or backward slashes change depending on your host OS)
```
scp /path/to/localfile azureuser@<vm-public-ip>:C:/remote/path
```

3/ Build or run docker container as usual
```
docker build -t my-image:latest . 
docker run -d -p 0.0.0.0:4444:4444 --name test-container my-image:latest
```

4/ Note that there are 2 level of firewall for windows VM on Azure. 
- the NSG: allows traffic to reach the VM
- windows firewall
=> You need to have inbound rules on the NSG list and Windows Firewall rule list to allow traffic through a port.  

```
# create inbound rule to allow port 4444
New-NetFirewallRule -DisplayName "Allow Port 4444" -Direction Inbound -LocalPort 4444 -Protocol TCP -Action Allow

# verify port listening
netstat -an | findstr 4444
```
### Troubleshoot
```
# Check Windows event logs for Docker errors
Get-EventLog -LogName Application -Source Docker -Newest 20

# Restart Docker service
Restart-Service docker

# Check if Containers feature is enabled
Get-WindowsFeature -Name Containers
# If not installed:
Install-WindowsFeature -Name Containers -Restart
```


