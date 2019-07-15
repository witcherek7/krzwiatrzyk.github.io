---
layout: post
title: Second post
date: 2019-07-14 10:00:00 +0100
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: #i-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Jekyll, Github]
---

## Github
Wow, it was easier to upload i supposed. First I uploaded only generated 'site' files into username.github.io repository... and that was i mistake. In this kind of scenario i needed two branches (one for pure html and one for jekyll files). I found out that GitHub automatically builds your Jekyll files when GitHub pages is activated on repository. So basically, all I need is one repo with all Jenkins files and when I want to write a post i can do it and push it to the repo and whole building process is done by GitHub or I can create the post on the text editor built inside GitHub page.


## Jekyll
My first mistake when I pushed Jekyll config into the repository was leaving 'localhost' in the url parameter in config.yml file. You have to change it to your page url. In my example: 'krzwiatrzyk.github.io'.
