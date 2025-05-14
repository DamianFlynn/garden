---
title: "Azure IaC - Appending Tags"
date: "2020-04-01"
lastmod: "2024-08-07T14:57:00.000Z"
draft: false
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/180b1cf4-6a14-4a9f-9cd8-95c2fb64ce3a/banner-Appending-tags.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=27879c5033892fd8c9dd1115b479ce996f1f4a4a1c837a703001e7332367ef30&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.039Z"
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
UPDATE_TIME: "2025-05-14T12:12:40.488Z"
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

