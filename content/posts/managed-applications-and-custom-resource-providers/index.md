---
title: "Managed Applications and Custom Resource Providers"
date: "2019-11-07"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
featuredImage: "./cover-26026ec2.jpg"
Categories:
  [
    "Azure Management"
  ]
series:
  [
    "Ignite 2019"
  ]
Status: "Published"
Tags:
  [
    "Conference"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "26026ec2-a236-435b-9f70-d339cd2da0b9",
    "created_time": "2024-07-11T14:01:00.000Z",
    "last_edited_time": "2024-07-19T15:22:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/abcf8863-d763-48fa-a379-3630ae167076/Managed-apps.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=0f3bda54b8e8270b80e09839f9e1479a8dd6a9b68de572ac1466e0f163c88bce&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.688Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Managed-Applications-and-Custom-Resource-Providers-26026ec2a236435b9f70d339cd2da0b9",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:36:19.890Z"
last_edited_time: "2024-07-19T15:22:00.000Z"
---

Magnify the power of extending Azure platform by enabling customers and partners to easily bring in custom solutions to azure. These can be scoped for offering to our own enterprise, or just some selected customers; or even all customers.

  Challenges with extending azure include many of the typical thoughts we face

* As part of my deployment i need to do extra works
* Need to interface with external APIs, create users, storage tables, calling APIs external to Azure, while deploying ARM templates
* 200 Services, which service should i be selected, What is the correct VM SKU? what would be more cost efficient
* How do I integrate my service into Azure; What is the correct option to expose my service to my enterprise, or all azure users
# How do we deploy and offer?

## Deployment Script

**New** resource type - `Microsoft.Resources/DeploymentScripts`

* Allows running any scripts, Azure CLI, Powershell,
* Content can be provided in line of using a URI
* Can also execute Pre or Post configuration on ARM resources,
* Fire and forget resource type, configurable auto-deletion of this, should it be deleted, and if so when?
## Azure Service Catalog

**Service Catalog** blade: Enable applications development teams to be more successful with the right services and scope the selections offered
Used in conjunction with Policy and RBAC, supports the compliance with the organization standard. Customized the creation experience for services and solutions

Applications are managed by a Central IT / Support Team

## Custom Resource Providers

Organizations want to extend ARM and Azure management to the services they user, both custom and 3rd party build.
Partners want to extend their custom resources directly into Azure for their customers
Managed app developers need to give some control to their custom resources, for example integrate a SaaS service in Azure, examples include DataDog or Service Now.

Any REST endpoint can be called from the new Custom Provider Endpoint.

Any API can be extended into Azure, with RBAC Policy, Activity Log etc.
Partners can have full control with governances, policy and templates for these services

> VSCode Managed Application extension is currently in Private Preview

Azure Policy extension allows the ability to link the resource with the managed application, so that each time a new resource is deployed the managed application can process the details.

* Enabled management access out of the box,
* Easy access to author, no code needed to extend azure
* Monotised through the application lifecycle.
* Managed application has full access to the target subscription when deployed.
# Partners - Managed Applications Center

See, manage, and deploy all instances of the applications which may be deployed, managed as scale across tenants.

Cloud Partner portal, new offer under Managed Applications, new SKU.

Include the packaged file for the Managed Application, with the tenant ID, and the view definitions.

Define who will have access - reader, contributor, etc. - that can cross-tenant manage these services.

How to bill, using a private preview (opened in December 2019) using customer metering that can be built on any measure; with price per unit, and amount offered as part of the base prices. Can be up to 18 line items for the application, with different prices by SKUs and users.Private Offers also supported for per-customer pricing.

## Resource Providers

Building a full-blown resource provider in Azure. Most powerful mechanism to deliver your service in Azure.

220+ RPs currently in Azure, first-party account for 90%

Expose resource types specific to your service, including CRUD, etc.

Get all the benefits of the ARM environment, including RBAC, Policy, etc.

## Why?

Custom use native Azure services AND partner Services.

Homogeneous experience across services

Capabilities parity across services and custom billing based on Azure billingLeverage Azure Arc to provide capabilities over resources on on-premises resources.

Custom views for the Custom resource providers

