---
title: Structuring Repositories For Hugo
type: article 
date: 2023-02-17 00:00:00+00:00
categories: ['todo'] 
tags: ['hugo', 'git', 'staticsite']
draft: false 
toc: false 
comments: false 
---

## Cranium Repo

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



Share folder

  

GIT extension every 5 minutes pushed changes to Cranium repo as a full backup

  

  

**Garden repo**

  

Repo is configure as a go module

  

No git action currently 

Git action on push to send webhook to Hugo repo to trigger rebuild 

  

  

**Hugo Partials repo**

  

Hugo partials repo (module) contains customising for the partials and statics

  

**Hugo repo**

  

Hugo repo (self module) unions 

the garden repo as the content type source. 

The theme repo as the theme type source

The partials repo as the customisation 

  

Git PR flow triggers a new environment per PR with the latest garden and partials content for preflight testing


`BOT_APP_ID`
`BOT_APP_PRIVATE_KEY`

AZURE_AD_TENANT_ID: ${{ secrets.AZURE_AD_TENANT_ID }}
AZURE_AD_CLIENT_ID: ${{ secrets.AZURE_AD_CLIENT_ID}}
AZURE_AD_CLIENT_SECRET: ${{ secrets.AZURE_AD_CLIENT_SECRET }}
AZURE_SUBSCRIPTION_ID: ${{ secrets.AZURE_SUBSCRIPTION_ID }}