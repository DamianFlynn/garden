---
title: "Azure IaC - Function Keys"
date: "2020-04-22"
lastmod: "2024-07-19T15:22:00.000Z"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/aa9451e1-603f-4c42-9341-2633f7287f76/banner-iac-functionkeys.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=885550ba25dfad666c84e7a12f14c34bdb5464b40b32037a89d16b300d43a57f&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.038Z"
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
UPDATE_TIME: "2025-05-14T12:12:10.996Z"
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


