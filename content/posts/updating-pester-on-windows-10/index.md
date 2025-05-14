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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/fc2a9715-5712-4f0b-a002-6fe9ef072141/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=0d5b68422fff4a76516d7a62feb8d536bfb46b0a3104fe1797505fbec9eb59f4&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Updating-Pester-on-Windows-10-6e55c6186d554e94ad6e134f102790d0",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:36:49.260Z"
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

