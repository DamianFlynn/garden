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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7882270c-803b-4cd6-a007-30ed96485f24/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=c7119d710b8f7294d9dea5c0d7f81fa0d27805ceb89e713d35920288ca5e5b4f&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/CBus-MQTT-Bridge-on-Raspberry-PI-55a555f6663940c4bac2306ea96ec881",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:11:04.622Z"
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

