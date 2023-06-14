---
title: Docker Practice 2 - Docker run excercise
author:
  name: SeognYun Jeong
date: 2023-03-05 10:00:00
categories: [Server, Linux]
tags: [server, docker, nvidia]     # TAG names should always be lowercase
---


## use docker...  ```docker help``` for more info

##### start / stop / restart docker service commands
```terminal
sudo service docker start

sudo service docker stop

sudo service docker restart
```

## excercise

how to use ubuntu 16.04, cuda 9.0 environment in ubuntu 18.04 or whatever.

[how to use pytorch 1.0.1 in this environment](https://jeongseong.github.io/posts/Old-pytorch-in-docker/)

##### pull ubuntu 16.04 image from docker hub after starting docker service
```terminal
sudo service docker start

docker pull nvidia/cuda:9.0-devel-ubuntu16.04
```

##### list docker images that I have... see this output with the bigger terminal window
```terminal
sudo docker images
```
This command will return a list of REPOSITORY, TAG, IMAGE ID, CREATED, SIZE informations of the images. 
Since I wanted to use ```nvidia/cuda:9.0-devel-ubuntu16.04``` image, find a row that has ```nvidia/cuda``` REPOSITORY and ```9.0-devel-ubuntu16.04``` TAG from the list. Then, let's denote the IMAGE ID from the row as IMG_ID

##### create and run a new container from an image... ```docker run --help``` for more info

Image Identifier can be IMG_ID or nvidia/cuda:9.0-devel-ubuntu16.04, but for the simplicity, I will use IMG_ID in this excercise. The basic command is ```sudo docker run IMG_ID```. Use ```-it``` option to get terminal input interactively, and use ```--gpus all``` option to access all gpus that you have.
```terminal
sudo docker run -it --gpus all IMG_ID
```

If you only want to check if a command (e.g.```nvidia-smi```) works and remove the container automatically when it exits, use ```--rm``` option. 
```terminal
sudo docker run --rm --gpus all IMG_ID nvidia-smi
```

[use docker with nvidia-smi](http://jeongseong.github.io/posts/Docker-with-nvidia-smi/)

##### check if Ubuntu and cuda versions are correct

```cat /etc/issue``` command returns ```Ubuntu 16.04.7 LTS \n \l```

```nvcc -V``` command returns 9.0 in the last line

##### how to get out of the container
If you created container with ```-it``` option, you can get out of the container without stopping it, by typing Ctrl+p, Ctrl+q.

If you want to stop the container and exit it, type ```exit``` command or Ctrl+d



[reference for using docker](https://saint-swithins-day.tistory.com/91)