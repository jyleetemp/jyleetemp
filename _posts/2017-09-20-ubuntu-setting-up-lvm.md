---
title: "Setting up LVM(Logical Volume Management) in Ubuntu"
date: 2017-09-20 00:14:14 +0900
tags: [ubuntu16.04, LVM]
categories: [ubuntu]
---
In this article, we are grouping four SSDs into two logical drives where each group consists of two SSDs.
## 1. Install LVM
```shell
$ sudo apt-get update
$ sudo apt-get install lvm2
```
## 2. Configure LVM
```shell
$ sudo su`
$ pvcreate /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
$ vgcreate vg001 /dev/sdb1 /dev/sdc1
$ vgcreate vg002 /dev/sde1 /dev/sdd1
$ vgdisplay
```
Check whether `vg001` and `vg002` exist and check the size (X.X TiB) of each group.
```shell
$ lvcreate -L [SIZE OF vg001]t -n lv001 vg001
$ lvcreate -L [SIZE of vg002]t –n lv002 vg002
$ fdisk -l
```
`/dev/mapper/vg001-lv001` and `/dev/mapper/vg002-lv002` should appear.
```shell
$ mkfs.ext4 /dev/vg001/lv001
$ mkfs.ext4 /dev/vg002/lv002
$ mkdir /data1 /data2
$ vi /etc/fstab
```
```
/dev/vg001/lv001            /data1               ext4    defaults 0       0
/dev/vg002/lv002            /data2               ext4    defaults 0       0
```
```shell
$ mount -a
$ df -h
```
Check the mount status of `/data1` and `/data2`.

---
