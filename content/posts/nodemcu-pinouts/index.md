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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/184e4658-36ad-42a8-8167-244b7a5f4662/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=6a9f645c9d9dbf4b57ed7291dd0a1d3bd66d32573151ceef49b85a0fa8fa3f1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
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
    "url": "https://www.notion.so/NodeMCU-Pinouts-4d0d2e1ca9fd4a2888b3cf1652a75bfa",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:25:44.366Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
EXPIRY_TIME: "2026-01-06T13:25:39.977Z"
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

