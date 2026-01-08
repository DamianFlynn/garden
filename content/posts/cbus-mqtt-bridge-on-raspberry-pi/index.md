---
title: "CBus MQTT Bridge on Raspberry PI"
date: "2019-10-10"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-55a555f6.jpg"
Status: "Published"
Tags:
  [
    "IoT",
    "Linux"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "55a555f6-6639-40c4-bac2-306ea96ec881",
    "created_time": "2024-07-11T14:01:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7882270c-803b-4cd6-a007-30ed96485f24/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=a7275bacd019ba7495ca3d7cf8bbb7471304c560aab496933d497c425473b517&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
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
    "url": "https://www.notion.so/CBus-MQTT-Bridge-on-Raspberry-PI-55a555f6663940c4bac2306ea96ec881",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:25:49.572Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
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

