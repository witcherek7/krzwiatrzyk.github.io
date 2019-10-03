---
layout: post
title: Great Linux Commands
date: 2019-10-02 8:00:00 +0100
description: Linux commands that can be usefull during daily operations
img: 
fig-caption: # Add figcaption (optional)
tags: [Linux]
---

## Commands

+ <span style="color:orange"><i>sudo !!</i></span> - redo previous command with root privilages
+ <span style="color:orange"><i>ctrl+x+e</i></span> - open an editor to run a command, commands written in the editor will be immidietaly executed
+ <span style="color:orange"><i> echo "st"</i></span> - to avoid command going to history of commands just type space before a command
+ <span style="color:orange"><i>fc</i></span> - fix the previous command in editor
+ <span style="color:orange"><i>ssh -L portLocal:addressRemote:portRemote root@host.com -N</i></span> - binds remote address to local host address
+ <span style="color:orange"><i>cat file | tee -a log | cat > /dev/null</i></span> - intercept stdout and log to file (tee)
+ <span style="color:orange"><i>disown -a && exit</i></span> - exit terminal but leave all processes running, usefull when connecting to cloud and disconnecting could kill the process
+ <span style="color:orange"><i>echo $SHELL</i></span> - checking which shell is installed
