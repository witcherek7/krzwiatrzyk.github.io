---
layout: post
title: Useful programs & StartUp script for Ubuntu 18.04
date: 2019-10-27 10:00:00 +0100
description: My script to execute on every Ubuntu new VM
img: 
fig-caption: # Add figcaption (optional)
tags: [Linux, Ubuntu]
---

# SCRIPT CONTENT

```
sudo apt-get update
sudo apt-get install openssh -y
sudo systemctl enable ssh
sudo apt-get install git -y
gsettings set org.gnome.desktop.screensaver lock-enabled false
sudo snap install code
sudo apt-get install guake -y
sudo apt-get install terminator -y
```

# Useful programs

## Guake

Guake is a dropdown terminal emulator with simple and fancy usage:

+ `F12` to hide and show terminal
+ `F11` to maximize it
+ `CTRL + SHIFT + R` to change name of tab
+ `CTRL + SHIFT + T` open new tab
+ `CTRL + SHIFT + W` close current tab
+ `CTRL + Page UP/DOWN` choose tab
+ `CTRL + SHIFT + Page UP/DOWN` move tab left/right


## Terminator

Terminator is multiple terminals on one screen tool. Cool! It is very simple, just click right mouse button and you can split every window horizontally or vertically.
