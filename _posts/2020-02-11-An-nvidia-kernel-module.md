---
title: "How to solve `An NVIDIA kernel module nvidia appears to already be loaded in your kernel`"
date: 2020-02-11 7:21:00 +0900
tags: [ubuntu16.04, ubuntu18.04]
categories: [ubuntu]
---
When I tried to install a new nvidia-driver, the error message
\"An NVIDIA kernel module 'nvidia' appears to already be loaded in your kernel\",
appears. 

I've solved it by first finding the process using nvidia,
```shell
$ sudo lsof -n -w /dev/nvidia*
```
And killed all the process using it.

- - -
