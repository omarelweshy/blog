---
date: 2021-10-07 14:14:00 GMT+2
categories: [Linux]
tags: [dotfiles]
comments: true
image:
  src: /media/github.png
  width: 800
  height: 533
  alt: Github repo sync dotfiles
---

## Introduction

When your OS crashes or you needed to change it, you will reconfigure `.dotfiles` such as `.zsh`, `.gitconfig` and `.vimrc`. These `.dotfiles` change over time as you start customizing linux according to your needs. How do we save this configurations to avoid manual work every time ? The answer is **Dotfiles Manager**

## How to build a Dotfile Manager

There are alot of Dotfile Managers out there, for example [atlassian](https://www.atlassian.com/), [yadm](https://yadm.io/) and more [here](https://dotfiles.github.io/utilities/).
I prefer the scratch way.

First of all, you should [Create a new repository](https://github.com/new) on GitHub named `.dotfiles` and clone it in the $HOME directory where you will host your dotfiles. Clone your repo to the `.dotfiles` directory

SSH

```shell
git clone git@github.com:<YOUR_GITHUB_USERNAME>/.dotfiles.git ~/.dotfiles
```

HTTPS

```shell
git clone https://github.com/<YOUR_GITHUB_USERNAME>/.dotfiles.git ~/.dotfiles
```

Now, you have an empty directory. The next step is to add your files in it. Move your files to `~/.dotfiles`.

![Dotfiles Dir](/media/dotfiles_dir.png)

For simplicity, we will work on the `.tmux.conf` file then you can do this for the rest of files.

Now, we will move `.tmux.conf` to `~/.dotfiles` directory. You can use `mv` to move file to `.dotfiles`, for example

```shell
mv .tmux.conf .dotfiles/
```

![mv](/media/mv.png)

With the command `ll -al ~/.dotfiles | grep tmux` we search for any files or directories contains tmux name in `~/.dtofiles` but not found. After run `mv .tmux.conf .dotfiles` and search again `.tmux.conf` file moved to `.dotfiles` directory successfully.

#### **BUT**

_The Tmux configuration has broken_. Of course, broken because there is no configuration file in the $HOME directory were in, it’s in the `~/.dotfiles` directory. How do we use this file and at the same time keep it tracked by git in `~/.dotfiles`?

We use `ln` util to link file from `~/.dotfiles` directory to same file in the $HOME directory where can tmux use it and track all changes.

```shell
ln -s ~/.dotfiles/.tmux.conf ~/.tmux.conf
```

With the command above we created a copy of the `.tmux.conf` file to the $HOME directory with a link as shown below

![Link betweem files](/media/linking_file.png)

Everything is ready. By opening `~/dotfiles/.tmux.conf` and `~/.tmux.conf` side-by-side, every editing happens to any of two files, it happens to another.

![Tracking](/media/tracking.gif)
we have a file that configures tmux and same file tracked by git into your `.dotfiles` github repo.

Finally go to `~/.dotfiles` and push to Github.

[Here is my repo](https://github.com/omarelweshy/.dotfiles) with the code.

## Summary

Let's summarize some important things we have done.

1. Create repo named `.dotfiles`
2. Clone it into your Home dir

```shell
git clone git@github.com:<YOUR_GITHUB_USERNAME>/.dotfiles.git ~/.dotfiles
```

3. Move your files to `~/.dotfiles` using `mv`
4. Link your moved files with `ln` and it will automatically copied for you
5. Push your files to Github
