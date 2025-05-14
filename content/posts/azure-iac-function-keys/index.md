---
title: "Azure IaC - Function Keys"
date: "2020-04-22"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
featuredImage: "./cover-459a9864.jpg"
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
    "ARM"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "459a9864-3c79-4926-949e-879c77be2673",
    "created_time": "2024-07-11T13:36:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/aa9451e1-603f-4c42-9341-2633f7287f76/banner-iac-functionkeys.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=58a6a1814bd11287fe78b9c43b64fdd68f72b3a4565e69a2b89bcfcc62cc4956&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.690Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Azure-IaC-Function-Keys-459a98643c794926949e879c77be2673",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:37:49.901Z"
last_edited_time: "2024-07-19T15:22:00.000Z"
---

**Retrieve the Function Host Keys while deploying an ARM template**

Todays conundrum: As I deploy a new Function Application, I need a simple methodology to retrieve the Host Keys for the function application so that I validate the deployment has been successful; and potentially pass on the key to related services, for example API Management.

As before, I am leveraging templates, and will stay cloud native; this time depending on the functions Output ability to present the keys.

## Solution

A not well know ARM resource is going to be our Hero in this journey. The resource is a called `Microsoft.Web/sites/host/functionKeys` and you can gain a little (actually almost none) more details on the Microsoft [reference site](https://docs.microsoft.com/en-us/azure/templates/microsoft.web/2018-02-01/sites/functions/keys).

> The Documentation refer the API release 2018-02-01; But as you will see in the example code; I discovered that a slightly newer version available.

The Key to this deployment is the following resource definition

```json
    "resources": [
        {
            "comments": "~~ Function App Keys  ~~",
            "type": "Microsoft.Web/sites/host/functionKeys",
            "apiVersion": "2018-11-01",
            "name": "[concat(variables('webappName'), '/default/apimanagement')]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites/', variables('webappName'))]"
            ],
            "properties": {
                "name": "api-management"
            }
        }
    ]
```

Let’s inspect this closer

  After this resource has been deployed, we then will reference this in our template `output` section

```json

    "outputs": {
        "functionKey": {
            "type": "string",
            "value": "[listkeys(concat(resourceId('Microsoft.Web/sites', variables('webappName')), '/host/default/'),'2016-08-01').functionKeys.apimanagement]"
        }
    }
```

In the `value` section of this output, we are getting a reference to the Function Applications resource identity we just deployed, and with this leverage the ARM Function of `listkeys` to return the function key for the resource we identified on Line 5 in the earlier snippet.

With this output, we can now chain this as the input for additional templates, or reference the value for our scripts.


