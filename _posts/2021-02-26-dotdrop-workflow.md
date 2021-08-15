---
title: "Dotdrop workflow"
date: 2020-12-26 23:38:22 +0900
tags: [ubuntu16.04, dotdrop]
categories: [ubuntu, installation]
---

I use `dotdrop` for managing my dotfiles (.zshrc, .vimrc, .tmux.conf, *etc.*)

Here is the workflow for installing dotfiles using the dotdrop on a server:
```shell
git clone --recurse-submodules -j8 https://github.com/codeslake/dotfiles.git
cd dotfiles
sudo pip3 install -r dotdrop/requirements.txt --user
./dotdrop/bootstrap.sh
./dotdrop.sh install -p junyonglee
```

* Tips
    * Install pip3 with `sudo apt install python3-pip`.
    * If there is an error for docopt, try `sudo apt-get install python3-docopt`.
    * If the error is about ruamel, try `sudo pip3 install ruamel_yaml`.
    * If the error is about python-magic, try `sudo apt install python3-magic`.
    * For OSX, clone the repository and manually copy `oh-my-zsh`, `zsh`, `vim`, `zshrc`, `vimrc` under `dotfiles/` to the home directory. Make sure to add `.` before a folder or a file (e.g., `cp -r oh-my-zsh ~/.oh-my-zsh`).

* Useful plugins
    * Install VundleVim with `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`.
    * Install autosuggestion plugin for zsh with `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`.
    * Install syntax highlighting plugin for zsh with `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`.
    * Install my zsh custom theme with `git clone https://github.com/codeslake/spaceship-prompt.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/spaceship-prompt`.
    
- - -





