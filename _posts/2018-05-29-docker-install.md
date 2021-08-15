---
title: "Installing Docker and Nvidia-docker v1.0.1 in Ubuntu"
date: 2018-05-29 18:32:01 +0900
tags: [docker, nvidia-docker, ubuntu16.04]
categories: [ubuntu, docker, nvidia-docker]
---
This post introduces the installation procedure for the docker (community edition) as well as nvidia-docker v1.0.1

```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update 
$ apt-cache policy docker-ce
$ sudo apt-get install -y docker-ce
$ sudo usermod -aG docker ${USER}
$ wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
$ sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb
$ sudo reboot
```

**Note**<br/>
You may need to run `sudo service restart nvidia-docker`
{: .notice--info}

---
