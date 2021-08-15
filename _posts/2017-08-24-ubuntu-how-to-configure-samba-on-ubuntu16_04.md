---
title: "How to configure Samba on Ubuntu"
date: 2017-08-24 23:34:05 +0900
tags: [samba, ubuntu16.04]
categories: [ubuntu, samba]
---
This article introduces how to install and configure Samba on Ubuntu16.04.<br/>
Also, we introduce how to mount the shared folder on Ubuntu and Windows.
## 1. Install and configure Samba
### Installation
Samba only needs to be installed for the server:
```shell
$ sudo apt-get install samba samba-common-bin
```
**Note**<br/>
We can check the status of the server with: `$ sudo smbstatus`
{: .notice--info}

For client, we need to install `cifs-utils` package:
```shell
$ sudo apt-get install cifs-utils
```

### Configuration (example)
Modify `/etc/samba/smb.conf`:
```
[workspace]
  comment = workspace
  path = /home/junyonglee/workspace
  writable = yes
  browseable = yes
  valid users = junyonglee
  create mask = 0777
  directory mask = 0777
```

**Note**<br/>
We are setting the mask with 0777, because we are sharing files not only in Windows but also in ubuntu.
{: .notice--info}

Add Samba user:
```shell
$ sudo smbpasswd -a ${USER}
```

{% capture notice-text %}
* The user name for Samba should be the same as user name of Ubuntu.
* For each modification, Samba server needs to be restarted: `$ sudo service smbd restart`
{% endcapture %}

<div class="notice--info">
  <h4>Note</h4>
    {{ notice-text | markdownify }}
</div>

## 2. Mounting
### Ubuntu
```shell
$ sudo mount -t cifs //[DOMAIN OR IP]/workspace ./ -o username=junyonglee,password=[PASSWORD],uid=uid,gid=gid
```
### Windows
Type `\\[DOMAIN OR IP]` on file browser.

---
