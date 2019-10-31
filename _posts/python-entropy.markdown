---
layout: post
title: Binary Entropy
date: 2019-10-10 10:00:00 +0100
description: Binary Entropy calculated and ploted in python
img: #
fig-caption: # Add figcaption (optional)
tags: [python, linux, entropy]
---

## TO ADD

LATEX USAGE FOR EQUATIONS

URL: http://flennerhag.com/2017-01-14-latex/

## Entropy

Entropy is a probability of uncertanity.

Equation: <br> H(X) = -p*log2(p)-(1-p)*log2(1-p)



## Entropy Linux

To check an available entropy:
```
cat /proc/sys/kernel/random/entropy_avail
```

1000 < X means that there is no problem.

