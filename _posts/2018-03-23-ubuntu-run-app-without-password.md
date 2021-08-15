---
title: "How to run app with the root privilege without a password in Ubuntu"
date: 2018-03-23 21:00:25 +0900
tags: [ubuntu16.04, sudo]
categories: [ubuntu]
---
Sometimes, there is a need of launching an app with a root privilege.
For example, Vmware requires the root privilege when it needs to use physical hard drive.
Also, Sublime, for example, it needs to be opened with the root privilege when we need to modify a file written as a root.

We can launch them from terminal with `sudo`, but it is inconvenient when we work with GUI.

Here, we specify the app to be always opened with the root privilege:
```shell
$ sudo visudo
```
add following line:
```
[USERNAME] ALL = NOPASSWD: [/path/to/app]
```

---
