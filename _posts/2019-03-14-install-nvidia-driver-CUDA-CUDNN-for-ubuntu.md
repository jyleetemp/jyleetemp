---
title: "Installing nvidia driver, CUDA and cuDNN for Ubuntu"
date: 2019-03-14 09:54:00 +0900
tags: [ubuntu16.04, ubuntu18.04, nvidia driver, CUDA, cuDNN]
categories: [ubuntu, CUDA]
---
In this document, I post how to install nvidia driver, CUDA and cuDNN for ubuntu.

- - - 
## Prerequisites
**Note:** Make suer to check version [compatibility](https://docs.nvidia.com/deploy/cuda-compatibility/index.html) between drivers. 
{: .notice--info}

### nvidia driver
You can install nvidia driver when you install CUDA. But If you want to install
specific version of nvidia driver, download it from [official website](https://www.nvidia.co.kr/Download/index.aspx),
or download it directly using `wget`. For example,
```shell
$ wget http://us.download.nvidia.com/XFree86/Linux-x86_64/418.56/NVIDIA-Linux-x86_64-418.56.run
```

### CUDA
Download runfile from [official website](https://developer.nvidia.com/cuda-downloads).
I selected Linux-x86\_64-Ubuntu-16.04-runfile (local).
Or, you can also run following line.
```shell
$ wget https://developer.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.168_418.67_linux.run
```

### cuDNN
Download cuDNN from [here](https://developer.nvidia.com/rdp/cudnn-download).
You need to have nvidia developer account to download it.
Download cuDNN 1.Runtime, 2.Developer, and 3.Code Samples and User Guide for Ubuntu 16.04 (The last one is optional).

- - -

## Installation
### Step 1. nvidia driver
**Note:** Install `dkms` before installation (`sudo apt-get install dkms`).
It prevents ubuntu from reinstalling nvidia-driver for every time you reboot.
{: .notice--info}

- Stop `lightdm` and enter into text login mode.
```shell
$ sudo service stop lightdm
$ sudo init 3
```

- Install nvidia driver
```shell
$ sudo sh NVIDIA-Linux-x86_64-418.56.run --dkms --no-opengl-files --no-x-check
```
When there is problem installing driver (*e.g.* `"You appear to be running an X server"`),
`--no-x-check` gives you detail error, such as `"nvidia-uvm appears to already be loaded in your kernel"`.

- And if there is such error, run following command.
```shell
$ sudo stop nvidia-digits-server
$ sudo service nvidia-docker stop
$ sudo rmmod nvidia-uvm
```

### Step 2. Install CUDA
- Run following command, and select only CUDA toolkit.
```shell
$ sudo sh cuda_10.1.168_418.56.run
```

### Step 3. Install cuDNN
- Install runtime, developer, code samples and the cuDNN library User Guide (optional) with following lines.
```shell
$ sudo dpkg -i libcudnn7_7.6.0.64-1+cuda10.1_amd64.deb
$ sudo dpkg -i libcudnn7-dev_7.6.0.64-1+cuda10.1_amd64.deb
$ sudo dpkg -i libcudnn7-doc_7.6.0.64-1+cuda10.1_amd64.deb
```

### Step 4. Add path to your `.zshrc` or `.bashrc`
```shell
$ vim ~/.zshrc

export LD_LIBRARY_PATH="/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH"
export PATH="/usr/local/cuda/bin:$PATH"
```

- - -

## References
[cuDNN installation guide](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html)<br/>
[nvidi-uvm error](https://github.com/NVIDIA/nvidia-docker/issues/62)

- - -
