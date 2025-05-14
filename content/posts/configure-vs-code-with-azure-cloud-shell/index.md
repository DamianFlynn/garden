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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/f92cc607-a4a3-45d7-bfd6-eeda3a761ba6/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=9078c15af00a510777b7ae73c8856e2a9de08835fb85317178af786597592455&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.037Z"
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
UPDATE_TIME: "2025-05-14T12:11:27.072Z"
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

