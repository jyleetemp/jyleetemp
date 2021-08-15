---
title: "How to solve `ImportError: cannot import name 'main'`"
date: 2019-08-29 11:07:00 +0900
tags: [ubuntu16.04, ubuntu18.04, docker, pip]
categories: [ubuntu, python]
---
When I upgrade `pip3` by typing `sudo pip3 install -U pip` and use the upgraded one,
I get the error message '`ImportError: cannot import name 'main'`'.
This is because a collision between `pip3`s installed by ubuntu and python3.

Here is the solution.

First, you need to reverse `pip3` back to previous version.
```shell
$ python3 -m pip uninstall pip --user && sudo apt install python3-pip --reinstall
```

Second, install `pip3` via `python3`.
```shell
$ python3 -m pip install pip
```

Then, whenever you want to use `pip3`, use it as,
```shell
$ python3 -m pip 
```
For example,
```shell
$ python3 -m pip install -U scikit-image
```
- - -
