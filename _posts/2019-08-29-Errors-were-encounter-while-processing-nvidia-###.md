s---
title: "How to solve `Errors were encountered while processing nvidia-###`"
date: 2019-08-29 11:07:00 +0900
tags: [ubuntu16.04, ubuntu18.04, docker]
categories: [ubuntu, docker]
---
I've recently started to get error message "Errors were encounter while processing nvidia-###"
while installing packages using `apt-get install ...`.
Here is a solution.

```shell
$ sudo apt-get remove nvidia-### --purge
```
- - -
