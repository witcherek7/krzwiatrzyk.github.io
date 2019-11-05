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
+ <span style="color:orange"><i>tail -f /var/log/syslog</i></span> - shows syslog
+ <span style="color:orange"><i>less +F /var/log/syslog</i></span> - shows syslog from the end, with following for updates, `CTRL+C` to stop following and enable scrolling, `SHIFT+F` to start following again
+ <span style="color:orange"><i>reset</i></span> - resets terminal, unbork everything
+ <span style="color:orange"><i>ps axflww</i></span> - shows processes in tree form
+ <span style="color:orange"><i>pstree -p 'pidof dockerd'</i></span> - another form of showing processes in tree form
+ <span style="color:orange"><i>netstat -anp</i></span> - lists open ports with processes that are responsible for them
+ <span style="color:orange">*command | grep --color -P "^|phrase|*</span> - this will leave all output but will color only searched phrase


## Linux Groups

+ <span style="color:orange"><i>adduser USERNAME</i></span> - adds new user
+ <span style="color:orange"><i>cat /etc/group</i></span> - show all user groups in the system
+ <span style="color:orange"><i>groups</i></span> - list all the groups of which the current user is a member
+ <span style="color:orange"><i>useradd name</i></span> - ????? 
+ <span style="color:orange"><i>usermod -G GROUP -a 'USER'</i></span> - adds user to the group 
+ <span style="color:orange"><i>addgroup GROUP</i></span> - create a group
+ <span style="color:orange"><i>groups USERNAME</i></span> - show groups of user
+ <span style="color:orange"><i>adduser USERNAME GROUP</i></span> - add user to the group 
+ <span style="color:orange"><i>deluser USER</i></span> - delete user
+ <span style="color:orange"><i>cut -d: -f1 /etc/passwd</i></span> - show all local users
+ <span style="color:orange"><i>chmod o-x FOLDER</i></span> - restrict other users for accessing directory (HOME)


adduser == useradd
Always use adduser/deluser as they are newer and friendlier.

## Resource stats

```
watch free -h

sudo apt-get install sysstat 
mpstat 1    ##1 is a number of seconds beetwen updates
```


## Keyboard shortcuts

`CTRL + K` - cuts enerything after cursor, it is great to clear console when cursor is at the beginning
`CTRL + U` - cuts everything before cursor
`CTRL + W` - cuts one word before cursor, it uses spaces as seperators
`CTRL + Y` - restore everything that was cutted
