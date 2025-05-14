---
title: "NodeMCU Pinouts"
date: "2018-11-18"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-4d0d2e1c.jpg"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/184e4658-36ad-42a8-8167-244b7a5f4662/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=bc41b17e7515f1d3ba37727d229f41d2c634fb141e5eda1a3cd58e43dcadc5f6&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/NodeMCU-Pinouts-4d0d2e1ca9fd4a2888b3cf1652a75bfa",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:37:19.679Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
EXPIRY_TIME: "2025-05-14T15:37:17.909Z"
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

