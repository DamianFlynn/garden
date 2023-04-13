---
title: Hosting Static Sites in Azure with CDN
type: article 
layout: post
description: Laying the foundation in Azure for a Static Site
date: 2018-10-20 22:15:09
series: Blogging Journey
categories: ['Cloud']
tags: ['ARM', 'Azure CDN', 'Azure Storage', 'Azure Static Website']
authors: ['damian']
draft: False
image: iac_bicep_tags-as-parameters.png
image_caption: Clipart
toc: False
featured: True
comments: True
---

Hosting my site on Wordpress was not super complex; I leveraged the Azure PaaS Services for Web Apps, and orginally the 3rd party support for hosted MySQL database's. Once I was up and running I quickly realised that all media hosted on the site were landing on the webserver, so a plugin from its marketplace offered the ability to relocate the media to an Azure Blob; offloading some of the challanges.

Hosting this was not free, while I could have leveraged the Free Webserver option it did not take a lot of load for this to be causing some unacceptable performace issues; which when combined with a hosted MySQL service which also was not going to be super fast and was limited in the database size; which became event more of a problem when an attach on the site would result in 1000's of useless comments filling the database to capacity and taking the site down as a direct result.

## Static Site Hosting

Not a lot has to change as we move to the static side model; The web server is still needed to host the side; but the database component is no longer a concern with this approach.

However, The cloud is a beautiful thing, and there are so many more ways to reach you goal, and while we are focused, we can complete a lot more for a lot less!

For this site I have chosen to run a lean cost model, while providing a super responsive experience to you, my readers and subscribers.

### Blob Storage Foundations

Using the standard **Azure Blob Storage** offering, I simply copy over the generated HTML site to the account. Assuming the blob is exposed to the public its content is available as a HTTPS endpoint; which simply put is a website.

But, alone this is not enough; why? because how would I address requested for pages which do not exist for example; I would not want an ugly HTTP 404 error page to be rendered, but instead a nice response to offer a search, navigation, or other more professional experience.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "#{GITVERSION_FullSemVer}#",
  "parameters": {
    "name": {
      "type": "String"
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    },
    "contactEmail": {
      "type": "string",
      "defaultValue": "info@damianflynn.com"
    },
    "projectName": {
      "type": "string",
      "defaultValue": "Static Site Hosting"
    },
    "environment": {
      "type": "string",
      "allowedValues": [
        "Production",
        "Test",
        "Developer",
        "Proof of concept"
      ],
      "defaultValue": "Developer",
      "metadata": {
        "description": "Type of environment"
      }
    }
  },

  "variables": {
    "storageName": "[tolower(take(concat('blob',parameters('name'),uniqueString(resourceGroup().id)),23))]"
  },

  "resources": [

    {
      "comments": "Storage Account for Static Site Content",
      "name": "[variables('storageName')]",
      "apiVersion": "2018-07-01",
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "location": "[parameters('location')]",
      "tags": {
        "Contact": "[parameters('contactEmail')]",
        "Project": "[parameters('projectName')]",
        "Environment": "[parameters('environment')]"
      },
      "scale": null,
      "properties": {
        "networkAcls": {
          "bypass": "AzureServices",
          "virtualNetworkRules": [],
          "ipRules": [],
          "defaultAction": "Allow"
        },
        "supportsHttpsTrafficOnly": true,
        "encryption": {
          "services": {
            "file": {
              "enabled": true
            },
            "blob": {
              "enabled": true
            }
          },
          "keySource": "Microsoft.Storage"
        },
        "accessTier": "Hot"
      },
      "dependsOn": []
    }

  ]
}
```

To achieve this I need some HTTP routing options

### House Warming, Inviting Guests

It is not hard to find a list of potential solutions in Azure to fill this role; We could use a WebApp, API Managment, or event Azure Functions, or the Azure Functions proxy features. However, we do not actually need to be very creative; as Microsoft have listened to uservoice, MVPs and customers all calling for some better web publishing support for blob storage, especially given AWS have features in thier S3 offer that work simply and effectively.

To this end we will take a look at a feature which at the time of writing is still in preview called **Static Website** (You would have never guessed right!)

All we need do, is set this feature as *Enabled* and define where visitors should be routed to for the *Index Document* and the *Error Document*, and apply the changes. This simple feature makes our site behave just as we would like; and it is *free!*.

> Note: Due to Preview Status, we currently have no ARM Template settings for enabling and configuring this setting. Therefore for *Infrasructure as Code* we will fall back to *Azure CLI*

```bash
az extension add --name storage-preview

