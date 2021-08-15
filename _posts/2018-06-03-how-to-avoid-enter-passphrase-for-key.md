---
title: "How to avoid `Enter passphrase for key` for Git"
date: 2018-06-03 14:41:04 +0900
tags: [git]
categories: [ubuntu, git]
---
While using git, it is annoying to type a password or passphrase when there are frequent commits/pushes.

Following steps avoids users from typing passphrase:
### Step 1. Start `ssh-agent`
```shell
$ eval `ssh-agent -s`
```

### Step 2. Add your private key using `ssh-add`
```shell
$ ssh-add ~/.ssh/id_rsa
```
```
Enter passphrase for /home/user/.ssh/id_rsa_key:
Identity added: /home/user/.ssh/id_rsa_key
(/home/user/.ssh/id_rsa_key)
```

---
