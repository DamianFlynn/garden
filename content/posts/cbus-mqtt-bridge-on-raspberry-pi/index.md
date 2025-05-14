---
title: "CBus MQTT Bridge on Raspberry PI"
date: "2019-10-10"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7882270c-803b-4cd6-a007-30ed96485f24/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=e622de98e03a1a33b8dcb8d30ab7bfba8e875be057bee890ec8ed044f18a1c57&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-13T11:55:29.453Z"
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
UPDATE_TIME: "2025-05-13T10:56:46.575Z"
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

