---
title: Running FastLED on the Dual-Core ESP32
type: article 
layout: post 
description: Building the FastLED solution with Platform.IO to run on the ESP32
date: 2019-09-11 21:55:09
categories: ['Developer', 'Smart Home', 'SoC', 'Microcontrollers', 'IoT']
tags: ['Internet of Things']
authors: ['damian'] 
draft: false 
image: /images/2019/09/11/banner.png
toc: false 
featured: false 
comments: True
---

## FastLED Package

There are many projects posted over the web which implement the excellent FastLED library on the ESP12 processor; however locating a project which implements this on the more powerful sibling is a lot more difficult.

So, with a few failed attempts and a lot of patching samples together; I have a stable running implementation which you can clone or fork to get up and running quickly with your own projects.

The sample includes 2 different sequences, a simple moving dot; and a more colorful Cylon effect.

The code is complied within Visual Studio Code; with the Platform.IO environment; and includes a working settings file while will automatically install the required libraries, ready to compile and flash to your device.

Check out the repo on GitHub @ [https://github.com/DamianFlynn/ESP32FastLED](https://github.com/DamianFlynn/ESP32FastLED); and if you have issues please use the tracker.

Enjoy