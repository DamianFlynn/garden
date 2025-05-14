---
title: "Running FastLED on the Dual-Core ESP32"
date: "2019-09-11"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Status: "Published"
Tags:
  [
    "ESP",
    "IoT"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "aa34ce01-aa48-401d-b13f-63a6ce01eced",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/3bdb5b7c-307b-4d35-871a-5ffb820c6684/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=09d190a3816c538bb3c678a1158b30a476b1f8848cb332b852a2311765bada16&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Running-FastLED-on-the-Dual-Core-ESP32-aa34ce01aa48401db13f63a6ce01eced",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:11:20.772Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

There are many projects posted over the web which implement the excellent FastLED library on the ESP12 processor; however locating a project which implements this on the more powerful sibling is a lot more difficult.

So, with a few failed attempts and a lot of patching samples together; I have a stable running implementation which you can clone or fork to get up and running quickly with your own projects.

The sample includes 2 different sequences, a simple moving dot; and a more colourful Cylon effect.

The code is complied within Visual Studio Code; with the Platform.IO environment; and includes a working settings file while will automatically install the required libraries, ready to compile and flash to your device.

Check out the repo on GitHub @ [https://github.com/DamianFlynn/ESP32FastLED](https://github.com/DamianFlynn/ESP32FastLED); and if you have issues please use the tracker.

Enjoy

