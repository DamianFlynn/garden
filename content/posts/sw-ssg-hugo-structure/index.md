---
title: Structuring Repositories For Hugo
type: article 
date: 2023-02-17 00:00:00+00:00
categories: ['todo'] 
tags: ['hugo', 'git', 'staticsite']
draft: False
toc: false 
comments: false 
---

## Obsidian Vault: repository `DamianFlynn/Cranium`

This repo is my Obsidian Vault. It is private and keeps an constant backup of all my notes.

### Plugin: `obsidian-git`
There are a number of Plug-ins added to my vault, but core to this flow is `obsidian-git` which is configured to backup with the following configuration settings

```json
{
	"commitMessage": "vault backup: {{date}}",
	"autoCommitMessage": "vault backup: {{date}}",
	"commitDateFormat": "YYYY-MM-DD HH:mm:ss",
	"autoSaveInterval": 5,
	"autoPushInterval": 0,
	"autoPullInterval": 5,
	"autoPullOnBoot": true,
	"disablePush": false,
	"pullBeforePush": true,
	"disablePopups": false,
	"listChangedFilesInMessageBody": true,
	"showStatusBar": true,
	"updateSubmodules": false,
	"syncMethod": "merge",
	"customMessageOnAutoBackup": false,
	"autoBackupAfterFileChange": true,
	"treeStructure": false,
	"refreshSourceControl": true,
	"basePath": "",
	"differentIntervalCommitAndPush": false,
	"changedFilesInStatusBar": false,
	"showedMobileNotice": true,
	"refreshSourceControlTimer": 7000,
	"showBranchStatusBar": true,
	"setLastSaveToLastCommit": false
}
```

### Workflow: '.github/workflows/'
Git action on push triggers parser to prepare the content which will be made public and posts to Garden repo, builds index and backlinks.

As I am only making a subset of my Craium repository public in the Garden repoisitory, currently this is based on content I place in the `/share` folder of my vault.

This workflow currently looks 
```yaml
name: Prepare Public Content
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}

    steps:
      - name: Checkout Garden repo
        uses: actions/checkout@v3
        with:
          repository: DamianFlynn/garden
          path: garden
          token: ${{secrets.API_TOKEN_GITHUB}}

      - name: Prune the Garden
        run: |
          cp garden/go.mod go.mod.stash
          cd garden
          find . -mindepth 1 -not -regex "^\./\.git.*" 
          find . -mindepth 1 -not -regex "^\./\.git.*" -delete
          cd ..
          cp go.mod.stash garden/go.mod
          mkdir ./garden/content -p
          mkdir ./garden/content/posts -p
          mkdir ./garden/assets/indices -p
          touch ./garden/assets/indices/contentIndex.json
          touch ./garden/assets/indices/linkIndex.json
          mkdir ./garden/static -p
          touch ./garden/static/linkmap

      - name: Checkout Cranium repo
        uses: actions/checkout@v3
        with:
          path: obsidian

      - name: Export Obsidian Notes
        id: obsidian-export
        uses: DamianFlynn/py-obsidian-parser@main
        with:
          hugo-content-dir: ./garden/content/posts
          obsidian-vault-dir: obsidian
          export-dir: share
        continue-on-error: true

      - name: Housekeeping
        run: |
          rm -rf obsidian

      - name: Link and Index Builder
        run: |
          export PATH=$PATH:$(go env GOPATH)/bin
          go install github.com/jackyzha0/hugo-obsidian@latest
          hugo-obsidian -input=./garden/content -output=./garden/assets/indices -index -root=./garden

      - name: Publish Content
        shell: bash
        run: |
          cd garden
          git config --local user.name "github-actions[bot]"
          git config --local user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add -A
          git commit -m "Content update"
          git push https://$USERNAME:$REPO_KEY@github.com/DamianFlynn/garden.git
        env:
          REPO_KEY: ${{secrets.API_TOKEN_GITHUB}}
          USERNAME: github-actions[bot]
        continue-on-error: true
```


## Public Content: repository `DamianFlynn/garden`
 
Repo is configure as a go module
No git action currently, considering a Git action on push to send webhook to Hugo repo to trigger rebuild 

## Hugo: partials repository   

Hugo partials repo (module) contains customising for the partials and statics

## Hugo Site repository `DamianFlynn/hugo`

  

Hugo repo (self module) unions 

the garden repo as the content type source. 

The theme repo as the theme type source

The partials repo as the customisation 

  

Git PR flow triggers a new environment per PR with the latest garden and partials content for preflight testing