---
title: "Defining Policy as Code with ARM Templates"
date: "2018-11-20"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-ac34fe35.jpg"
Categories:
  [
    "Azure Management"
  ]
Status: "Published"
Tags:
  [
    "ARM",
    "Policy",
    "Governance"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "ac34fe35-718f-4eb8-abf8-9c7266c4c33a",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/21f3ca34-0054-4797-9f3c-20e5c4f1f269/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=a6a48720d638e60b36ab416999120510f24bef2b7206a7004b9621131ce9ff55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-06T13:23:25.972Z"
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
    "url": "https://www.notion.so/Defining-Policy-as-Code-with-ARM-Templates-ac34fe35718f4eb8abf89c7266c4c33a",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:26:09.381Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

My colleagues and friends [Tao Yang](https://blog.tyang.org/2018/06/06/using-arm-templates-to-deploying-azure-policy-definitions-that-requires-input-parameters/) , and [Stanislav Zhelyazkov](https://cloudadministrator.net/2018/07/17/defining-input-parameters-for-policy-definitions-in-arm-template/) have both recently posts interesting topics on how to implement your Azure Policy as Code which I strongly recommend you take a few moments to review
* [Using ARM Templates to deploy azure policy definitions that require input parameters](https://blog.tyang.org/2018/06/06/using-arm-templates-to-deploying-azure-policy-definitions-that-requires-input-parameters/)
* [Defining input parameters for policy definitions in ARM Templates](https://cloudadministrator.net/2018/07/17/defining-input-parameters-for-policy-definitions-in-arm-template/)

# Improving Readability

Both of these topics address the core of the challenges we face when approaching policy as an Infrastructure as Code problem. However, one of the things that is lost in the translation is the readability of the templates which they are deploying.

Not to reinvent the wheel, I am going to use the same template which Stan presented in his post, and make a small tweak to the process which he has employed to deal with the `'` *single quote* problem!

ARM follows most of the standard JSON escape sequences, therefore the following examples are quite useful

## Escaping a Single quote

Azure ARM behaves nicely with a simply doubling the single quote characters; just as we apply in Visual Basic.

`[concat('This is a ''quoted'' word.')]` which then provides the output of `This is a 'quoted' word.`

### Escaping a Double quote

For the Double quotes, we use the normal escape character `/`.

`[concat('''single'' and \"double\" quotes.')]` will render the output as follows `'single' and "double" quotes.`

## The Solution

With this simple trick, we can replace the *variables* definition which the guys used with the following snippet:

```json
  "variables": {
     "filterVNetId": "[ concat( '[concat(parameters(''virtualNetworkId''),''*'')]' ) ]"
   }
```

I am sure that this is a lot easier to read, and therefor debug; So the full template would look as follows

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

## Summary

Using native escape sequences in ARM assist in the overall readability of the final code.

Thank you Tao and Stan for the inspiration

