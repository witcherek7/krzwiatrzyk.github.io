---
layout: post
title: Systemctl Cheat Sheet
date: 2019-09-13 8:00:00 +0100
description: Quickly about most import systemctl commands
img: 
fig-caption: # Add figcaption (optional)
tags: [Linux, Systemctl, Systemd]
---

## What is systemctl?

    Systemctl is a Linux tool to manage systemd services and units. 


## Commands

+ <span style="color:orange"><i>systemctl start</i></span> - starts service in current session
+ <span style="color:orange"><i>systemctl stop</i></span> - stops the service in current session
+  <span style="color:orange"><i>systemctl restart</i></span> - restart the service
+ <span style="color:orange"><i>systemctl reload</i></span> - reload service files
+  <span style="color:orange"><i>systemctl enable</i></span> - start the service after the kernel is loaded, autostart
+  <span style="color:orange"><i>systemctl disable</i></span> - disable autostart
+  <span style="color:orange"><i>systemctl status</i></span> - checks status on service
+  <span style="color:orange"><i>systemctl is-active/enable/failed</i></span> - shows specific info about service
+  <span style="color:orange"><i>systemctl list-units</i></span> - lists all services