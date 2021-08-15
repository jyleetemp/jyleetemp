---
title: "How to install `nvcc` for conda-installed PyTorch in Ubuntu"
date: 2021-04-10 22:27:34 +0900
tags: [ubuntu, docker]
toc: false
categories: [ubuntu, docker]
---

When we install PyTorch using conda (*e.g.*,`conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch`),
it *incompletly* installs the cudatoolkit, which means that we cannot use `nvcc` provided by the cudatoolkit.

Following commands will help for completely installing the cudatoolkit:

```shell
>> wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
>> sh ./cuda_10.2.89_440.33.01_linux.run --no-opengl-libs --toolkit --silent --override
```

**Note:** Make sure to set `export CUDA_HOME=/usr/local/cuda` in the shell configuration file (*i.e.*, `.bashrc` or `.zshrc`).
{: .notice--info}

---
