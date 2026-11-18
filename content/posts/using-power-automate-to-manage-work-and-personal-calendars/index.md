---
title: Using Power Automate to manage Work and Personal Calendars
date: '2020-04-16T00:00:00Z'
draft: true
slug: using-power-automate-to-manage-work-and-personal-calendars
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/using-power-automate-to-manage-work-and-personal-calendars/index.md
neocortex_source_sha: 447d511656080a9d7ca589421538eb9837cefd82f16f6c68e67e35937868f838
description: Sorting out the chaos of managing multiple calendars, and putting assistants
  to work
tags:
- Internet of Things
categories:
- Smart Home
- Siri
- Alexa
- Google Home
- Azure
- Microsoft Flow
- Synchronization
- Exchange
---

Its just over 18 months since I originally posted about how I organize and connect my various calendars together with the objective of ensuring that I keep all appointments from the disaster of double booking.

Today, my Microsoft Flows all shutdown as I had happily ignored them for much to long, and have had to revist the configuration; which is now branded as Power Automate.

## Sync Family Google Calender to Work Office 365 Calender

1. Google Calander: When an event is added, Updated or Deleted from a Calander

2. Select Calender: 
   Calender Id | Calender
   Start Time | Fx utcNow()
   End Time   | Fx addSeconds

---
<!-- enrich:containment -->
*Part of: damianflynn*
