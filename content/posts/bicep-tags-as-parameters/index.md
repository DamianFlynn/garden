---
title: "Bicep - Tags as Parameters"
date: "2021-07-23"
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
    "id": "b97ceb91-9261-4473-911d-bf9d435ad891",
    "created_time": "2024-07-11T10:42:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/46583960-2e02-45bd-9e94-5b7156e828d1/banner-BICEP_-_TAGS_AS_PARAMETERS.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=7641d2dd35ea529b19424e0d287305c3d1527279388cd58d03ab48239457aeb0&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.094Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Bicep-Tags-as-Parameters-b97ceb9192614473911dbf9d435ad891",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:12:41.302Z"
last_edited_time: "2024-08-07T14:57:00.000Z"
---

**Deploying infrastructure ARM Templates to Azure, but using Tags and their respective value as the parameter configuration settings**

In a post earlier, we look at using arm to lookup the value of tags' at both the Subscription and Resource Level.

With Bicep this is much easier to understand. This is the same lab configuration as in the original post, but this time to code should be a lot more readable.

```powershell
// Sample to lookup tag values 
// Both Subscription and Resource Level

@description('The resource ID of the resource we wish to look up a tag from.')
param sourceResourceId string = '/subscriptions/547d54ea-411b-459e-b6f8-b3cc5e84c535/resourceGroups/p-vm/providers/Microsoft.Compute/virtualMachines/p-vm001'


// Variables to reference the Tags Resource Providers

// Subscription Tags Resource Provider
var referenceSubscriptionTagsResourceId = '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Resources/tags/default'

// Resource Tags Resource Provider
var referenceResourceTagsResourceId = '${sourceResourceId}/providers/Microsoft.Resources/tags/default'
var referenceTagsApi = '2020-06-01'


// 
// Lookup the tags and return the value
// 

// Subscription Tags
output subscriptionRecoveryVaultRGTag string = reference(referenceSubscriptionTagsResourceId, referenceTagsApi).tags.recoveryVaultRG
output subscriptionRecoveryVaultTag string = reference(referenceSubscriptionTagsResourceId, referenceTagsApi).tags.recoveryVault

// Resource Tags
output resourceRecoveryPolicyTag string = reference(referenceResourceTagsResourceId, referenceTagsApi).tags.recoveryPolicy


// Concatanation of the outputs to build Resource Ids

output recoveryVaultId string = resourceId(subscription().subscriptionId, reference(referenceSubscriptionTagsResourceId, referenceTagsApi).tags.recoveryVaultRG, 'Microsoft.RecoveryServices/vaults', reference(referenceSubscriptionTagsResourceId, referenceTagsApi).tags.recoveryVault)
output recoveryPolicyId string = '${resourceId(subscription().subscriptionId, reference(referenceSubscriptionTagsResourceId, referenceTagsApi).tags.recoveryVaultRG, 'Microsoft.RecoveryServices/vaults', reference(referenceSubscriptionTagsResourceId, referenceTagsApi).tags.recoveryVault)}/backupPolicies/${reference(referenceResourceTagsResourceId, referenceTagsApi).tags.recoveryPolicy}'
```


