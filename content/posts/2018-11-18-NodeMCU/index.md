---
title: NodeMCU Pinouts
type: article 
layout: post 
description: Quick reference for the Pin connections on the NodeMCU boards
date: 2018-11-18 10:30:30
categories: ['Developer', 'Smart Home', 'SoC', 'Microcontrollers', 'IoT']
tags: ['Internet of Things']
authors: ['damian'] 
draft: false 
image: /images/2018/11/18/banner.jpg
toc: false 
featured: false 
comments: True
---

Over the last number of years I have deployed various Raspberry PI's around my home, to add more features to how we interact in the house. However, as wonderful as the PI is, there are alternative options available which are more appropriate to the various tasks which I need to address.

## NodeMCU

For the past year, I have been using a tiny board, known as the **NodeMCU** which is essentially a developer board for a module know as the **ESP8266**. The **NodeMCU** is formed by an **ESP12E**, which still has an *ESP8266EX* inside it.

![NodeMCU ESP12 Exploded View](2018-11-18-NodeMCU/opps-missing-image.png)

This device is really nice to work with, it is supplied preconfigured with a Micro USB input, for both programming and power.

> The term NodeMCU usually refers to the firmware, while the board is called Devkit. NodeMCU Devkit 1.0 consists of an ESP-12E on a board, along with a voltage regulator, a USB interface.

### ESP-12E

The board created by AI-THINKER, which consists of an *Espressif ESP8266EX* inside the metal cover. The microchip has a low-power consumption profile, integrated WiFi and the *RISC Tensilica L106 32bit Processor* has a maximum clock of 160 MHz

![ESP-12](2018-11-18-NodeMCU/opps-missing-image.png)

The following illustrates the pinouts on this board

![ESP-12E_Pinout](2018-11-18-NodeMCU/opps-missing-image.png)


## IDE 

I have so far being developing on this board using VS Code, and its integrations with the Arduino IDE. While this works well, I am currently considering alternative approach's; as the debugging experience is far from optimal in my opinion; but that is work for another day.

The power of this board however, is understanding how to connect with outside world, and in this case what are the correct pin identifiers, trough reference of the NodeMCU datasheet and how the board boot's.

### NodeMCU Pinout

The Devkit board which we are leveraging maps the pinouts from the ESP-12E module as follows: 

![NodeMCU_ESP12E_Pinout](2018-11-18-NodeMCU/opps-missing-image.png)

Translating this however to the code we are going to develop in the Arduino sketch's; we need to reference the pins with thier respective names; which is illustrated in this following image

![NodeMCU_ESP12E_Arduino](2018-11-18-NodeMCU/opps-missing-image.png)

Use the number that is in front of the GPIO or the constants as follows

|Constant | IO
|---|---|
|D0<br>LED_BUILTIN | GPIO16
|D1 | GPIO5
|D2 | GPIO4
|D3 | GPIO0
|D4 | GPIO2
|D5 | GPIO14
|D6 | GPIO12
|D7 | GPIO13
|D8 | GPIO15
|D9 | GPIO3
|D10 | GPIO1
|A0 | ADC


### Pin IO Functions

When performing INPUT and OUTPUT tests on the pins, we obtained the following results:

|Function   | GPIO
|digitalWrite| Working: 0, 1, 2, 3, 4, 5, 9, 10, 12, 13, 14, 15<br>Not Working: 6, 7, 8, 11, ADC
|digitalRead | Working: 0, 2, 4, 5, 9, 10, 12, 13, 14, 15<br>Not Working: 1, 3, 6, 7, 8, 11, ADC
|analogWrite | Software PWM: 0, 1, 2, 3, 5, 9, 10, 13<br>Hardware PWM: 4, 12, 14, 15<br>Not Working: 6, 7, 8, 11, ADC<br>
|analogRead  | Working: ADC


## Flashing Sketch

The following simple sketch should flash an LED connected directly to the NodeMCU

```arduino
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

![LED Connection](2018-11-18-NodeMCU/opps-missing-image.png)

Alternatively, without any external LED, we can set the LED to use the onboard LED with the mapping to `D0` or the constant `LED_BUILTIN`

> Important: Please note that there are lots of generic ESP8266 boards and there is the possibility that some of them are sold under the name of NodeMCU and have different pin mappings. Besides that, there are different NodeMCU versions.