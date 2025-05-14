---
title: "Using pipelines for flow static site content between markdown and foundation"
date: "2018-10-24"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Status: "Published"
Tags:
  [
    "SSG"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "a9cd739c-8cd5-4365-8d3e-6d5ffbf71ce3",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/52b3e815-9e44-4449-bf7b-ad3516b92a03/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=98af1a8c21ea80890cb85d13f00867d74892e8887b149f99dce95c3d3ca6d073&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.038Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Using-pipelines-for-flow-static-site-content-between-markdown-and-foundation-a9cd739c8cd543658d3e6d5ffbf71ce3",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:11:55.987Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
EXPIRY_TIME: "2025-05-14T13:11:52.492Z"
---

With the heavy lifting done in creating the site building mechanics and a solid foundation to build and share upon; our final objective is to automate the process of connecting these two stages.

# Release Pipeline

Technically the goal we are speaking about is the **Release Pipeline** which will take the artefact *(our site .ZIP file)* that we created in the **Build Pipeline** in our previous topic [Constructing a new Home with Jekyll and Azure DevOps](/Building-The-Site/); and publish this to our storage account.

In simple terms, we are going to Unzip the archive to the storage, which is configured to expose the content as a website, and to increase performance we are leveraging a content delivery network.

## Copying Content

As with our *Build Pipeline* there are a few different options on how to accomplish the work.

> Currently Azure DevOps is not exposing the release pipelines in an Infrastructure as Code configuration so we will complete this in the portal.

We begin with a new *Release Pipeline*, add add the task **Azure File Copy**, and set the settings similar to the following:

  Done!, Seriously; pretty easy right!

## Replace Content

One small issue with the previous approach is that it merely overwrites the content in the blob with the latest version, but it failed to remove any content which might have been removed from the site.

Currently, there is no equivalent to the magic *XCOPY* command that will sync the blob removing the data that is no longer required; that’s a project for another day.

The quick and dirty fix for this scenario is first to delete the existing content and then deploy the latest version. This would typically result in a potential outage as the content vanishes for a little time; however, we do not have that concern, because we have decided to use a Content Delivery Network which caches our site.

### Task 1

  ### Task 2

  ## Tiggers Ready

The last point to consider is *When* should this deployment happen, and that’s also pretty obvious when we think about it.

Every time we have a good build of the site, we should check to see which branch just completed the process, and update the relevant site based on this information.

![Image](img-a9cd739c-Release-Pipeline-Flow-01.png)

## Summary

Which flow you choose to implement are entirely at your discretion; As I am currently doing much work on the taxonomy of the site; I have implemented the second option of delete and redeploy; but I do in the plan to come back and optimize this by adding a sync process which will be far more efficient.

