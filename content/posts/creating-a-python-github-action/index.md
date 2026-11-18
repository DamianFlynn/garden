---
title: Creating a Python Github Action
date: '2026-07-28'
draft: true
slug: creating-a-python-github-action
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/creating-a-python-github-action/index.md
neocortex_source_sha: f95f03024fef7273a25df9626dc2cfd041b6f64a15ad20e50692a38f2057211e
description: Guide to creating a public Python GitHub Action for exporting from Obsidian
  to Hugo, including repo setup and development using VSCode.
tags: []
featuredImage: hero.png
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
