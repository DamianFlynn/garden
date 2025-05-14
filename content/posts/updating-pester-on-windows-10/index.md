---
title: "Updating Pester on Windows 10"
date: "2019-01-29"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Status: "Published"
Tags:
  [
    "Powershell",
    "Pester"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "6e55c618-6d55-4e94-ad6e-134f102790d0",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/fc2a9715-5712-4f0b-a002-6fe9ef072141/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=454ad979528d9123dd231db7adbcfd10ea92755029a5e1558bc1f19b5ed764d0&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Updating-Pester-on-Windows-10-6e55c6186d554e94ad6e134f102790d0",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:56:54.451Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

I spend the majority of my time working on my Windows machines, and for many scenarios, I find it difficult to complain. However, when Windows decides to dig the boot in and not co-operative; usually is when I grab my Mac Book and get the work done.

However, running aware from the problem rarely is a good fix for the issue; My latest battle has been Pester. The testing framework builds on Powershell, and by the grace of God, now shipped as part of the Windows 10 operating system.

The issue is that the included version is 3.4 and at the time of writing the current release is 4.6. Typically, this is a non-issue a directly issuing an `Update-Module` command addresses the issue and allow the product to continue.

In the odd case we might need to revert to a push and include the `-Force` switch; but for various reasons, this sometimes also fails; which is the case in this Pester Module scenario

## The Hard Way

A little research identifies that over time, a few things have evolved with this module. In our case, the certificate used for signing the modules has changed, and for obvious reasons of security, the normal processes are failing.

To accomplish the objective in this case, I have reverted to removing the pre-installed version of pester, and then once a memory; I can proceed to deploy the newest release of this product.

Using an administrative PowerShell session, I proceed to issue the following commands; which define where the module in question is residing on my system and then assume ownership of the associated files, which I then delete. After all, Powershell modules are discovered, based on the search folder they have been installed within.

```powershell
$module = "c:\Program Files\WindowsPowerShell\Modules\Pester"
takeown /F $module /A /Ricacls $module /reset
icacls $module /grant Administrators:'F' /inheritance:d /T
Remove-Item -Path $Module -Recurse -Force -Confirm:$false
```

Now, we can put the hammer away, and proceed to deploy the module we originally required, this time without challenge.

```powershell
Install-Module -name pester -MinimumVersion 4.3.0
```

Happy Testing

