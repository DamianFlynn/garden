---
title: "NodeMCU Pinouts"
date: "2018-11-18"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Categories:
  [
    "Azure Edge Devices"
  ]
Status: "Published"
Tags:
  [
    "ESP"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "4d0d2e1c-a9fd-4a28-88b3-cf1652a75bfa",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/184e4658-36ad-42a8-8167-244b7a5f4662/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=a99cd8043c6c2ba365a380ee131d9e9c096644b68f4a451a77178aec134f7708&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/NodeMCU-Pinouts-4d0d2e1ca9fd4a2888b3cf1652a75bfa",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:11:48.757Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
EXPIRY_TIME: "2025-05-14T13:11:46.923Z"
---

For the past year, I have been using a tiny board, known as the **NodeMCU** which is essentially a developer board for a module know as the **ESP8266**. The **NodeMCU** is formed by an **ESP12E**, which still has an **ESP8266EX** inside it.

![Image](img-4d0d2e1c-NodeMCU_ESP12E.jpg)

This device is really nice to work with, it is supplied preconfigured with a Micro USB input, for both programming and power.

>  The term NodeMCU usually refers to the firmware, while the board is called Devkit. NodeMCU Devkit 1.0 consists of an ESP-12E on a board, along with a voltage regulator, a USB interface. 
# ESP-12E

The board created by AI-THINKER, which consists of an *Espressif ESP8266EX* inside the metal cover. The microchip has a low-power consumption profile, integrated WiFi and the *RISC Tensilica L106 32bit Processor* has a maximum clock of 160 MHz

The following illustrates the pinouts on this board

![Image](img-4d0d2e1c-ESP-12E.jpg)

ESP-12E_Pinout

![Image](img-4d0d2e1c-ESP-12E_Pinout.jpg)

# IDE

I have so far being developing on this board using VS Code, and its integrations with the Arduino IDE. While this works well, I am currently considering alternative approach’s; as the debugging experience is far from optimal in my opinion; but that is work for another day.

The power of this board however, is understanding how to connect with outside world, and in this case what are the correct pin identifiers, trough reference of the NodeMCU datasheet and how the board boot’s.

# NodeMCU Pinout

The Devkit board which we are leveraging maps the pinouts from the ESP-12E module as follows:

![Image](img-4d0d2e1c-NodeMCU_ESP12E_Pinout.jpg)

Translating this however to the code we are going to develop in the Arduino sketch’s; we need to reference the pins with thier respective names; which is illustrated in this following image

![Image](img-4d0d2e1c-NodeMCU_ESP12E_Arduino.jpg)

Use the number that is in front of the GPIO or the constants as follows

  ## Pin IO Functions

When performing INPUT and OUTPUT tests on the pins, we obtained the following results:

  ## Flashing Sketch

The following simple sketch should flash an LED connected directly to the NodeMCU

```c
//Connect a testing LED to GPIO14 which is pin D5
#define LED D5

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  digitalWrite(LED, HIGH);    // Turn on the LED
  delay(1000);                // Wait 1 Second
  digitalWrite(LED, LOW);     // Turn off the LED
  delay(1000);                // Wait 1 Second
}
```

Using a very simple hook up example.

![Image](img-4d0d2e1c-LED_Connection.jpg)

Alternatively, without any external LED, we can set the LED to use the onboard LED with the mapping to `D0` or the constant `LED_BUILTIN`

>  Important: 
Please note that there are lots of generic ESP8266 boards and there is the possibility that some of them are sold under the name of NodeMCU and have different pin mappings. Besides that, there are different NodeMCU versions.

