---
title: "GPU status checking in Ubuntu"
date: 2018-06-16 16:24:41 +0900
tags: [lab, ubuntu16.04]
categories: [lab]
---
Currently, we are maintaining four clusters: mark0, mark1, mark2, mark3.<br/>
For checking GPU status, **mark0** works as a server where each client (clusters including mark0) runs a shell script informing GPU usages.

### Server Setting
In mark0, run
```shell
$ sh run_server.sh
```

### Client Setting
For every clusters, run
```shell
$ nohup ./gpu_daemon.sh mark0 > /dev/null &`
```

---
