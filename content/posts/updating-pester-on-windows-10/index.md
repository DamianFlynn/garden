---
title: "Updating Pester on Windows 10"
date: "2019-01-29"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-6e55c618.jpg"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/fc2a9715-5712-4f0b-a002-6fe9ef072141/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=e359f23ca2c99eac76a5d5060d97b316bd09a2af7e363516c361a0f039dbada3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
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
    "url": "https://www.notion.so/Updating-Pester-on-Windows-10-6e55c6186d554e94ad6e134f102790d0",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:25:51.491Z"
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

