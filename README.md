# How to install docker on Ubuntu 20.04

## Install Docker 
first update ubuntu
```bash
sudo apt update
```
next install package support for Docker 
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
add GPG key for the official Docker repository
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
update ubuntu again
```bash
sudo apt update
```
Make sure you are about to install from the Docker repo instead of the default Ubuntu repo
```bash
apt-cache policy docker-ce
```
Output
```
docker-ce:
  Installed: (none)
  Candidate: 5:19.03.9~3-0~ubuntu-focal
  Version table:
     5:19.03.9~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```
Install Docker
```bash
sudo apt install docker-ce
```
check status Docker
```bash
sudo systemctl status docker
```
Output
```
Output
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-05-19 17:00:41 UTC; 17s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 24321 (dockerd)
      Tasks: 8
     Memory: 46.4M
     CGroup: /system.slice/docker.service
             └─24321 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```


## Executing the Docker Command Without Sudo
add your username to the docker group
```bash
sudo usermod -aG docker ${USER}
```
or specify USERNAME
```bash
sudo usermod -aG docker username
```
To apply the new group membership
```bash
su - ${USER}
```
You will be prompted to enter your user’s password to continue.

## Check docker working with Docker Images
```bash
docker run hello-world
```






