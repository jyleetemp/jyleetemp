---
title: "How to clean up docker images/containers"
date: 2021-04-06 23:23:59 +0900
tags: [ubuntu, docker]
toc: false
categories: [ubuntu, docker]
---

Here are some commands that I use for cleaning up unused or dangling docker images.

```shell
## New commands (for docker >= 1.13)
# clean up all unused containers, networks, images (both dangling and unreferenced)
>> sudo docker system prune

# remove images older than 4 months since their creation
>> sudo docker system prune -a --filter "until=3840h"

## Old ones
# remove exited containers
>> sudo docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm

# remove dangling images
>> sudo docker rmi $(docker images -f "dangling=true" -q)

# remove images older than 4 months since their creation
>> sudo docker system prune -a --filter "until=3840h"
```

---

## References
[How to remove old and unused Docker images](https://stackoverflow.com/questions/32723111/how-to-remove-old-and-unused-docker-images)<br/>
[How to remove old Docker containers](https://stackoverflow.com/questions/17236796/how-to-remove-old-docker-containers)<br/>
