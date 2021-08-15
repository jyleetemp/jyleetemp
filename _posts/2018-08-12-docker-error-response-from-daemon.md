---
title: "How to fix docker error, `docker:Error response from daemon: Unknown runtime specified nvidia`"
date: 2018-08-12 18:32:01 +0900
tags: [docker, ubuntu16.04]
categories: [ubuntu, docker]
---
The error usually occurs when Nvidia driver is newly installed, or docker/nvidia-docker is installed.

Simply type:
```shell
$ sudo service nividia-docker restart
```

---
