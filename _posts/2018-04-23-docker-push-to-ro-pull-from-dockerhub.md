---
title: "How to push to or pull from docker hub"
date: 2018-04-23 12:30:46 +0900
tags: [docker, ubuntu16.04]
categories: [ubuntu, docker]
---

Docker hub is very useful for maintaining images, especially when you are working with many machines with the same image.
---
[My DockerHub](https://hub.docker.com/u/codeslake/)
{: .notice-info}

### Push
```shell
$ docker push codeslake/tensorflow-1.10.0:latest
```
### Pull
```shell
$ docker pull codeslake/tensorflow-1.10.0:latest
```

---
