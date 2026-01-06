---
title: "Configure VS Code with Azure Cloud Shell"
date: "2019-01-04"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-3f167845.jpg"
Categories:
  [
    "Azure Management"
  ]
Status: "Published"
Tags:
  [
    "Code"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "3f167845-7055-4506-8445-e68fa050095b",
    "created_time": "2024-07-11T14:00:00.000Z",
    "last_edited_time": "2024-07-19T15:23:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": {
      "type": "file",
      "file": {
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/f92cc607-a4a3-45d7-bfd6-eeda3a761ba6/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=e70dfba9f413efd8afcf1534a2105538dcbd468c171f1d6aed22ff0fbb5aca70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-06T13:23:25.917Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "data_source_id",
      "data_source_id": "235a5f88-c313-46d9-84b5-9f168a1633b7",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Configure-VS-Code-with-Azure-Cloud-Shell-3f167845705545068445e68fa050095b",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:25:34.241Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

After years living in tools like Visual Studio, and PowerShell; Currently my primary landing ground is Visual Studio Code. With my target audience firmly defined as Azure; In this post I am going to share my notes on how to get these two tools working harmonisly; and to make the experience a little richer, we will also mount the underlying Cloud Drive File Share of the Azure Cloud Shell on our local computer as a PowerShell Drive (PSDrive).

# Azure Cloud Shell in Visual Studio Code

In VSCode we will naviagte to the **Extension** icon and search for **Azure Account**, then install it.

The Azure Account extension provides a single Azure sign-in and subscription filtering experience for all other Azure extensions. This extension also exposes the *Azure’s Cloud Shell* service in VS Code’s integrated terminal.

Once the extension in installed you will see the button to **Reload** your Visual Studio Code instance, which you must complete before we can proceed.

## Sign In to Azure

Now, Open the VS Code command palette (using the key sequence *CTRL, SHIFT + P* or *F1*) and select the option to Sign-in to your Azure Subscription; by typing `Azure: Sign In`

Once you click on Sign-in, Visual Studio Code will launch your browser and you will be naviagted to login to your azure account. Select the appropiate account and complete the authentication process. Once authenticated, the browser window will confirm, and tell you to return to VS Code.

As a confirmation of your autentication state, in the status bar of VS Code a new element which is prefixed with the word **Azure:** and postfixed with the account you just authenticated with will be presented.

## Cloud Shell in VS Code

Now that we are signed in, launch the VSCode command palette again (CTRL, SHIFT+P or F1), this time typing `Azure: Open PowerShell in Cloud Shell` or `Azure: Open Bash in Cloud Shell`

> Note: This extension requires Node.JS; If not found on your system, you will be prompted to remediate.

VSCode will update the status bar to indicate it is activating the extenstion, If your account is determined to have access to mulitple Azure AD Directories, you will be prompted to select the directory you wish to work in for this session from the drop down list.

After a few moments you will then observe the **Terminal** window launch, and the session will be be entitled as *PowerShell in Cloud Shell*.

```powershell
Select directory...
Requesting a Cloud Shell...
Connecting terminal...
Welcome to Azure Cloud Shell
Type "az" to use Azure CLI 2.0
Type "help" to learn about Cloud Shell
VERBOSE: Authenticating to Azure ...
VERBOSE: Building your Azure drive ...
Azure:/PS Azure:\>
```

## Mount Azure Cloud Shell drive Locally

For the best flexibility you will regularly have the requirement of interacting with the files in the Cloud Drive, directly from your local working environment.

Launch your browser, and authenticate to https://portal.azure.com

1. Locate the Resource Group which contains the Cloud Shell Storage
1. Open the Storage Account and select the Files Service
1. Inspect the names of the presented File Shares, and select the name which contains your chosen authentication account, eg cs-technology-damianflynn-com-1001100110011001
1. Finally, On the File share blade, you will select the ‘Connect’ button, which will present a new blade to the right of the window.
1. In the blade, choose a Drive letter and then copy the auto-generated PowerShell Commands to map your Cloud Drive.
In a powershell session on your local machine, paste the copied commands; which will automatically mount the cloud drive to your chosen drive letter on your local computer.

From here you can now access and interact with the cloud drive, making changes which will appear real time in the Cloud Shell.

Enjoy!

