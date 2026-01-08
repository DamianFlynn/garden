---
title: "“Voice First” - A Revolution in User Interaction"
date: "2026-01-08T11:54:00.000Z"
lastmod: "2026-01-08T14:15:00.000Z"
draft: true
featuredImage: "./cover-2e2eb56e.jpg"
Author: "Damian"
authors:
  [
    "Damian"
  ]
Tags:
  [
    "Generative AI"
  ]
Status: "In Progress"
summary: "Explore the transformative potential of the \"Voice First\" revolution in user interaction, where speaking becomes the primary interface for technology, enhancing efficiency, accessibility, and personal connection with our devices—ushering in a new era of seamless communication!"
Categories: "AI"
NOTION_METADATA:
  {
    "object": "page",
    "id": "2e2eb56e-a1c3-80f2-8e8f-c63df8f9bcbc",
    "created_time": "2026-01-08T11:54:00.000Z",
    "last_edited_time": "2026-01-08T14:15:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": {
      "type": "external",
      "external": {
        "url": "https://storagebucketn8n.blob.core.windows.net/notion/2e2eb56e-a1c3-80f2-8e8f-c63df8f9bcbc.png"
      }
    },
    "icon": null,
    "parent": {
      "type": "data_source_id",
      "data_source_id": "0cb08ce3-4a92-421c-8f01-a3d2151ce62e",
      "database_id": "f9f77549-5ec7-48f1-a25a-aaeed54650ab"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Voice-First-A-Revolution-in-User-Interaction-2e2eb56ea1c380f28e8fc63df8f9bcbc",
    "public_url": null
  }
UPDATE_TIME: "2026-01-08T14:19:26.144Z"
last_edited_time: "2026-01-08T14:15:00.000Z"
---

**… And a friendly, voice-controlled interface provides you with all the answers. This scenario isn’t just a thing of the future anymore but may be approaching reality thanks to the “Voice First” concept.**

## The Inspiration: Microsoft’s Guidepost

Watching the Microsoft Copilot event in September 2023 - checking off the ‘Watch-later’ list on vacation 🙂 - it became clear to me: Speech could soon be the primary medium of our digital interactions. The first 15 minutes of the event made it evident how Copilot is already changing our business applications.

## What Does “Voice First” Mean?

“Voice First” refers to prioritizing speech as the main user interface. Just as “Mobile First” emphasized mobile devices, “Voice First” relies on the immediacy and naturalness of the human voice. Imagine not having to fill out a child benefit form but simply talking to a digital assistant about it. This way, a complex document could be replaced by a simple voice dialogue that not only asks for information but also provides guidance.

## The Bridge: Chat Interfaces as an Intermediate Step

![Image](img-2e2eb56e-illustration-good-morning-voice-first_hu933f9d204d1eafc26e29eac2706cc57d_499777_288x0_resize_q55_h1_lanczos_3.webp)

Let’s consider chat interfaces as a transition. They are the first step towards a world where full voice interaction is possible. A purely voice-controlled User Interface (UI) is a fascinating thought, isn’t it? It would fundamentally change the way we interact with technology.

> Instead of typing away, we will soon let our voice do the talking – ‘Voice First’ is not just a trend, it is tomorrow’s communication.

## Speculations and Possibilities

The launch of MS Copilot - which has been usable for a few days now - in business applications might only be the start of a “Voice First” era. Think about it: How would your daily work change if you could talk to your computer instead of typing and clicking? The boosts in efficiency and the reduction of accessibility issues are just a few of the benefits that could be ahead.

* Imagine a world where your words take command – ‘Voice First’ enables seamless and efficient communication with technology without ever having to lift a finger.
* ‘Voice First’ creates a more personal relationship with technology – your device becomes more of a partner than a tool, understanding your instructions and responding like a good friend.
* The ‘Voice First’ movement could radically transform everyday business life – concepts like talking to your computer could enhance productivity while also improving accessibility.
## Your Opinions Are Wanted!

Now it’s your turn. What are your thoughts on “Voice First”? Could the integration of Copilot into our systems be the catalyst for this revolution? Share your thoughts in the comments. It’s incredibly exciting to hear different perspectives on this development. Your insights enrich the discussion and help us shape the future of human-machine interaction. Do you prefer other AI

> We’re at the start of a new era. Let’s explore and shape this path together. 🌐

# Interesting Content

When I originally set up this site, I used Azure Multistage Pipelines to build & deploy the site automatically. I recently switched the CI/CD process to replace Azure Pipelines with [**GitHub Actions**](https://github.com/features/actions) and in this post, I want to show you how I did it.

>  Info: Interested in deploying Hugo with Azure Pipelines?
  If you want to read more about how to publish your Hugo site to Azure static sites with Azure Pipelines, you can my original post here: [**Automated Hugo Releases With Azure Pipelines**](https://www.andrewconnell.com/blog/automated-hugo-releases-with-azure-pipelines/).

Before I get into this, let me jump in front of a few questions you may have:

* Why aren’t you use Azure Static Web Apps? First and foremost, Azure Static Web Apps weren’t around when I launched this site. While a good option, at the time of writing this post they have some limitations. I personally prefer my set up as it gives me much more control.
* Why did I switch to GitHub Actions? GitHub is more natural to me and I like it’s simplicity over Azure DevOps. Azure DevOps is a great platform, but it’s overly complicated for most of my work.
>  The original post on Azure Pipelines already covered CI/CD & my requirements. Please refer to that post if you want to see how I roll things out:
  * Automated Hugo Releases With Azure Pipelines: CI/CD overview
  * Automated Hugo Releases With Azure Pipelines: Requirements
## GitHub Actions for Building & Deploying Hugo Sites

GitHub Actions are just like Azure Pipelines in concept. You create a workflow which is in a **.github/workflows/*.yml** file in your repo. This file contains one or more jobs & each job contains one or more steps.

You start by defining a schedule when you want the workflow to run & other global settings.

There are a few things to note here:

* defaults: This is where you can specify defaults for your entire workflow. Here, I say I want to use a bash shell whenever I run commands in the console.
* on: This section is the trigger definition. Here I’m saying two things:
  * Run this workflow whenever there’s there’s a push to the master branch.
  * Run this workflow on a specific schedule using the cron syntax. I use Hugo’s feature where you can specify content should not be published before a specific time. With these constant builds, I get the scheduled publishing capability of a CMS without the cost of a dynamic site.
    ```yaml
    name: Build and deploy live site

    defaults:
      run:
        shell: bash

    # run this workflow on...
    on:
      # ... all pushes to master
      push:
        branches:
          - master
      # ... this schedule: 1m past the hour @ 7a/9a/12a/5a, M-F (ET)
      schedule:
        - cron: '1 12,14,17,22 * * 1,2,3,4,5'
    ```

Unlike Azure Pipelines where you have to specify the variables you want to use in your pipeline by specifying the variable group you created, your workflow has access to any of the **Settings > Secrets** defined in your GitHub repo. I’ve set a few of these on my repo:

**AndrewConnell.com repro Secrets**

![Image](img-2e2eb56e-hugo-releases-github-secrets_hu11443439386708188114.webp)

I do have a bunch of environment variables set up, but here are the important ones for this blog post.

```yaml
env:
  HUGO_VERSION: 0.71.1
  SITE_BASEURL: https://www.andrewconnell.com
  AZURE_STORAGE_ACCOUNT: andrewconnellcom
  # LIVE_AZURE_STORAGE_KEY: <secret>
```

## Building Hugo Sites with GitHub Actions

Unlike Azure Pipelines, I have my entire build & deploy process in a single job. It’s simpler that way with how Hugo is set up with the deploy command.

### Step 1: Download & Install Hugo

So, I start by cloning the repo & then downloading and installing the Hugo executable:

```yaml
jobs:
######################################################################
# build & deploy the site
######################################################################
build-deploy:
name: Build and deploy
if: "!contains(github.event.head_commit.message,'[skip-ci]')"
runs-on: ubuntu-latest
steps:
######################################################################
# checkout full codebase
######################################################################
- name: Checkout repo codebase
  uses: actions/checkout@v2
  with:
    fetch-depth: 1
    clean: true
    submodules: false
######################################################################
# download & install Hugo (line break added in URL for readability)
######################################################################
- name: Download Hugo v${{ env.HUGO_VERSION }} Linux x64
  run: "wget https://github.com/gohugoio/hugo/releases/download/v${{ env.HUGO_VERSION }}
  /hugo_${{ env.HUGO_VERSION }}_Linux-64bit.deb -O hugo_${{ env.HUGO_VERSION }}_Linux-64bit.deb"
- name: Install Hugo
  run: sudo dpkg -i hugo*.deb
```

You’ll notice this job `build-deploy` contains an `if` condition. At the present time, GitHub Actions doesn’t support skipping a workflow run when the text **[skip-ci]** is present in the commit message. So, I added a condition to check and run this job only if that text isn’t found in the commit message.

### Step 2: Build the site Using the Hugo Executable

With Hugo installed, build the site:

```yaml
######################################################################
# build site with Hugo
######################################################################
- name: Build site with Hugo
  run: hugo --baseUrl '${{ env.SITE_BASEURL }}'
```

Notice I’m using an environment variable passed into the job to control an argument on the Hugo executable.

## Deploy Hugo Sites with GitHub Actions

With the site built, I’m ready to publish! Use the Hugo **deploy** CLI command to deploy the site. This will determine what files need to be uploaded or deleted from the Azure Storage Blob:

```yaml
######################################################################
# deploy site with Hugo deploy comment (only deploys diff)
######################################################################
- name: Deploy build to static site (Azure storage blob)
  run: hugo deploy --maxDeletes -1
  env:
    AZURE_STORAGE_ACCOUNT: ${{ env.AZURE_STORAGE_ACCOUNT }}
    AZURE_STORAGE_KEY: ${{ secrets.LIVE_AZURE_STORAGE_KEY }}
```

### BONUS: Annotate the release in Azure Application Insights

Azure Application Insights supports adding an annotation to your deployment. These will show up in your your AppInsights reports as little callouts in the graphs:

![Image](img-2e2eb56e-azure-application-insights-annotation_hu11918216392546223260.webp)

I’m doing this with a custom action from [**Wictor Wilen**](https://www.wictorwilen.se/) in my workflow:

```yaml
######################################################################
# add release annotation to Azure App Insights
# > only annotate on push events & non [content] commits
######################################################################
- name: Annotate deployment to Azure Application Insights
  uses: wictorwilen/application-insights-action@v1
  if: "github.event_name=='push' && !contains(github.event.head_commit.message,'[content]')"
  with:
    applicationId: ${{ env.AZURE_APPLICATION_INSIGHTS_APPID }}
    apiKey: ${{ secrets.AZURE_APPLICATION_INSIGHTS_APIKEY }}
    releaseName: ${{ github.event_name }}
    message: ${{ github.event.head_commit.message }} (${{ github.event.head_commit.id }})
    actor: ${{ github.actor }}
```

In mine, notice the `if` condition is making sure to only include annotations when the workflow trigger is a **push** (so I don’t add annotations for every scheduled build) and when I don’t have the string **[content]** in the commit message.

This ensures I only add annotations when I’m making a non-content change to the site which is what I want.

I’ve posted the full workflow at the end of this post

## Wrapping it up

That’s it! I’ve written about a few other ways I was using Azure Pipelines with my Hugo site.

In the future days, I’ll show you how I moved those things over to GitHub Actions as well. This includes, for example, the post[**Automatically Reindex Hugo Sites with Azure Pipelines**](https://www.andrewconnell.com/blog/automatically-reindex-hugo-sites-on-updates/).

```yaml
name: Build and deploy live site

defaults:
  run:
    shell: bash

# run this workflow on...
on:
  # ... all pushes to master
  push:
    branches:
      - master
  # ... this schedule: 1m past the hour @ 7a/9a/12a/5a, M-F (ET)
  schedule:
    - cron: '1 12,14,17,22 * * 1,2,3,4,5'

env:
  HUGO_VERSION: 0.71.1
  SITE_BASEURL: https://www.andrewconnell.com
  AZURE_STORAGE_ACCOUNT: andrewconnellcom
  # LIVE_AZURE_STORAGE_KEY: <secret>
  AZURE_APPLICATION_INSIGHTS_APPID: 63YoMama-2e26-W3rs-8Pnk-0fArmyB00tsd
  # AZURE_APPLICATION_INSIGHTS_APIKEY: <secret>

jobs:
  ######################################################################
  # build & deploy the site
  ######################################################################
  build-deploy:
    name: Build and deploy
    if: "!contains(github.event.head_commit.message,'[skip-ci]')"
    runs-on: ubuntu-latest
    steps:
      ######################################################################
      # checkout full codebase
      ######################################################################
      - name: Checkout repo codebase
        uses: actions/checkout@v2
        with:
          fetch-depth: 1
          clean: true
          submodules: false
      ######################################################################
      # download & install Hugo
      ######################################################################
      - name: Download Hugo v${{ env.HUGO_VERSION }} Linux x64
        run: "wget https://github.com/gohugoio/hugo/releases/download/v${{ env.HUGO_VERSION }}
        /hugo_${{ env.HUGO_VERSION }}_Linux-64bit.deb -O hugo_${{ env.HUGO_VERSION }}_Linux-64bit.deb"
      - name: Install Hugo
        run: sudo dpkg -i hugo*.deb
      ######################################################################
      # build site with Hugo
      ######################################################################
      - name: Build site with Hugo
        run: hugo --baseUrl '${{ env.SITE_BASEURL }}'
      ######################################################################
      # deploy site with Hugo deploy comment (only deploys diff)
      ######################################################################
      - name: Deploy build to static site (Azure storage blob)
        run: hugo deploy --maxDeletes -1
        env:
          AZURE_STORAGE_ACCOUNT: ${{ env.AZURE_STORAGE_ACCOUNT }}
          AZURE_STORAGE_KEY: ${{ secrets.LIVE_AZURE_STORAGE_KEY }}
      ######################################################################
      # add release annotation to Azure App Insights
      # > only annotate on push events & non [content] commits
      ######################################################################
      - name: Annotate deployment to Azure Application Insights
        uses: wictorwilen/application-insights-action@v1
        if: "github.event_name=='push' && !contains(github.event.head_commit.message,'[content]')"
        with:
          applicationId: ${{ env.AZURE_APPLICATION_INSIGHTS_APPID }}
          apiKey: ${{ secrets.AZURE_APPLICATION_INSIGHTS_APIKEY }}
          releaseName: ${{ github.event_name }}
          message: ${{ github.event.head_commit.message }} (${{ github.event.head_commit.id }})
          actor: ${{ github.actor }}
```

