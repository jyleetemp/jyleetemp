---
title: "How to fix repeating characters with tab autocompletion in Ubuntu"
date: 2018-03-21 21:21:46 +0900
tags: [ubuntu16.04, locale, duplicate character, tab, autocompletion]
categories: [ubuntu]
---
This article introduces a solution when a character duplication occurs, when one types `tab` for completion.
For me, this usually occurs when I run a Ubuntu docker image in which locale is not configured.

In a docker container:
```shell
$ sudo apt-get update
$ sudo apt-get install locales
$ locale-gen en_US.UTF-8
$ dpkg-reconfigure locales
```
select `en\_US.UTF-8 UTF-8`

```shell
$ dpkg-reconfigure keyboard-configuration
$ localedef -i en_US -c -f UTF-8 en_US.UTF-8
```
commit the image (example):
```shell
$ docker commit -a "Junyong Lee" -m "tf1.10.0" junyonglee_tf codeslake/tensorflow-1.10.0:latest
```
When you run the image, run with the option `-e LNAG=en_US.UTF-8`:
```shell
$ nvidia-docker run --privileged -it -v /home/junyonglee:/root -v /Jarvis:/root/Jarvis -v /Mango:/Mango -v /Mango:/root/Mango -p 7001-7004:7001-7004 -e TERM=`echo $TERM` -e LANG="en_US.UTF-8" -e LANGUAGE="en_US.UTF-8" -e LC_ALL="en_US.UTF-8" --name junyonglee_tf --rm codeslake/tensorflow-1.8.0:latest /bin/zsh
```

---
