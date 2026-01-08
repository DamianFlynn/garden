---
title: "Azure IaC - Appending Tags"
date: "2020-04-01"
lastmod: "2024-08-07T14:57:00.000Z"
draft: false
featuredImage: "./cover-e10b191b.jpg"
Categories:
  [
    "Infrastructre As Code"
  ]
series:
  [
    "Building Azure with Code"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/180b1cf4-6a14-4a9f-9cd8-95c2fb64ce3a/banner-Appending-tags.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122326Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=3161c62f042bd5dc1e99d4bb215a9a7ce8e79478c21861a8715e6d58f1486810&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-06T13:23:26.030Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "data_source_id",
      "data_source_id": "235a5f88-c313-46d9-84b5-9f168a1633b7",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Azure-IaC-Appending-Tags-e10b191b770c44a3a5f31d6c4da04403",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:26:31.107Z"
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

