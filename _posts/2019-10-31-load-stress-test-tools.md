---
layout: post
title: Load and stress test tools 
date: 2019-10-29 10:00:00 +0100
description: JMeter stress tool testing
img: 
fig-caption: # Add figcaption (optional)
tags: [JMeter, Apache]
---

## Server stress tools

+ [JMterer](#JMeter)
+ Artillery
+ The Grinder
+ [Taurus](#Taurus)
+ [Siege](#Siege)
+ [Apache Bench](#Bench)
+ [Locust](#Locust)

# <a name="JMeter">Apache JMeter</a>

JMeter is Java based application for load testing servers. It is produced by Apache.

## Installation

Installation script: here

## Preparing JMeter test

### Step 1 - Create a TestPlan - project
### Step 2 - Create a Thread Group - how many threads(users) will be hitting server and how long
### Step 3 - Add a Sampler - method with URL, port and other stuff
### Step 4 - Add listeners - they show data to us
### Step 5 - Execute TestPlan

## Objects

+ Listener - it gathers the data
+ Assertion - our test, ex. if response == 200, RTT time etc.
+ TestPlan - project test in general
+ Thread Group - how many users simultanously are going to send requests
+ Sampler - URL and port
  
## Viewing HMTL based output of the test

```
jmeter -g /Result.csv -o /ReportD
OR
jmeter -n -t <path_to.jmx> -l <log.jtl> -e -o <dashboard_folder>
```

## CLI execution of script:
```
jmeter -n -t <path_to.jmx> -l <log.jtl>
```

# <a name="Bench">Apache Bench</a>

## Installation

```
sudo apt-get install apache2-utils -y
```

## Usage

```
ab -n <num_requests> -c <concurrency> <addr>:<port><path>
```

## Messages
<i>The Connection reset by peer error</i> - means that your web server closed the connection before responding to your request. So, the load is too big for your server and it starts to drop connections.

# <a name="Siege">Siege</a>

## Change max number of opened files message:
```
ulimit -n NUMBER
```

## Installation

```
sudo apt-get install siege -y
```

## Usage

```
siege -c 100 -t 30s http://localhost/
```

# <a name="Locust">Locust</a> 

## Installation

### Option 1 - Docker

https://hub.docker.com/r/peterevans/locust/

Usage also explained on the site.


# <a name="Taurus">Taurus</a>

## Installation

### Option 1 - CLI
Disclaimer: As for now 31.10.2019 Taurus didn't want to work if installed by pip. Container version could be more stable.
```
pip3 install --upgrade pip wheel
pip3 install bzt
pip3 install --upgrade bzt
```

### Option 2 - Docker
```
docker pull blazemeter/taurus
sudo docker run -it --rm -v /tmp/my-test:/bzt-configs blazemeter/taurus my-config.yml

## my-config.yml should be in /tmp/my-test
```

## Example test.yaml:
```
---
execution:
- concurrency: 10
  ramp-up: 1m
  hold-for: 1m30s
  scenario: simple

scenarios:
  simple:
    think-time: 0.75
    requests:
    - http://localhost:5000/
```

## Usage:
```
bzt test.yml
```

