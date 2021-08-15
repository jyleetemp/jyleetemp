---
title: "How to fix ubuntu showing grub rescue at booting"
date: 2018-08-18 22:12:07 +0900
tags: [Grub, ubuntu16.04]
categories: ubuntu
---
When I reinstalled ubuntu from scratch, the machine sometimes throws following error.
```shell
error: no such partition.
Entering rescue mode...
grub rescue>
```
It seems that the system enters into grub rescue mode when grub is installed in a drive other than `/dev/sda`

## Solution
### Step 1. list file systems
```shell
grub rescue> ls
(hd0) (hd1) (hd2) (hd1,msdos6) (hd1.msdos5) (hd2.msdos1)
```

### Step 2. find the file system for grub to be installed
```shell
grub rescue> ls (hd1,msdos6)
(hd1,msdos6): Filesystem is unknown.
grub rescue> ls (hd1,msdos5)
(hd1,msdos5): Filesystem is unknown.
grub rescue> ls (hd2,msdos1)
(hd2,msdos1): Filesystem is ext4.
```

### Step 3. configuration
```shell
grub rescue> set root=(hd2,msdos1)
grub rescue> set prefix=(hd2,msdos1)/boot/grub
grub rescue> insmod normal
grub rescue> normal
```

### Step 4. permanently fix the configuration
```shell
$ sudo update-grub
$ sudo grub-install /dev/sda
```

## Reference
[How to boot from grub rescue?](https://geekyshacklebolt.wordpress.com/2018/03/13/how-to-boot-from-grub-rescue-fixederror-no-such-partition/)

---
