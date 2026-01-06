---
title: "CBus MQTT Bridge on Raspberry PI"
date: "2019-10-10"
lastmod: "2026-01-06T22:21:00.000Z"
draft: false
featuredImage: "./cover-2e0eb56e.jpg"
series: "Smart Buildings"
Tags:
  [
    "IoT",
    "Linux",
    "MQTT"
  ]
Status: "Published"
summary: "Unlock the potential of your smart home with the CBus MQTT Bridge on Raspberry Pi, a comprehensive guide that details how to integrate Clipsal's C-Bus lighting system into your IoT setup, enhancing automation and control like never before!"
Categories: "IoT"
NOTION_METADATA:
  {
    "object": "page",
    "id": "2e0eb56e-a1c3-800b-8025-e2b170055919",
    "created_time": "2026-01-06T17:34:00.000Z",
    "last_edited_time": "2026-01-06T22:21:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7882270c-803b-4cd6-a007-30ed96485f24/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZUKSQ5GC%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T225933Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHdZ%2BeR8TBWHRrYEcamSgP12ab4wks%2F6aOTIN3lGsv14AiEAsKnrybeAneWNRhOF4HI0ye4F6QwBBJbxDp3J0gTd%2FX0q%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDM2QUxy9kNzK0RP0syrcA0qquR9ds7R7%2BuEs%2BNIoW1aTjlaFMSVonSnnfdvVs30PSFR%2B2dpg4VVgCUyXiCSA95D338wqaqAQcgo6StIChki6tH2nRMk3dH2chq0RdKlTmq2ExXBTd3ZOgQ4KaqU7Cf4Zjgkko%2BEqGNJrYQiJsiIRLl7UQ26%2BgAnNfSBiKqI43dZaOvy3VGsHbdYcDzccB%2BZd6bjV4cvvpxWrZHxV3sueQptXrh0sHJr4nsVufdeLbPGUfmMcGehh63Q%2F0TMAZuwihqfOhQMajlKChhPEdqD6U6ZYHRuIhyZuEAyFGNCsBphujGejCOKTV%2BlEhki85RQJtOQHiPL2V3z66yZigyQViQ3LwRkzur7LXb6IwPhCDqCU67btBsCMIFXa9mZbBwL7zei4cI6j7ex6q1YLznKDMB1WdKdOpbFGL5zfOQrCF6XytGK3Ju%2FKunZSfEbNzUlX5FlwToBLNG6bQfG7IZakz9kbl8kn3FCjF4atOLc7PtwYSTjnPGcYculVN%2BV4Zby9DDk4WWv%2Bi6B3orUkbea7BVtpfqkO8CDEiJoa%2BezFKjBqcDXeZNiGobGoZsj%2F2H1nvCoEwqUtIDLk6IfGxFnqxFfnSJEcugObOfPzqwcZT1i%2F50db6LvezQb7MOuh9soGOqUBdLho7s0k8SRF3%2FV33gCyEhIfNX4q8g7oQiuYzYnuhuCDjvBF8gvczi6u%2Fo3z1I4%2Fg3fn2B%2FVx3Bmt41XT4l13UgYRMJdLJNVcHupZ59G5IvJVTyGQOI1VyM2h9tkZJKoOCmR%2B33RHNIMQRWyLLkiN3mwu7iJOu%2Bt0tZDWoxl0KkLPMLTU9om5PHINs715vkNzPjVJXE7MfbPZmk4AuY8nP6jZecP&X-Amz-Signature=f66a3d56a1e54b1610af638a10a01e8963fdb3bee6efec59db9c60d48adce3e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-06T23:59:33.813Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "data_source_id",
      "data_source_id": "0cb08ce3-4a92-421c-8f01-a3d2151ce62e",
      "database_id": "f9f77549-5ec7-48f1-a25a-aaeed54650ab"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/CBus-MQTT-Bridge-on-Raspberry-PI-2e0eb56ea1c3800b8025e2b170055919",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T22:59:36.490Z"
last_edited_time: "2026-01-06T22:21:00.000Z"
---

Turn back to 2007; My wife and I built our home, integrating many smart technologies, including the Clipsal C-Bus lighting system. This solution is classified as a Prosumer technology, and is designed to integrate into whole house automation systems.

The C-Bus system implements however a propriatory technology, and utilizes a communication protocol which is not ‘open source’; however, accepting a licence agreement will permit access to this protocol for creating an programming interface.

To simplify (arguable) the process of integrating with the CBus environment Clipsal released a Bridge solution which enables a TCP interface using a special Java application called ‘C-GATE’.

Using a Raspberry Pi, with a USB to RS-232 cable, which is then connected to a Clipsal interface called the *Serial PCI Module*,

# Prerequisites

Deploy the current relase of Rasbian for your Pi.

> Update: Jan 2021 - Currently using Rasbian 2021-01-11

Once the Pi have been configured and added to the network, we can connect via SSH, and begin installing the pre-requisites for our gateway.

```shell
# Serial 2 Socket Build Tools
sudo apt-get install git build-essential autotools-dev devscripts libssl-dev

# Java 8 Runtime for CGate
sudo apt-get installopenjdk-8-jdk

# Node and NPM for CGateWeb
sudo apt-get installnode npm
```

# TCP to Serial Bridge

Configuring the CBus system over TCP however will not work with just C-Gate alone, we need to also establish a TCP connection directly to the *Serial PCI Module*.

## Serial To Socket

This requires that we compile and run a small *C* application (Don’t worry, this is painless and fast); It took a lot of searching to find, posted the source to the following GIT repository; the application is called **ser2sock**.

Here’s the steps to get it set up:

```shell
git clone https://github.com/nutechsoftware/ser2sock.git

cd ser2sock
./configure --without-sslcc -o ser2sock ser2sock.c
sudo mv ser2sock /usr/local/bin

cd /usr/local/bin/ser2sock
sudo chown -R pi:pi /usr/local/bin/ser2sock
```

## Running as a service - System.d

The source offers us a sample init script which we can use for starting the service. First we will place this in the SystemD folder, and then update it to match our requirements for C-Gate

```shell
sudo cp ~/ser2sock/init/systemd/ser2sock.service /etc/systemd/system
```

Update the startup script, `/etc/systemd/system/ser2sock.service` to have the daemon auto-start with the required C-Gate port, which is TCP 10001, and the serial interface baud rate set to 9600.

```shell
[Unit]
Description=Proxy that allows tcp connections to serial ports
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/ser2sock -p 10001 -s /dev/ttyUSB0 -b 9600 -d
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

Then activate using:

```shell
sudo systemctl enable ser2sock.service
sudo systemctl start ser2sock.service
```

# Installing C-Gate

[Download](https://updates.clipsal.com/ClipsalSoftwareDownload/mainsite/cis/technical/CGate/cgate-2.11.4_3251.zip) the current software release of C-Gate off the clipsal website and unzipped the files into `/usr/local/bin/cgate.`

```shell
cd ~
wget https://updates.clipsal.com/ClipsalSoftwareDownload/mainsite/cis/technical/CGate/cgate-2.11.4_3251.zip
unzip cgate-*.zip
sudo mv cgate /usr/local/bin
```

## Running C-Gate as Service - System.d

Adding the following ‘system.d’ startup script, to have the daemon auto-start with the operating system `/etc/systemd/system/cgate.service`

```shell
[Unit]
Description=Clipsal CBUS Gateway
After=syslog.target network.target[Service]
ExecStart=/usr/bin/java -Djava.awt.headless=true -jar -noverify /usr/local/bin/cgate/cgate.jar
Restart=always
User=root
Group=root
Environment=PATH=/usr/bin:/usr/local/bin
WorkingDirectory=/usr/local/bin/cgate/

[Install]
WantedBy=multi-user.target
```

Then activate using

```shell
sudo systemctl enable cgate.service
sudo systemctl start cgate.service
```

## C-Gate Access Control

We must configure the C-Gate service to allow remote connections from machines on the network by editing the access control file `nano /usr/local/bin/cgate/config/access.txt`; adding a line, providing the ip address of the remote system. In the example I am allowing the network 172.16.0.0/23 or the IP’s in the range of 172.16.0.0 to 172.16.255.255

```shell
echo "remote 172.16.255.255 Program" >> /usr/local/bin/cgate/config/access.tx
```

Save, then restart cgate:

```shell
sudo systemctl restart cgate.service
```

## C-Gate Auto-Connect

Edit the file `/usr/local/bin/cgate/config/C-gateConfig.txt`. Set the **project.default** and **project.start** lines to the name of the C-Gate Project.

# CGate MQTT

MQTT interface for C-Bus lighting written in Node.js

```shell
cd /usr/local/bin
sudo git clone https://github.com/the1laz/cgateweb.git
cd cgateweb
npm install
sudo nano settings.js
```

Put in your settings.

```shell
sudo cp cgateweb.service /etc/systemd/system
sudo systemctl enable cgateweb
sudo systemctl start cgateweb
```

