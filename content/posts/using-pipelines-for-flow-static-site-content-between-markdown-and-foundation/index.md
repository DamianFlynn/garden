---
title: "Using pipelines for flow static site content between markdown and foundation"
date: "2018-10-24"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-a9cd739c.jpg"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/52b3e815-9e44-4449-bf7b-ad3516b92a03/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=1db789d7dd50fbab915fa6ea10a1349ca491b12ac6bfa63dbe2f6ce9f30aecf3&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.690Z"
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
UPDATE_TIME: "2025-05-14T14:37:30.188Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
EXPIRY_TIME: "2025-05-14T15:37:27.140Z"
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

