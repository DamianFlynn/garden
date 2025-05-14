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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/fc2a9715-5712-4f0b-a002-6fe9ef072141/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=900e38da9fcc231a404a5e3b08c6400fdd5d3d061e0009bcc72da8bb49fac8d6&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.037Z"
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
UPDATE_TIME: "2025-05-14T12:11:21.484Z"
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

