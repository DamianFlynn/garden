---
title: Installing Docker and Compose on Raspbian Buster
date: '2019-10-01T00:00:00Z'
draft: false
slug: installing-docker-and-compose-on-raspbian-buster
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/installing-docker-and-compose-on-raspbian-buster/index.md
neocortex_source_sha: ccc0088f15ee739ef78f1c7101274fcb5dca8144c0b0df0276a291a3d10a9436
description: Steps to install Docker and Docker-compose on a Raspbian Buster PI.
tags:
- Docker
- Raspberry Pi
- Linux
categories:
- DevOps and Tooling
featuredImage: banner.png
---

Quickly update a new Raspberry Pi, which has an install of Raspbian Buster with Docker and Docker-compose.

## Docker

This is simple, as the Docker team have done all the work

```bash
curl -fsSL get.docker.com -o get-docker.sh
sh get-docker.sh
```

And, we can add our user to the Docker group so we do not need the `sudo` every time. I am using the environment variable `$USER`; which indicates who is logged in currently. In my case this is the user *pi*.

```bash
sudo usermod -aG docker $USER
```


Right, that was painful. now reboot the Pi and we are solid.

## Docker-Compose

This is actually a Python script. Raspbian Buster is shipped with Python 3.6; so we just need to add `PIP3` to install the python packages from *pypy*

```bash
sudo apt-get install -y python3 python3-pip
sudo pip3 install docker-compose
```

Wow, that was a struggle, lets check we are good

```bash
docker-compose --version
```

---
<!-- enrich:containment -->
*Part of: damianflynn*
