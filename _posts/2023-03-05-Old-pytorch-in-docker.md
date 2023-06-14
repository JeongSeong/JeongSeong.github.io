---
title: Docker Practice 3 - Using old version of pytorch
author:
  name: SeognYun Jeong
date: 2023-03-05 10:00:00
categories: [Server, Linux]
tags: [docker, pytorch, anaconda]     # TAG names should always be lowercase
---

This post is about setting pytorch environment in a docker container described in [Docker run excercise](http://jeongseong.github.io/posts/Docker-run-excercise/#excercise)

#### **new access to a running container... ```docker exec --help``` for more info**
```docker ps -a``` will return a list of CONTAINER ID, IMAGE, COMMAND, CREATED, STATUS, PORTS, NAMES. you can access a container STATUS of which is Up Let's denote CONTAINER ID or NAMES of a container as **CTR_ID**. To use the container interactively, use ```-it``` option: ```docker exec -it CTR_ID /bin/bash```

note that ```docker attach CTR_ID``` can be used for only one access... ```docker attach --help``` for more info 

#### **make apt-get install possible**
In this environment, ```apt-get install ``` command returns GPG error since there is no public key. 
In the error message, you can find a string, consist of alphabets and numbers, after "NO_PUBKEY". 
Let's denote the string as CODE, and the below command will fix this error.

```terminal
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys CODE
```

#### **create python environment**
I've tried to install python and pytorch with pip, the library dependency error occurred. 
To use conda installation, which manages the dependencies, I installed [Anaconda](https://www.anaconda.com/products/distribution). 

Find "Anaconda Installers" section. Right click "64-Bit (x86) Installer" from the "Linux" column and copy the link of it. Let's denote the link as LINK. 

Type command ```wget LINK``` to download the installation file.

Let's denote the downloaded file as SH_FILE.

Type command ```sh SH_FILE``` to start the installation.

In the installation process, press Enter key for whole agreement. Then, when you meet a question that saying "Do you wish the installer to initialize Anaconda3 by running conda init?", type ```yes``` and Enter.

Then, you have two options, 

- ```exit``` the container and start it by ```sudo docker start CTR_ID```
- get out of the container (Ctrl+p, Ctrl+q) and restart it by ```sudo docker restart CTR_ID```

After you enter the container (```docker exec -it CTR_ID /bin/bash```), you will see the "(base)" ahead of the command line.

#### **create an environment for old version pytorch**
When you go to pytorch website, you can see ["Install" button](https://pytorch.org/get-started/locally/). In the site, search (Ctrl+f) python, then you will find the available python versions, which are 3.7-3.9. Since the old version pytorch will work properly on the lower version python, I will create a virtual environment of python=3.7.0
```terminal
conda create --name NAME_THAT_YOU_LIKE python=3.7.0
```
note that you have to check if such python version available:```conda search python```

enter the create conda environment: ```conda activate NAME_THAT_YOU_LIKE```.

then, you will see "(NAME_THAT_YOU_LIKE)" ahead of the command line.

#### **install an old version pytorch**
To install pytorch in cuda 9.0 environment, first we have to find a compatible version. When you go to pytorch website, you can find ["install previous versions of PyTorch"](https://pytorch.org/get-started/previous-versions/). 

Since CUDA of the container was 9.0, find the appropriate version of pytorch: v1.1.0, v1.0.1, v1.0.0. Choose one among them and copy the command for Linux and Windows that using conda. Then, paste and Enter the command in the terminal.

#### **install CuDNN**
```terminal
conda install -c anaconda cudnn
```

[reference for GPG error](https://velog.io/@offsujin/Ubuntu-GPG-error-%ED%95%B4%EA%B2%B0)