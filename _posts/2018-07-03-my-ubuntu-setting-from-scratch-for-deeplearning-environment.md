---
title: "My ubuntu setting procedure for deep learning environment"
date: 2018-07-03 01:40:46 +0900
tags: [ubuntu16.04, setting, deep learning environment]
categories: [ubuntu, installation]
---

Here, I summarize my deep learning environment setting on freshly-installed Ubuntu16.04.

## 1. Nvidia Driver Installation
### Download nvidia driver
```shell
$ cd ~/Download
$ wget http://us.download.nvidia.com/XFree86/Linux-x86_64/390.42/NVIDIA-Linux-x86_64-390.42.run
```
### Configuration
```shell
$ sudo vim /etc/modprobe.d/blacklist-nouveau.conf
```
```
blakclist nouveau
options nouveau modset=0
```
```shell
$ sudo update-initrafms -u
$ sudo reboot
```
### Installation
After a reboot, enter CLI mode and type:
```shell
$ sudo service stop lightdm
$ cd ~/Download
$ sudo sh  ./NVIDIA-Linux-x86_64-390.42.run --no-opengl-files
$ sudo service restart lightdm
```
**Note:** Do not give the option `--no-opengl-files` if you work in ubuntu GUI for the most of time.
If you give `--no-opengl-files` option, GUI runs in cpu, which slows down the user experience.
{: .notice--warning}

**Note:** When `The system is running in low-graphics mode` appears, install dkms with
`sudo apt-get install dkms`<br/> before adding `blacklist-nouveau.conf`. Then, select `YES` when the NVIDIA-driver installation process asks for applying `dkms` in the system.
{: .notice--warning}

## 2. Basic Installations & Settings
### Mount NAS
```shell
$ sudo mkdir -p /Mango/Users /Mango/Common /Jarvis/logs /Jarvis/workspace
$ sudo vim /etc/fstab
```
```
[NAS_M]:/volume1/Users /Mango/Users nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800,rsize=32768,wsize=32768 0 0
[NAS_M]:/volume2/Common /Mango/Common nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800,rsize=32768,wsize=32768 0 0

[NAS_J]:/volume1/logs /Jarvis/logs nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800,rsize=32768,wsize=32768 0 0
[NAS_J]:/volume1/workspace /Jarvis/workspace nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800,rsize=32768,wsize=32768 0 0
```
```shell
$ sudo mount -a
$ ln -s /Mango ~/Mango
$ ln -s /Jarvis ~/Jarvis
```
### Basic installations
```shell
$ sudo apt-get install git
$ sudo apt-get install wget
$ sudo apt-get install curl
$ sudo apt-get install tmux
$ sudo apt-get install vim
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
$ sudo apt-get install zsh
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

### Basic configurations
```shell
$ git clone git@github.com:codeslake/settings.git
$ cp settings/.vimrc ~/
$ cp settings/.zshrc ~/
$ cp settings/.tmux.conf ~/
$ cp -r settings/.tmux ~/
$ cp wombat256mod.vim /usr/share/vim/vim*/colors
$ rm -r settings
```
## 3. Configure `vim`
```shell
$ vim
```
```
:BundleInstall
```
### Install `YouCompleteMe` for `vim`
```shell
$ sudo apt-get install vuild-essential cmake
$ sudo apt-get install python-dev python3-dev
$ cd ~/.vim/bundle/YouCompleteMe
$ ./install.py -all
```

## 4. Docker, Nvidia-docker
### Install docker
```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update 
$ apt-cacheolicy docker-ce
$ sudo apt-get install -y docker-ce
$ sudo usermod -aG docker ${USER}
```
### Install nvidia-docker
```shell
$ wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
$ sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb
$ sudo reboot
```
### Pull images from dockerHub
```shell
$ docker pull codeslake/tensorflow-1.10.0:latest
```

### Useful Commands
```shell
$ nvidia-docker run --privileged -it -v /home/junyonglee:/root -v /Jarvis:/root/Jarvis -v /Mango:/Mango -v /Mango:/root/Mango -p 7001-7004:7001-7004 -e "TERM=`echo $TERM`"-e "LNAG=en_US.UTF-8" --name junyonglee_tf --rm codeslake/tensorflow-1.8.0:latest /bin/zsh
$ nvidia-docker attach junyonglee_tf
$ nvidia-docker exec -it junyonglee_tf /bin/zsh
$ docker commit -a "Junyong Lee" -m "tf1.10.0" junyonglee_tf codeslake/tensorflow-1.10.0:latest
$ docker build --tag codeslake/tensorflow-1.10.0:latest
```

---
