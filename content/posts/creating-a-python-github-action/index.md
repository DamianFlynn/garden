---
title: Creating a Python Github Action
date: '2026-07-28'
draft: true
slug: creating-a-python-github-action
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/creating-a-python-github-action/index.md
neocortex_source_sha: 340b6865b869f7b00540b7d8b9fbbcc52d6bad9532b08a8ef07e2b0f3929ab6e
description: ''
tags: []
---

If you don’t already have a repo, create a new public repo on GitHub. _You cannot create and distribute your GitHub Action from a private repo._

For the purpose of this example, I will be creating a Python application to Export from Obsidian vault to Hugo blog as page bundles. I will be adopting the repository name of **gha-obsidian2hugo** to describe the purpose, with the prefix of *gha* to signify this is a GitHub Action.

![](./Pasted image 20221110130940.png)

Once you’ve created the #Git repository, we will clone it locally, and launch #VSCode.

```bash
git clone https://github.com/DamianFlynn/gha-obsidain2hugo.git
cd gha-obsidian2hugo
code .
```


## Azure Static Web App

### Custom Domain

*(image unavailable — “Static Web App - Add Custom Domain” screenshot not in source vault)*

---
<!-- enrich:containment -->
*Part of: damianflynn*
