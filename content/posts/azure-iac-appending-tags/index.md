---
title: "Azure IaC - Appending Tags"
date: "2020-04-01"
lastmod: "2024-08-07T14:57:00.000Z"
draft: false
Categories:
  [
    "Infrastructre As Code"
  ]
Status: "Published"
Tags:
  [
    "IaC",
    "ARM",
    "Bicep"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "e10b191b-770c-44a3-a5f3-1d6c4da04403",
    "created_time": "2024-07-11T10:46:00.000Z",
    "last_edited_time": "2024-08-07T14:57:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/180b1cf4-6a14-4a9f-9cd8-95c2fb64ce3a/banner-Appending-tags.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=68915a72fbf9366eb6242c6163432de2549ee6b4d95cb517cdeba2c8d6b3df50&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-13T11:55:29.454Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Azure-IaC-Appending-Tags-e10b191b770c44a3a5f31d6c4da04403",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:47.005Z"
last_edited_time: "2024-08-07T14:57:00.000Z"
---

**Dynamically appending Tags to our ARM template with the union function**

Todays conundrum: As I am leveraging templates, there will always be some standard tags I require to implement within the template, but I also require to provide additional tags as a parameter to be appended with the deployment.

My objective is to set up tags within an ARM template in accordance with good governance and the Cloud adoption framework.

# Solution

ARM Template functions to the rescue. Todays salvation is called `union`, which you can learn more about on the actual [reference Site](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-functions-array#union)

This is the existing implementation

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "configVersion": {
      "type": "string",
      "defaultValue": "1.0.0.0",
      "metadata": {
        "description": "The version of the parameters file that holds the configuration."
      }
    },
    "purpose": {
      "type": "string",
      "allowedValues": [
        "diag",
        "log",
        "audit",
        "data"
      ],
      "defaultValue": "data",
      "metadata": {
        "description": "The designated purpose of the storage account. 'diag' for diagnostics, 'log' for logging, 'audit' for auditing, 'data' for data storage"
      }
    },
    "resilience": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Choose a level of resilience and tier suitable for the purpose and region"
      }
    },
    "tier": {
      "type": "string",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "defaultValue": "Standard",
      "metadata": {
        "description": "Choose tier, Standard or Premium."
      }
    },
    "kind": {
      "type": "string",
      "allowedValues": [
        "Storage",
        "StorageV2",
        "BlobStorage",
        "FileStorage",
        "BlockBlobStorage"
      ],
      "defaultValue": "StorageV2",
      "metadata": {
        "description": "Choose a kind of storage account"
      }
    }
  },
  "variables": {
    "iacVersion": "undefined",
    "rgName": "[resourceGroup().name]",
    "rgLocation": "[resourceGroup().location]",
    "uniqueString": "[uniqueString(subscription().id, resourceGroup().id)]",
    "storageAccountAffix": "[concat(replace(variables('rgName'), '-', ''), parameters('purpose'))]",
    "storageAccountName": "[toLower(substring(replace(concat(variables('storageAccountAffix'), variables('uniqueString')), '-', ''), 0, 23) )]"
  },
  "resources": [
    {
      "comments": "~~ Storage Account  ~~",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "sku": {
        "name": "[parameters('resilience')]",
        "tier": "[parameters('tier')]"
      },
      "kind": "[parameters('kind')]",
      "location": "[variables('rgLocation')]",
      "tags": {
        "IaCVersion": "[variables('iacVersion')]",
        "ConfigVersion": "[parameters('configVersion')]"
      },
      "scale": null,
      "properties": {
        "supportsHttpsTrafficOnly": true,
        "encryption": {
          "services": {
            "file": {
              "keyType": "Account",
              "enabled": true
            },
            "blob": {
              "keyType": "Account",
              "enabled": true
            }
          },
          "keySource": "Microsoft.Storage"
        },
        "accessTier": "[if(equals(parameters('kind'), 'Storage'), json('null'),'Hot')]"
      },
      "dependsOn": [
      ]
    }
  ],
  "outputs": {
    "storageId": {
      "type": "string",
      "value": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    },
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## The Change Set

And using our new `union` we can resolve the puzzle with 3 simple changes

1. To the parameter we add a new object
  ```json
      "tagValues": {
        "type": "object",
        "defaultValue": {
          "Dept": "Undefined",
          "Environment": "Development"
        }
      },
  ```
  
  1. In the variables we can define our default tags
  ```json
      "defaultTag": {
        "IaCVersion": "[variables('iacVersion')]",
        "ConfigVersion": "[parameters('configVersion')]"
      },
  ```
  
  1. And in the resource we can use the union function to merge the objects together
  ```json
    "tags": "[union(parameters('tagValues'),variables('defaultTag'))]",
  ```
  
  ## The Final Result

The final template will look as follows

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "configVersion": {
      "type": "string",
      "defaultValue": "1.0.0.0",
      "metadata": {
        "description": "The version of the parameters file that holds the configuration."
      }
    },
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Undefined",
        "Environment": "Development"
      }
    },
    "purpose": {
      "type": "string",
      "allowedValues": [
        "diag",
        "log",
        "audit",
        "data"
      ],
      "defaultValue": "data",
      "metadata": {
        "description": "The designated purpose of the storage account. 'diag' for diagnostics, 'log' for logging, 'audit' for auditing, 'data' for data storage"
      }
    },
    "resilience": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Choose a level of resilience and tier suitable for the purpose and region"
      }
    },
    "tier": {
      "type": "string",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "defaultValue": "Standard",
      "metadata": {
        "description": "Choose tier, Standard or Premium."
      }
    },
    "kind": {
      "type": "string",
      "allowedValues": [
        "Storage",
        "StorageV2",
        "BlobStorage",
        "FileStorage",
        "BlockBlobStorage"
      ],
      "defaultValue": "StorageV2",
      "metadata": {
        "description": "Choose a kind of storage account"
      }
    }
  },
  "variables": {
    "iacVersion": "undefined",
    "defaultTag": {
      "IaCVersion": "[variables('iacVersion')]",
      "ConfigVersion": "[parameters('configVersion')]"
    },
    "rgName": "[resourceGroup().name]",
    "rgLocation": "[resourceGroup().location]",
    "uniqueString": "[uniqueString(subscription().id, resourceGroup().id)]",
    "storageAccountAffix": "[concat(replace(variables('rgName'), '-', ''), parameters('purpose'))]",
    "storageAccountName": "[toLower(substring(replace(concat(variables('storageAccountAffix'), variables('uniqueString')), '-', ''), 0, 23) )]"
  },
  "resources": [
    {
      "comments": "~~ Storage Account  ~~",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "sku": {
        "name": "[parameters('resilience')]",
        "tier": "[parameters('tier')]"
      },
      "kind": "[parameters('kind')]",
      "location": "[variables('rgLocation')]",
      "tags": "[union(parameters('tagValues'),variables('defaultTag'))]",
      "scale": null,
      "properties": {
        "supportsHttpsTrafficOnly": true,
        "encryption": {
          "services": {
            "file": {
              "keyType": "Account",
              "enabled": true
            },
            "blob": {
              "keyType": "Account",
              "enabled": true
            }
          },
          "keySource": "Microsoft.Storage"
        },
        "accessTier": "[if(equals(parameters('kind'), 'Storage'), json('null'),'Hot')]"
      },
      "dependsOn": [
      ]
    }
  ],
  "outputs": {
    "storageId": {
      "type": "string",
      "value": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    },
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

Now, Feel the force, and see what you can create…

