---
title: "Azure IaC - Function Keys"
date: "2020-04-22"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
Categories:
  [
    "Infrastructre As Code"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/aa9451e1-603f-4c42-9341-2633f7287f76/banner-iac-functionkeys.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=ce35ee2341066856ad476912fc94264573c334a10d48b527ac9f76950dea2919&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Azure-IaC-Function-Keys-459a98643c794926949e879c77be2673",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:32.988Z"
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


