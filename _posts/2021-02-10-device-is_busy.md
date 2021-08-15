---
title: "How to resolve `device is busy` when unmounting a drive in Ubuntu"
date: 2021-01-10 09:45:19 +0900
tags: [ubuntu18.04, umount]
categories: [ubuntu, installation]
---

When the error containing `device is busy` occurs when one tries to unmount the mounted drive, try this one:

```shell
umount -l [PATH to mount point]
```
