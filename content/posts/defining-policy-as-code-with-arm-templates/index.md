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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/21f3ca34-0054-4797-9f3c-20e5c4f1f269/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=3823d16834da630a1202cea2594f6944b75d3fd8ef27879e664db7d5346b1598&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.689Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Defining-Policy-as-Code-with-ARM-Templates-ac34fe35718f4eb8abf89c7266c4c33a",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:37:16.238Z"
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

