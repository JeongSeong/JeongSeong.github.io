---
title: Docker Practice 1 - Docker with nvidia smi
author:
  name: SeognYun Jeong
date: 2023-03-05 10:00:00
categories: [Server, Linux]
tags: [server, docker, nvidia]     # TAG names should always be lowercase
---

This post is for Ubuntu server with nvidia GPU

## Install docker

##### install packages to use repositories through https
```terminal
sudo apt-get update

sudo apt-get install \
   apt-transport-https \
   ca-certificates \
   curl \
   gnupg \
   lsb-release
```

##### add Docker’s official GPG key
```terminal
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

##### set as stable repo
```terminal
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

##### install docker engine
```terminal
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

If docker is installed successfully, command
```sudo docker run hello-world``` will get hello from docker software

## configuration for nvidia container toolkit to use nvidia GPU

##### stable repository and GPG key setting
```terminal
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

##### install nvidia-docker
```terminal
sudo apt-get update
sudo apt-get install -y nvidia-docker2
```

##### restart docker daemon for the updates
```terminal
sudo systemctl restart docker
```

##### check if ```nvidia-smi``` command works
run a docker container with ```nvidia-smi``` command, or and check it inside the container

[example of running a docker container](http://jeongseong.github.io/posts/Docker-run-excercise/)


[reference for nvidia docker](https://velog.io/@boom109/Nvidia-docker)


<!-- ---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
2022-11-29 16:38:00 
categories: [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [TAG]     # TAG names should always be lowercase
--- -->
