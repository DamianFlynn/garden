---
title: Letting AI operate production Azure — what it gets wrong
date: '2026-11-10'
draft: false
slug: ai-operating-production
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/ai-operating-production/index.md
neocortex_source_sha: eddf2fa41333dd4caaeff6b5e96373596ce37435a52edfb7db01a3830364174a
format: talk
publishDate: '2026-11-07'
events:
- Azure and GitHub Community Day
role: workshop-lead
session_type: workshop
event: Azure and GitHub Community Day
venue: Microsoft Ireland
city: Dublin
country: Ireland
slides: slides.html
slides_resource: slides.html.deck
description: Real failure stories from AI-assisted Azure operations, each with the
  guardrail that fixed it, and the patterns that worked.
tags:
- MVP
- Site Reliability Engineering
categories:
- AI and Agents
- Infrastructure as Code
featuredImage: 00-title.jpg
---

## Abstract

Real failure stories from AI-assisted operations, each with the guardrail that fixed it:

- an Azure GET can't prove a property is settable (recommended four times off an empty read);
- advising a change from a stale read in the middle of a cutover;
- restating a verified fact until it's wrong;
- a disk caching change that hot-detached a production disk;
- "a freeze is not a control".

Plus the patterns that worked: subagents, cross-model review with gh copilot, and grep-verifying every HIGH finding. It's honest, funny, and very MVP.
