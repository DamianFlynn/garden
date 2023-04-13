---
title: Using Power Automate to manage Work and Personal Calendars
type: article 
layout: post 
description: Sorting out the chaos of managing multiple calendars, and putting assistants to work
date: 2020-04-16 07:46:30
categories: ['Smart Home', 'Siri', 'Alexa', 'Google Home', 'Azure', 'Microsoft Flow', 'Synchronization', 'Exchange']
tags: ['Internet of Things']
authors: ['damian'] 
draft: True
image: /images/2018/12/18/banner.jpg
toc: false 
featured: false 
comments: True
---

Its just over 18 months since I originally posted about how I organize and connect my various calendars together with the objective of ensuring that I keep all appointments from the disaster of double booking.

Today, my Microsoft Flows all shutdown as I had happily ignored them for much to long, and have had to revist the configuration; which is now branded as Power Automate.

## Sync Family Google Calender to Work Office 365 Calender

1. Google Calander: When an event is added, Updated or Deleted from a Calander

2. Select Calender: 
   Calender Id | Calender
   Start Time | Fx utcNow()
   End Time   | Fx addSeconds