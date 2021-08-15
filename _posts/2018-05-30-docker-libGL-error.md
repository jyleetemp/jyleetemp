---
title: "Solution for docker error, `ImportError: libGL.so.1: cannot open shared object file: No such file or directory`"
date: 2018-05-30 12:42:31 +0900
tags: [docker, ubuntu16.04]
categories: [ubuntu, docker]
---
When we pull a docker image from the dockerhub, the container sometimes throws error regarding with `libGL.so.1`:
```
ImportError: libGL.so.1: cannot open shared object file: No such file or directory
```

It can be easily fixed with:
```shell
$ sudo apt-get update
$ sudo apt-get install -y libgl1-mesa-dev
```

---
