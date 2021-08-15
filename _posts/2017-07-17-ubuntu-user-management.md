---
title: "User management in Ubuntu"
date: 2017-07-17 21:27:02 +0900
tags: [ubuntu16.04, user management]
categories: [lab, ubuntu]
---
This post describes a multi user management method that I use in the lab.
Since all the users require a root privilege, it is better to have only one group with the root privilege.
### Add group
```shell
$ sudo groupadd --gid 2000 cglab
```
### Add user
```shell
$ sudo useradd [username] -m
$ sudo passwd [username]
$ sudo usermod -a -G docker [username]
```
**Note**<br/>
-m: Create a home directory.<br/>
-a: Add an user to the supplementary **group**(s).<br/>
-G: A list of supplementary groups in which the user is associated.<br/>
{: .notice--info}

### Add sudo group
Allow the root privilege for those of who are in the group `cglab`.
```shell
$ sudo visudo
```
Add following line:
```
...
cglab ALL=(ALL) ALL
```
```shell
$ sudo usermod -a -G cglab [username]
```

in user account, type:
```
$ sudo usermod -g cglab [username]
```
**Note**<br/>
-g: The group name or number of the user's new initial login group.
{: .notice--info}

---
### *(Optional) Change shell to zsh*
**Note**<br/>
Make sure to install zsh and oh-my-zsh first:<br/>
`$ sudo apt-get install zsh`<br/>
`$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`<br/>
{: .notice--info}

```shell
$ sudo vim /etc/passwd
```
add following at the end of line where [username] is:
```
...
/bin/zsh
```

---