az storage blob service-properties update '
   --account-name <ACCOUNT_NAME>
   --static-website 
   --404-document <ERROR_DOCUMENT_NAME> 
   --index-document <INDEX_DOCUMENT_NAME>

az storage account show -n <ACCOUNT_NAME> -g <RESOURCE_GROUP> --query "primaryEndpoints.web" --output tsv
```

## Foundation for Global Scale

But why stop here, we can go a little further; I want this site to be responsive for you, regardless of where you are located on the planet; afer all we are all embracing the cloud and need to learn and share; so how better to acomplish this, simply leverage another feature from the Azure arsnal of services.

Calling out the *Azure Content Delivery Network* (Azure CDN) we can take advantage, or any one of three platforms 

* Verizon's Global Network
* Akamai CDN Platform
* Microsofts Azure's Footprint

The choice you make here really will come down to what you want in the list of features and the budget you have in mind; but for the purpose of this blog; I have for now chosen to run with the option of **Premium Verizon**!

Why? Well It is the most feature rich of the offerings currently, and also the most expensive; which still should not cost me more then 3 or 4 euro a month; which is a lot less than I was paying for my Wordpress site. Give me a month or two and I will share thee exact costs of this solution so you can appreciate the value offered by Azure PaaS offerings.


```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "#{GITVERSION_FullSemVer}#",
  "parameters": {
    "name": {
      "type": "String"
    },
    "cdnEndpointTargetUrl": {
      "type": "String"
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    },
    "contactEmail": {
      "type": "string",
      "defaultValue": "info@damianflynn.com"
    },
    "projectName": {
      "type": "string",
      "defaultValue": "Static Site Hosting"
    },
    "environment": {
      "type": "string",
      "allowedValues": [
        "Production",
        "Test",
        "Developer",
        "Proof of concept"
      ],
      "defaultValue": "Developer",
      "metadata": {
        "description": "Type of environment"
      }
    }
  },

  "variables": {
    "cdnName": "[concat(parameters('name'),'-cdn-',parameters('environment'))]",
    "cdnEndpoint": "[concat(parameters('name'),'-cde-',parameters('environment'))]",
    "cdnEndpointName": "[concat(variables('cdnName'), '/',   variables('cdnEndpoint'))]",
    "cdnEndpointOriginName": "[concat(variables('cdnEndPointName'), '/','Origin')]"
  },

  "resources": [
    
    {
      "comments": "Azure CDN Service.",
      "name": "[variables('cdnName')]",
      "apiVersion": "2016-04-02",
      "type": "Microsoft.Cdn/profiles",
      "sku": {
        "name": "Premium_Verizon"
      },
      "location": "[parameters('location')]",
      "tags": {
        "Contact": "[parameters('contactEmail')]",
        "Project": "[parameters('projectName')]",
        "Environment": "[parameters('environment')]"
      },
      "scale": null,
      "dependsOn": []
    },

    {
      "comments": "Azure CDN Endpoint",
      "name": "[variables('cdnEndpointName')]",
      "apiVersion": "2016-04-02",
      "type": "Microsoft.Cdn/profiles/endpoints",
      "location": "[parameters('location')]",
      "tags": {
        "Contact": "[parameters('contactEmail')]",
        "Project": "[parameters('projectName')]",
        "Environment": "[parameters('environment')]"
      },
      "scale": null,
      "properties": {
        "originHostHeader": "[parameters('cdnEndpointTargetURL')]",
        "isHttpAllowed": true,
        "isHttpsAllowed": true,
        "queryStringCachingBehavior": "NotSet",
        "originPath": null,
        "origins": [
          {
            "name": "[variables('cdnEndpoint')]",
            "properties": {
              "hostName": "[parameters('cdnEndpointTargetURL')]",
              "httpPort": 80,
              "httpsPort": 443
            }
          }
        ],
        "contentTypesToCompress": [],
        "isCompressionEnabled": false
      },
      "dependsOn": [
        "[resourceId('Microsoft.Cdn/profiles', variables('cdnName'))]"
      ]
    }

  ]
}
```

The template will setup out CDN environment, all we need to provide is a name for the CDN service, and the URI to the Static Site which we established in the previous stage, for example this may appear as `blobwebaddress.z00.web.core.windows.net` 

## Adding Content

With very little effort, and cost, we now have established a foundation which can serve content efficiently globally, Our next challage will be to deploy our site to this foundation in a easy manner