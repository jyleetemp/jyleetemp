---
title: "Setting up Nvidia GPU persistence mode in Ubuntu"
date: 2018-07-26 11:13:37 +0900
tags: [ubuntu16.04, nvidia, GPU, persistence mode]
categories: [ubuntu, nvidia]
---

When `nvidia-smi` prints out an output slowly, the following command may by useful.
```shell
$ sudo nvidia-persistenced --persistence-mode
```

---
