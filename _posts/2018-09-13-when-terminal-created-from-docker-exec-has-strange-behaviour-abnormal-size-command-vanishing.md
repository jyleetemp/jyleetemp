---
title: "When terminal created from docker `exec` has strange behaviour (abnormal size, command vanishing, etc.)"
date: 2018-09-13 13:12:54 +0900
tags: [ubuntu16.04, Docker]
categories: [ubuntu, installation]
---

When I launch a docker container using `exec` command inside tmux, I occasionally experience odd behaviors in terminal:
1. The size of terminal within container is not the same as size of tmux pane.
2. When I try to search for a command history with the upper arrow key, the searched command would not be displayed in the terminal.

Here is solution for the behavior.
I basically pass the size of terminal that a container should create.
```shell
docker exec -ti --env COLUMNS=`tput cols` --env LINES=`tput lines` junyonglee_tf /bin/zsh
```

