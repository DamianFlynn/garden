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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7882270c-803b-4cd6-a007-30ed96485f24/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=1cbcf505d1bdd2470695eeb694c6d1b1231eeb29bb98275b1859b3d34daff548&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.689Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/CBus-MQTT-Bridge-on-Raspberry-PI-55a555f6663940c4bac2306ea96ec881",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:36:35.399Z"
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

