---
title: "Laying the foundation in Azure for a Static Site"
date: "2018-10-20"
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
    "id": "b3f55055-b878-42fa-be97-27205f81b68d",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/baaa316b-8ce2-4df7-833c-36a7039ceb85/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=037782eb3a85463766bfe2f6c2d18c9af954492bf0d8d4745d5883b74f1643c7&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Laying-the-foundation-in-Azure-for-a-Static-Site-b3f55055b87842fabe9727205f81b68d",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:25.346Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

Hosting my site on Wordpress was not super complex; I leveraged the Azure PaaS Services for Web Apps, and orginally the 3rd party support for hosted MySQL database’s. Once I was up and running I quickly realised that all media hosted on the site were landing on the webserver, so a plugin from its marketplace offered the ability to relocate the media to an Azure Blob; offloading some of the challanges.

Hosting this was not free, while I could have leveraged the Free Webserver option it did not take a lot of load for this to be causing some unacceptable performance issues; which when combined with a hosted MySQL service which also was not going to be super fast and was limited in the database size; which became event more of a problem when an attack on the site would result in 1000’s of useless comments filling the database to capacity and taking the site down as a direct result.

# Static Site Hosting

Not a lot has to change as we move to the static side model; The web server is still needed to host the side; but the database component is no longer a concern with this approach.

However, The cloud is a beautiful thing, and there are so many more ways to reach you goal, and while we are focused, we can complete a lot more for a lot less!

For this site I have chosen to run a lean cost model, while providing a super responsive experience to you, my readers and subscribers.

## Blob Storage Foundations

Using the standard **Azure Blob Storage** offering, I simply copy over the generated HTML site to the account. Assuming the blob is exposed to the public its content is available as a HTTPS endpoint; which simply put is a website.

But, alone this is not enough; why? because how would I address requested for pages which do not exist for example; I would not want an ugly HTTP 404 error page to be rendered, but instead a nice response to offer a search, navigation, or other more professional experience.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vNetId": {
      "type": "string",
      "defaultValue": "ScreenConnect"
    }
  },
  "variables": {
    "filterVNetId": "[ concat( '[concat(parameters(''virtualNetworkId''),''*'')]' ) ]"
  },
  "resources": [
    {
      "name": "vm-creation-in-approved-vnet-definition",
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-03-01",
      "properties": {
        "displayName": "Use approved vNet for VM network interfaces",
        "policyType": "Custom",
        "mode": "All",
        "description": "Use approved vNet for VM network interfaces",
        "metadata": {
          "category": "IaaS"
        },
        "parameters": {
          "virtualNetworkId": {
            "type": "string",
            "metadata": {
              "description": "Resource Id for the vNet",
              "displayName": "vNet Id"
            }
          }
        },
        "policyRule": {
          "if": {
            "allOf": [
              {
                "field": "type",
                "equals": "Microsoft.Network/networkInterfaces"
              },
              {
                "not": {
                  "field": "Microsoft.Network/networkInterfaces/ipconfigurations[*].subnet.id",
                  "like": "[variables('filterVNetId')]"
                }
              }
            ]
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    },
    {
      "name": "vm-creation-in-approved-vnet-assignment",
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "dependsOn": [
        "[resourceId('Microsoft.Authorization/policyDefinitions/', 'vm-creation-in-approved-vnet-definition')]"
      ],
      "properties": {
        "displayName": "Use approved vNet for VM network interfaces",
        "description": "Use approved vNet for VM network interfaces",
        "metadata": {
          "assignedBy": "Admin"
        },
        "scope": "[subscription().id]",
        "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'vm-creation-in-approved-vnet-definition')]",
        "parameters": {
          "virtualNetworkId": {
            "value": "[parameters('vNetId')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

To achieve this I need some HTTP routing options

## House Warming, Inviting Guests

It is not hard to find a list of potential solutions in Azure to fill this role; We could use a WebApp, API Managment, or event Azure Functions, or the Azure Functions proxy features. However, we do not actually need to be very creative; as Microsoft have listened to uservoice, MVPs and customers all calling for some better web publishing support for blob storage, especially given AWS have features in thier S3 offer that work simply and effectively.

To this end we will take a look at a feature which at the time of writing is still in preview called **Static Website** (You would have never guessed right!)

All we need do, is set this feature as *Enabled* and define where visitors should be routed to for the *Index Document* and the *Error Document*, and apply the changes. This simple feature makes our site behave just as we would like; and it is *free!*.

> Note: Due to Preview Status, we currently have no ARM Template settings for enabling and configuring this setting. Therefore for Infrasructure as Code we will fall back to Azure CLI

```shell
az extension add --name storage-preview
az storage blob service-properties update
  --account-name <ACCOUNT_NAME>
  --static-website
  --404-document <ERROR_DOCUMENT_NAME>
  --index-document <INDEX_DOCUMENT_NAME>
  
az storage account show
  -n <ACCOUNT_NAME>
  -g <RESOURCE_GROUP>
  --query "primaryEndpoints.web" --output tsv
```

## Foundation for Global Scale

But why stop here, we can go a little further; I want this site to be responsive for you, regardless of where you are located on the planet; afer all we are all embracing the cloud and need to learn and share; so how better to acomplish this, simply leverage another feature from the Azure arsnal of services.

Calling out the *Azure Content Delivery Network* (Azure CDN) we can take advantage, or any one of three platforms

* Verizon’s Global Network
* Akamai CDN Platform
* Microsofts Azure’s Footprint
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
    "storageName": "[tolower(take(concat('blob', parameters('name'), uniqueString(resourceGroup().id)), 23))]"
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

The template will setup out CDN environment, all we need to provide is a name for the CDN service, and the URI to the Static Site which we established in the previous stage, for example this may appear as `blobwebaddress.z00.web.core.windows.net`

## Adding Content

With very little effort, and cost, we now have established a foundation which can serve content efficiently globally, Our next challenge will be to deploy our site to this foundation in a easy manner

