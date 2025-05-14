---
title: "Configure VS Code with Azure Cloud Shell"
date: "2019-01-04"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/f92cc607-a4a3-45d7-bfd6-eeda3a761ba6/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=7a3c8d8f6476e02f9eec821fca7998c2c2f58a0455302cbf7fb22ec968c01285&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-13T11:55:29.453Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Configure-VS-Code-with-Azure-Cloud-Shell-3f167845705545068445e68fa050095b",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:56:55.544Z"
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

