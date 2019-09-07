---
layout: post
title: Ubuntu Snap installer
date: 2019-09-07 8:00:00 +0100
description: About snap installer and useful commands.
img: ubuntu.svg
fig-caption: # Add figcaption (optional)
tags: [Ubuntu, Snap]
---

## What is snap?

Basically snap is an application installing tool. It's multiplatform and has many features like auto-updating and searchning through database. 

Snap: <https://snapcraft.io>

Ubuntu uses snap as default package installer for Ubuntu software application.

## Snap commands

+ List all the apps:
    >snap list

+ Show tasks and software that is being installed:
    >snap changes

+ Show information about app:
    >snap info `<app>`

+ Searchning for app in database:
    >snap find `<app>`

+ Installing app:
    >sudo snap install `<app>`

+ Uninstalling app:
    >sudo snap remove `<app>`


