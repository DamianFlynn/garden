---
title: Running FastLED on the Dual-Core ESP32
date: '2019-09-11T00:00:00Z'
draft: false
slug: running-fastled-on-the-dual-core-esp32
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/running-fastled-on-the-dual-core-esp32/index.md
neocortex_source_sha: 9102588e3cae12edc557030ffa49aea971b4e2a3699202c4dd708505b791bde0
description: Building the FastLED solution with Platform.IO to run on the ESP32
tags:
- Internet of Things
categories:
- Developer
- Smart Home
- SoC
- Microcontrollers
- IoT
featuredImage: banner.png
---

## FastLED Package

There are many projects posted over the web which implement the excellent FastLED library on the ESP12 processor; however locating a project which implements this on the more powerful sibling is a lot more difficult.

So, with a few failed attempts and a lot of patching samples together; I have a stable running implementation which you can clone or fork to get up and running quickly with your own projects.

The sample includes 2 different sequences, a simple moving dot; and a more colorful Cylon effect.

The code is complied within Visual Studio Code; with the Platform.IO environment; and includes a working settings file while will automatically install the required libraries, ready to compile and flash to your device.

Check out the repo on GitHub @ [https://github.com/DamianFlynn/ESP32FastLED](https://github.com/DamianFlynn/ESP32FastLED); and if you have issues please use the tracker.

Enjoy

---
<!-- enrich:containment -->
*Part of: damianflynn*
