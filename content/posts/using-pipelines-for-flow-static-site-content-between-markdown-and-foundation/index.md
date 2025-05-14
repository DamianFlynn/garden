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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/52b3e815-9e44-4449-bf7b-ad3516b92a03/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=e31ac5ac1483ad708d253f290e0508ab8a50ef36e4175738581aa14023ce12dc&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Using-pipelines-for-flow-static-site-content-between-markdown-and-foundation-a9cd739c8cd543658d3e6d5ffbf71ce3",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:21.144Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
EXPIRY_TIME: "2025-05-13T11:57:19.395Z"
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

