---
title: "How to set up NIS for Ubuntu"
date: 2019-06-04 15:59:00 +0900
tags: [ubuntu16.04, ubuntu18.04, NIS]
toc: true
categories: [ubuntu, NIS]
---
NIS is very useful tool to automatically set up user information between clusters.
Currently, our lab maintains 9 clusters: mark0-8.
Here, mark1 is working as the NIS server where others are clients.

**Note:** If there is a group with root privilege in the server via `sudo visudo`,
the group should also be configured in clients.
{: .notice--warning}

## Server
### Step 1. Update Ubuntu
```shell
$ apt-get update && apt-get -y dist-upgrade
```

### Step 2. Install NIS
```shell
$ apt-get install -y rpcbind nis

...
NIS domain:
[DOMAIN NAME]
```
**Note:** We've set `[DOMAIN NAME]` as `mark1.nis`.
{: .notice--info}

### Step 3. Edit `/etc/default/nis`
```shell
$ sed -i 's/NISSERVER=.*$/NISSERVER=master/' /etc/default/nis
```

### Step 4. Edit `/etc/hosts`
```bash
$ sudo vim /etc/hosts
...

[IP ADDRESS] [HOSTNAME]
```

**Note:** We've set it to `141.***.***.*** cglabmark1` where `cglabmark1` is the host name of the server.
{: .notice--info}

### Step 5. Configure NIS
```shell
$ /usr/lib/yp/ypinit -m

At this point, we have to construct a list of the hosts which will run NIS
servers.
cglabmark1 is in the list of NIS server hosts. Please continue to add
the names for the other hosts, one per line. When you are done with the
list, type a <control D>.
  next host to add: cglabmark1
  next host to add: 
The current list of NIS servers looks like this:

  cglabmark1

Is this correct? [y/n: y] y
We need a few minutes to build the databases...
Building /var/yp/mark1.nis/ypservers...
Running /var/yp/Makefile...
gmake[1]: Entering directory '/var/yp/mark1.nis'
Updating passwd.byname...
...
gmake[1]: Leaving directory '/var/yp/mark1.nis'

cglabmark1 has been set up as a NIS master server.
```

### Step 6. Update changes in user information
```shell
make -C /var/yp/
```
- - -

## Client
### Step 1. Update Ubuntu
```shell
$ apt-get update && apt-get -y dist-upgrade
```

### Step 2. Install NIS
```shell
$ apt-get install -y rpcbind nis
```

{% capture notice-text %}
* For step 2, make sure to add the domain name of the server (For me, it is mark1.nis).
* The domain name can be found at `/etc/defaultdomain` in the server.
* We can change the domain name with `$ dpkg-reconfigure nis`.
{% endcapture %}

<div class="notice--info">
  <h4>Note</h4>
    {{ notice-text | markdownify }}
</div>

### Step 3. add following line to `/etc/yp.conf`
```shell
$ sudo vim /etc/yp.conf

domain mark1.nis server [IP ADDRESS of SERVER]
```

**Note:** Do not type the domain address. NIS has a bug looking up the DNS server.
{: .notice--danger}

### Step 4. Edit `/etc/nsswitch.conf`
#### * option 1 (for ubuntu 18.04)
```shell
$ sudo sed -i 's/compat$/compat nis/g;s/dns$/dns nis/g' /etc/nsswitch.conf
```
#### * option 2
```shell
$ vim /etc/nsswitch.conf

passwd:     compat nis      # line 7; add
group:      compat nis      # add
shadow:     compat nis      # add

hosts:      files dns nis   # add
```

### Step 5. Edit `/etc/pam.d/common-session` for creating home directory automatically
```shell
$ vim /etc/pam.d/common-session

# add to the end
session optional       pam_mkhomedir.so skel=/etc/skel umask=000
```

### Step 6. Restart NIS
```shell
$ sudo systemctl restart rpcbind
$ sudo systemctl restart nis
```

**Note:** If an user sets up one's default shell as other than bash (e.g., zsh), make sure to install it!
{: .notice--danger}

- - -
### For ubuntu18.04, we need to change systemd setting
comment the line `IPAdressDeny=Any` in `/lib/systemd/system/systemd-logind.service`
- - -

## References
[How to install a cluster with NIS and NFS in Ubuntu 16.04](https://ilearnedhowto.wordpress.com/2017/05/15/how-to-install-a-cluster-with-nis-and-nfs-in-ubuntu-16-04/)<br/>
[Configure NIS Server](https://www.server-world.info/en/note?os=Ubuntu_16.04&p=nis&f=1)<br/>
[Configure NIS Client](https://www.server-world.info/en/note?os=Ubuntu_16.04&p=nis&f=2)<br/>
[for ubuntu18.04](https://askubuntu.com/questions/1031022/using-nis-client-in-ubuntu-18-04-crashes-both-gnome-and-unity)<br/>

- - -
