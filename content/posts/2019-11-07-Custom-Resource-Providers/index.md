---
title: Ignite 2019 - Managed Applications and Custom Resource Providers
type: article 
layout: post 
description: Extending Azure Resource Manager and Publishing Custom Offers
date: 2019-11-07 10:10:09
categories: ['Resource Manager']
tags: ['Custom Providers', 'Managed Applications', 'Service Catalog']
authors: ['damian'] 
draft: True
image: /images/2019/11/07/crp-banner.jpg
toc: false 
featured: false 
comments: false 
---

Ignite Session: BRK3227
Presenters: Gaurav Bhatnagar, Evan Hissey

Challenges with extending azure include many of the typical thoughts we face

* As part of my deployment i need to do extra works
* Need to interface with external APIs, create users, storage tables, calling APIs external to Azure, while deploying ARM templates
* 200 Services, which service should i be selected, What is the correct VM SKU? what would be more cost efficient
* How do I integrate my service into Azure; What is the correct option to expose my service to my enterprise, or all azure users

## What does extending Azure actually mean?

Magnify the power of extending Azure platform by enabling customers and partners to easily bring in custom solutions to azure. These can be scoped for offering to our own enterprise, or just some selected customers; or even all customers.



## Deployment Script

**New** resource type - `Microsoft.Resources/DeploymentScripts`

* Allows running any scripts, Azure CLI, Powershell,
* Content can be provided in line of using a URI
* Can also execute *Pre* or *Post* configuration on **ARM** resources,
* Fire and forget resource type, configurable auto-deletion of this, should it be deleted, and if so when?

## Azure Service Catalog

**Service Catalog** blade: Enable applications development teams to be more successful with the right services and scope the selections offered
used in conjunction with Policy and RBAC, supports the compliance with the organization standard. Customized the creation experience for services and solutions

Applications are managed by a Central IT / Support Team


## Custom Resource Providers

Organizations want to extend ARM and Azure management to the services they user, both custom and 3rd party build.
Partners want to extend their custom resources directly into Azure for their customers
Managed app developers need to give some control to their custom resources, for example integrate a SaaS service in Azure, examples include DataDog or Service Now.

Any REST endpoint can be called from the new Custom Provider Endpoint.

Any API can be extended into Azure, with RBAC Policy, Activity Log etc.
Partners can have full control with governances, policy and templates for these services 

> VSCode Managed Application extension is currently in **Private Preview**

Azure Policy extension allows the ability to link the resource with the managed application, so that each time a new resource is deployed the managed application can process the details.

* Enabled management access out of the box,
* Easy access to author, no code needed to extend azure
* Monotised through the application lifecycle. 
* Managed application has full access to the target subscription when deployed.


Partners, - Managed Applications Center 

See, manage and deploy all instances of the applications which maybe deployed. managed as scale across tenants.
Cloud Partner portal, new offer under mangged applications, new SKU,
Inclued the packaged file for the managemeed application, with the teannat ID, and the view definations
Define who will ahve access, reader, contribtor, ect; that can cross tenant manage these services.
How to bill, using a private preview (open in December 2019) using customer meteringl that can be build on any meauseue; with price per unti, and amount offer as part of the base prices. can be up to 18 line items for the applicstion, with different prices by skus, and users.

Private Offers also supported for per customer pricing


Resource Providers

buidling a full blown resource provider in azure. most prowerfull mechanism to deliver your service in azure. 
220+ RPs currently in azure , first part account for 90%
Espose resurce rypes secpeict to your service invlucing CRUD, etc
Get all the benfits of the ARM environment, including RBAC, Policy, etc.

Why?

Cusrteom use natcice azure services AND partner Services.
Homogenious experience across services
Capabiliers partity across services and cusomt billing based on azure biilling

Leverage Azure ARC for proicude cabailies ove resource on on-premises resouces.
Custom views for the Custom resiucre providers.