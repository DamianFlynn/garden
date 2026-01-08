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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7882270c-803b-4cd6-a007-30ed96485f24/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFRQGMOZ%2F20260108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260108T125211Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCNRuAlg0q%2FUEQq9p57i2BGkzEAMVyretF1acdSNRcbEAIgNH0ig4glv2jF8d0nABesB9QSNseSd7R7rL3mI%2FgwmqQqiAQIjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDArrvfeNMKGHRF%2B%2B%2BCrcA%2BJN5EVt9%2BB4SwcAA%2BtohgYU79JCH7KC9xph8hZgfmspYwtn0O6zxEY7hIzeS5PXimx4Jw7JeW0FMpOZ1uNpXNCfBZCjHYuZkcY1paiSB1iiin6wce2otHWfgUoT4dmwPm%2FU3iJrLPTw8jqJnV9fpkKgZiEVb8sU1AwXAvXRdn%2BJCSQ9we8Iu4CsFKXGP1hHF8UYW4fL8qYij1Cre7wA2jmzI4GpZCRHNuP7Vzk4kH0SdyvRuS1xN0l%2F0CfCJjTxvAqFeZP0nuAoC%2BD3JjMjkmmvUC1ruK1sAL4eyi%2FcLp1Y%2B%2FoALyqppYUw0pcOinMOa3DuuBkIViSLfjopMdJgoi1ylholOvrGU1TXASlcSvQDSq31ctpT3vmRu1Z3UGex2j%2FaFEb0cuErxzXyXoGWrHltiThZG38iBXKyScNcoxbZ1J0dNmk9FSolmizuwIHm3LL8pRdq2p2BQAWYnO3RgrEXH8ZB7Z8zp2OahjIRFhB73WwhloiF5G4CQJVvf3hLcvFahrRVnX%2B5IUqlX7RcwHHldJqmw8aieHQwAxLBuxIMleAI6IiTnJkGgsp0U2InHrq7%2FxwJ7AsBbJEMe4evsAqVIt2sMiIestt%2BZgoDVeaPTdgf2d82eszZucimMNbC%2FsoGOqUB%2B9tbYqjxD2Cod%2BojFWEuzJUkSGxplkyqeH50Sf839HQpjS1YTo2IaPwyItoS85mboSX6LT95Cy57ZwP8AyfOyO1bakP7d%2FNBbBG4DiW0GHq6yF89Kg40jAQzKMRIK3pEbao4IF3wn0TjncCZLCJNjFLAD7LUg%2FSoYpL1Jm6N0Y3SRZm6Dad6w6SKVkrT0s1FavbGV%2BT4YowvTlixR8DOia4bBp8I&X-Amz-Signature=48ca4c816ff6a20f05d11798bd14a9b2872a32ddf7898a53642dc40f097037bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-08T13:52:11.847Z"
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
UPDATE_TIME: "2026-01-08T12:52:12.888Z"
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

