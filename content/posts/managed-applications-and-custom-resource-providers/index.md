---
title: "Managed Applications and Custom Resource Providers"
date: "2019-11-07"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
Categories:
  [
    "Azure Management"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/abcf8863-d763-48fa-a379-3630ae167076/Managed-apps.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=c0d120b643edfb42b4e592ec9f30070a96aac6430f809651ab7626c40598ae36&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Managed-Applications-and-Custom-Resource-Providers-26026ec2a236435b9f70d339cd2da0b9",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:56:39.491Z"
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

