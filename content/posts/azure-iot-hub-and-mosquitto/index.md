---
title: Azure Iot Hub And Mosquitto
date: '2026-07-27'
draft: true
slug: azure-iot-hub-and-mosquitto
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/azure-iot-hub-and-mosquitto/index.md
neocortex_source_sha: 6a1bdc7657e0d64040df368cceeefe48836334b02ccff6907fb09bb671021950
description: ''
tags: []
---

## References
 
https://liupeirong.github.io/mqttIoTHub/

https://automation.baldacchino.net/category/mqtt/

https://automation.baldacchino.net/monitoring-solar-consumption-with-an-esp32-8266-and-led-lights/
https://automation.baldacchino.net/publishing-mqtt-messages-from-mosquitto-to-microsoft-azure-iot-hub/
https://automation.baldacchino.net/building-a-bridge-between-mosquitto-mqtt-broker-and-azure-iot-using-paho-mqtt-and-the-azure-iot-sdks/
https://automation.baldacchino.net/replacing-mosquitto-with-azure-iot-edge/

### Debugging
https://stackoverflow.com/questions/35863841/connecting-mosquitto-to-the-new-azure-mqtt-backend


## Post

Public cloud brings a paradigm shift in what can be done, the art of the possible is possible and in the context of this post, I am planning to connect my existing MQTT-based Smart Home system to Azure.

From ML (Machine Learning) through to anomaly detection and everything in between, the bells and whistles Azure graces myself is far and beyond the capabilities of any on premise system, coupled with low administrative effort of consuming a managed service.

Who has the longest showers? Who wakes up first? Which devices are powered on, yet there is no activity? These questions and many can all be answered from the thousands events occurring each day in my house, but first we need to get these events in to the Cloud, a.k.a. Azure.

The thing is whilst familiar with MQTT, I am new to Azure IoT (and Azure in general). So, in this multi-part series of blog posts, let's learn together as I walk you through my journey.

## The inception

Today I use [Mosquitto](https://mosquitto.org/) (in Docker container) locally with [Home Assistant](https://www.home-assistant.io/), [Tasmota](https://tasmota.github.io/docs/) devices, a [PLC](https://en.wikipedia.org/wiki/Programmable_logic_controller) and an [Arduino Mega](https://store-usa.arduino.cc/products/arduino-mega-2560-rev3). I publish and consume thousands of messages each day (1.4 messages per second, which is mostly [PIR](https://en.wikipedia.org/wiki/Passive_infrared_sensor) data). The easiest path here is to not have these devices publish to Azure IoT Hub (the Azure service used as a Cloud gateway for IoT devices) directly, but to have Mosquitto replicate all messages locally through to Azure IoT Hub.

Remember the three laws of IoT:

1.  **The Law of Physics :** Latency to the cloud can be unacceptable, think crash avoidance system and medical alerts. So some decision-making must continue to be executed locally on the device. High-value and safety-critical processes must always continue working.
    
2.  **The Law of Economics :** The cost of bandwidth is not falling as fast as the cost of storage and compute.
    
    We might act on low value data locally and upload high value and aggregate data to the cloud for analytics and storage.
    
3.  **The Law of the Land:** For legal or compliance reasons, and concerns around privacy, some industries prefer to store data locally more-so, some governments impose data sovereignty restrictions on where data may be stored.  
      
    

But here, it’s my snarky 10-year-old wondering why there is so much lag when a button is pressed (The law of physics). Therefore, some decision-making must continue to be executed locally and in the absence of Internet connection, my house must continue to function. Ideally something like this image below, once we are publishing events in Azure IoT Hub we are then able to leverage these in the Azure platform as described above.

![thumbnail image 1 captioned What I want to do](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/316088iC216C60E7F3B1AD1/image-size/large?v=v2&px=999)What I want to do

The thing is, you need to crawl before you can walk and the first thing I did was to create an Azure IoT Hub instance, create a device identity and publish/subscribe MQTT (version 3.11 compliant) messages via [Mosquitto client tools](https://mosquitto.org/) (mosquitto_pub / mosquitto_sub) to Azure IoT Hub, all this using the [Azure CLI](https://docs.microsoft.com/cli/azure/).

![thumbnail image 2 captioned What we will do](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/316090i71346655DD321D87/image-size/large?v=v2&px=999)What we will do

The first thing I did was to leverage my favorite search engine to see if this has been done before I found this interesting guide ([https://github.com/Azure-Samples/IoTMQTTSample/tree/master/src/Mosquitto_pub](https://github.com/Azure-Samples/IoTMQTTSample/tree/master/src/Mosquitto_pub "https://github.com/azure-samples/iotmqttsample/tree/master/src/mosquitto_pub")), and whilst light on details, 2 hours later my MQTT messages started flowing in my IoT Hub, but intially I had many questions. Why use a SAS token for auth? How do I create my Azure IoT Hub? How do I create my device? But more importantly how can I do this in a way that will be part of a build pipeline?

### What is Azure IoT Hub?

[Azure IoT Hub](https://azure.microsoft.com/en-au/services/iot-hub) brings highly secure and reliable communication between your Internet of Things (IoT) application and the devices it manages. Some key features are per-device authentication, built-in device management and scaled provisioning. From an IoT perspective it provides AMQP, MQTT and HTTP endpoints (it's MQTT we will be focusing on in this post). It does a lot, but in the context of this post, think of it as your Cloud gateway for devices that you don't need to manage.

### How about AZ-CLI?

Simply put, your CLI (Command Line Interface) for Azure. It is a cross-platform CLI to connect to Azure and execute administrative commands on Azure resources. It allows the execution of commands through a terminal using interactive command-line prompts or a script. The Azure CLI, known as AZ CLI is available across Azure services and is designed to get you working quickly with Azure, with an emphasis on automation and I see it as that interim step between the console and using a direct SDK or service API. You can find more information on Azure CLI [here](https://docs.microsoft.com/cli/azure/).

You can install the Azure CLI locally on Linux, Mac, or Windows computers. It can also be used from a browser through the [Azure Cloud Shell](https://azure.microsoft.com/en-au/features/cloud-shell/) (very cool :thumbs_up::thumbs_up:) or run from inside a Docker container.

I am not going to cover setting up Azure CLI in here. See the Azure CLI [setup guide](https://docs.microsoft.com/cli/azure/install-azure-cli) for more information on setting up and configuring for your subscription but suffice to say, Azure CLI ticks that box, which will allow build pipeline integration.Enough about this, lets get started.


## Prepairing an IoT Hub

To start, you must create your own Azure IoT Hub instance.

### Create the Azure IoT Hub

While there are various SKUs to choose from, keep in mind that the *Basic tier* does not support **cloud to device** messages or **IoT Edge**. For a free test, the **F1** SKU is usually recommended, as it allows for up to 8,000 messages per day to a hub. This translates to just over 5 messages per minute or one message every 12 seconds (average). Alternatively, the **S1** SKU is available starting at €24 (March 2023) and permits up to 400,000 messages per day, equivalent to 4 messages per second. For additional SKUs, visit [here](https://azure.microsoft.com/en-au/pricing/details/iot-hub/).

```bash
az iot hub create --resource-group p-iot --name p-iot-hub --sku S1 --partition-count 2
```

As normal for most `az` commands, we can expect a lot of feedback, the following is a sample from my environment

```json
{
  "etag": "AAAADH1EDdY=",
  "id": "/subscriptions/547d54ea-411b-459e-b6f8-b3cc5e84c535/resourceGroups/p-iot/providers/Microsoft.Devices/IotHubs/p-iot-hub",
  "identity": {
    "principalId": null,
    "tenantId": null,
    "type": "None",
    "userAssignedIdentities": null
  },
  "location": "westeurope",
  "name": "p-iot-hub",
  "properties": {
    "allowedFqdnList": [],
    "authorizationPolicies": null,
    "cloudToDevice": {
      "defaultTtlAsIso8601": "1:00:00",
      "feedback": {
        "lockDurationAsIso8601": "0:00:05",
        "maxDeliveryCount": 10,
        "ttlAsIso8601": "1:00:00"
      },
      "maxDeliveryCount": 10
    },
    "comments": null,
    "deviceStreams": null,
    "disableDeviceSas": null,
    "disableLocalAuth": null,
    "disableModuleSas": null,
    "enableDataResidency": null,
    "enableFileUploadNotifications": false,
    "encryption": null,
    "eventHubEndpoints": {
      "events": {
        "endpoint": "sb://iothub-ns-p-iot-hub-24866976-29e72da33e.servicebus.windows.net/",
        "partitionCount": 2,
        "partitionIds": [
          "0",
          "1"
        ],
        "path": "p-iot-hub",
        "retentionTimeInDays": 1
      }
    },
    "features": "None",
    "hostName": "p-iot-hub.azure-devices.net",
    "ipFilterRules": [],
    "locations": [
      {
        "location": "West Europe",
        "role": "primary"
      },
      {
        "location": "North Europe",
        "role": "secondary"
      }
    ],
    "messagingEndpoints": {
      "fileNotifications": {
        "lockDurationAsIso8601": "0:00:05",
        "maxDeliveryCount": 10,
        "ttlAsIso8601": "1:00:00"
      }
    },
    "minTlsVersion": null,
    "networkRuleSets": null,
    "privateEndpointConnections": null,
    "provisioningState": "Succeeded",
    "publicNetworkAccess": null,
    "restrictOutboundNetworkAccess": null,
    "rootCertificate": null,
    "routing": {
      "endpoints": {
        "cosmosDbSqlCollections": [],
        "eventHubs": [],
        "serviceBusQueues": [],
        "serviceBusTopics": [],
        "storageContainers": []
      },
      "enrichments": null,
      "fallbackRoute": {
        "condition": "true",
        "endpointNames": [
          "events"
        ],
        "isEnabled": true,
        "name": "$fallback",
        "source": "DeviceMessages"
      },
      "routes": []
    },
    "state": "Active",
    "storageEndpoints": {
      "$default": {
        "authenticationType": null,
        "connectionString": "",
        "containerName": "",
        "identity": null,
        "sasTtlAsIso8601": "1:00:00"
      }
    }
  },
  "resourcegroup": "p-iot",
  "sku": {
    "capacity": 1,
    "name": "S1",
    "tier": "Standard"
  },
  "subscriptionid": "547d54ea-411b-459e-b6f8-b3cc5e84c535",
  "systemData": {
    "createdAt": "2023-03-23T07:50:55.436666+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": null,
    "lastModifiedBy": null,
    "lastModifiedByType": null
  },
  "tags": {},
  "type": "Microsoft.Devices/IotHubs"
}
```


### Extract the Primary Connection String

Using the `az iot hub` command, we can retrieve the connection details to our new hub as follows

```bash
az iot hub connection-string show
```

With a response similar to the following

```json
[
  {
    "connectionString": "HostName=p-iot-hub.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=0d6nvvKjBnLa71RXNl9e/ldiDaN1J4m7fk7k3+MCKPI=",
    "name": "p-iot-hub"
  }
]
```

### Create your device

In Azure IoT Hub, a device is a virtual representation of a physical or digital device that is capable of sending and receiving messages to and from the IoT Hub. Devices can be registered with an IoT Hub and can communicate using various protocols such as HTTP, MQTT, and AMQP.

To create a device called `mqtt-bridge` in Azure IoT Hub using the Azure CLI, you can use the following command:

```bash
az iot hub device-identity create --device-id mqtt-bridge --hub-name p-iot-hub
```

You now need to create your MQTT device, for the purpose of this walkthrough we will use a SAS token for authentication.

```bash
az iot hub device-identity create -n p-iot-hub -d mqtt-bridge
```

Again, we will get some feedback from this command, where we can see that the *type* for authentication is currently set to *SAS*

```json
{
  "authentication": {
    "symmetricKey": {
      "primaryKey": "A44oN4u7WDaFXZLYtL9MZbY5yp1v0HH8/TF3irFzamE=",
      "secondaryKey": "Aq9GfxUqQoij4ZTB5BgolKwmW44uDWi+2AQlUkaZISc="
    },
    "type": "sas",
    "x509Thumbprint": {
      "primaryThumbprint": null,
      "secondaryThumbprint": null
    }
  },
  "capabilities": {
    "iotEdge": false
  },
  "cloudToDeviceMessageCount": 0,
  "connectionState": "Disconnected",
  "connectionStateUpdatedTime": "0001-01-01T00:00:00",
  "deviceId": "mqtt-bridge",
  "deviceScope": null,
  "etag": "OTk1NDM3Mzc1",
  "generationId": "638151556617016781",
  "lastActivityTime": "0001-01-01T00:00:00",
  "parentScopes": [],
  "status": "enabled",
  "statusReason": null,
  "statusUpdatedTime": "0001-01-01T00:00:00"
}
```

### Generate a SAS token

There are multiple ways to provide Authentication. [SAS](https://docs.microsoft.com/azure/storage/common/storage-sas-overview) tokens and [x509](https://en.wikipedia.org/wiki/X.509) certificates are the common approaches. The SAS token is a string that you generate on the client side, and you pass this string to Azure IoT Hub for authentication. Azure IoT Hub then checks the SAS parameters and the signature to verify that it is valid. In a production environment you will likely prefer using X-509 certificates for this authentication but for the sake of my project, SAS token will work.

```bash
az iot hub generate-sas-token -d mqtt-bridge -n p-iot-hub
```

Copy and paste the SharedAccessSignature from the JSON output in your document.

```json
{
  "sas": "SharedAccessSignature sr=p-iot-hub.azure-devices.net%2Fdevices%2Fmqtt-bridge&sig=pU2iUP6Nz5KEZbPoGZ0uP3niwZiqrrRbktwqogux1Jg%3D&se=1679562876"
}
```

## Connecting to the IoT Hub

### We need TLS

Azure IoT Hub devices use SSL (Secure Sockets Layer) to encrypt and protect the devices' communications with the IoT Hub. SSL is a protocol that provides secure communication over the internet by encrypting the data that is sent between the client and the server. To establish a secure connection between the device and the IoT Hub, the device must have a copy of the certificate public key.

To create the certificate, you can use a Bash script that connects to the Azure IoT Hub and retrieves the certificate. The following Bash script can be used to create the certificate:

```
echo -n | openssl s_client -connect p-iot-hub.azure-devices.net:8883 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ./iot-hub.cert
```

This script connects to the IoT Hub over port *8883*, retrieves the certificate, and saves it as `iot-hub.cert` in the current directory. 

Once you have the certificate, you can verify that it is valid using OpenSSL. The following command can be used to verify the certificate:

```bash
openssl x509 -in Balitore.pem -text
```

This command displays the certificate information, including the certificate's expiration date, issuer, and subject. By verifying the certificate, you can ensure that the device is communicating with the correct IoT Hub and that the connection is secure.

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            06:d0:dc:75:7e:ca:79:6b:3b:1a:4c:13:8a:5d:ff:72
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = US, O = Microsoft Corporation, CN = MSFT BALT RS256 CA
        Validity
            Not Before: Feb 21 00:00:00 2023 GMT
            Not After : Jul 21 23:59:59 2023 GMT
        Subject: C = US, ST = Washington, L = Redmond, O = Microsoft Corporation, CN = *.azure-devices.net
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
```

### MQTT Client

#### Installing Moquuitto Client

We will soon be using Mosquitto_pub and Mosquitto_sub to publish and subscribe to messages. I am going to assume you have these installed on your local environment.You can easily install this on a Debian based environment with the following command.

```bash
sudo apt install mosquitto-clients
```

#### Communicating with the Device

```bash
mosquitto_sub -h p-iot-hub.azure-devices.net -p 8883 -t "devices/mqtt-bridge/messages/devicebound/#" -i mqtt-bridge -u "mytarget.azure-devices.net/Device1" -P "SharedAccessSignature sr=p-iot-hub.azure-devices.net%2Fdevices%2Fmqtt-bridge&sig=pU2iUP6Nz5KEZbPoGZ0uP3niwZiqrrRbktwqogux1Jg%3D&se=1679562876" --capath /etc/ssl/certs/ --tls-version tlsv1 -d -V mqttv311 -q 1

mosquitto_sub -h p-iot-hub.azure-devices.net -p 8883 -t "devices/mqtt-bridge/messages/devicebound/#" -i mqtt-bridge -u "mytarget.azure-devices.net/Device1" -P "SharedAccessSignature sr=p-iot-hub.azure-devices.net%2Fdevices%2Fmqtt-bridge&sig=pU2iUP6Nz5KEZbPoGZ0uP3niwZiqrrRbktwqogux1Jg%3D&se=1679562876" --cafile ./iot-hub.cert --tls-version tlsv1 -d -V mqttv311 -q 1
```


```
For Windows and MAC, see the [Mosquitto website](https://mosquitto.org/). We now have everything we need for Mosquitto but publish and subscribe have different required parameters for publishing and subscribing.  
  
What Mosquitto_pub needs:

```bash
mosquitto_pub \
-t "MQTT topic name" \
-i "pub_client" \
-u "username" \
-P "password" \
-h "host name" \
-V mqttv311 \
-p 8883 \
--cafile Baltimore.pem
-m '{"key":"value"}'
```

What Mosquitto_sub needs:

```bash
mosquitto_sub \
-t "MQTT topic name" \
-i "pub_client" \
-u "username" \
-P "password" \
-h "host name" \
-V mqttv311 \
-p 8883 \
--cafile Baltimore.pem
```

 To publish a MQTT message to Azure, you can not just any topic name. It must be in the following format:

```json
devices/{DeviceID}/messsages/events/
```

In my example Mosquitto is my DeviceID.  
  

### Step 7: Example Publishing and Subscribing

We now have all of the data and understand the required parameters to pass to both Mosquitto_pub and Mosquitto_sub. Now lets leverage these command line tools to build our completed commands and publish a message.

Substitute the values in the examples above with your data. My topic in this example is ‘devices/Mosquitto/messages/events/’. I am using ‘-d’ for verbose logging.   
  
mosquitto_sub/pub:

```
mosquitto_sub -h mytarget.azure-devices.net -p 8883 -t "devices/Device1/messages/devicebound/#" -i Device1 -u "mytarget.azure-devices.net/Device1" -P "SharedAccessSignature sr=mytarget.azure-devices.net&sig=snip&skn=snip" --capath /etc/ssl/certs/ --tls-version tlsv1 -d -V mqttv311 -q 1
```

and for publishing:

```
mosquitto_pub -h mytarget.azure-devices.net -p 8883 -t "devices/Device1/messages/events/" -i Device2 -u "mytarget.azure-devices.net/Device2" -P "SharedAccessSignature sr=bbvgathering.azure-devices.net&sig=snip&se=snip&skn=snip" --capath /etc/ssl/certs/ --tls-version tlsv1 -d -V mqttv311 -q 1 -m "{\"key\": \"value\"}"
```

**Important** You have to send JSON-Data, everything else will get rejected (at least on our setup)!


Mosquitto_pub:

```bash
mo**squitto_pub -t "devices/Mosquitto/messages/events/" -i "Mosquitto" -u "Baldacchino-IOTHub.azure-devices.net.azure-devices.net/Mosquitto/?api-version=2018-06-30" -P "SharedAccessSignature sr=Baldacchino-IOTHub.azure-devices.net%2Fdevices%2FMosquitto&sig=O5Pt61EL3n1HLzt9G2%2FYJglpCk6m4I6XsbEu4WfnRoA%3D&se=1636984597" -h "Baldacchino-IOTHub.azure-devices.net" -V mqttv311 -p 8883 --cafile Balitore.pem -m '{"key":"value"}' -d

Client Mosquitto sending CONNECT
Client Mosquitto received CONNACK (0)
Client Mosquitto sending PUBLISH (d0, q0, r0, m1, 'devices/Mosquitto/messages/events/', ... (12 bytes))
Client Mosquitto sending DISCONNECT
```

Mosquitto_sub:

```bash
mosquitto_sub -t "devices/Mosquitto/messages/events/" -i "Mosquitto" -u "Baldacchino-IOTHub.azure-devices.net.azure-devices.net/Mosquitto/?api-version=2018-06-30" -P "SharedAccessSignature sr=Baldacchino-IOTHub.azure-devices.net%2Fdevices%2FMosquitto&sig=O5Pt61EL3n1HLzt9G2%2FYJglpCk6m4I6XsbEu4WfnRoA%3D&se=1636984597" -h "Baldacchino-IOTHub.azure-devices.net" -V mqttv311 -p 8883 --cafile c:\scripts\cert.pem -d

Client Mosquitto sending CONNECT
Client Mosquitto received CONNACK (0)
Client Mosquitto sending SUBSCRIBE (Mid: 1, Topic: devices/Mosquitto/messages/events/, QoS: 0, Options: 0x00)
Client Mosquitto received SUBACK
Subscribed (mid: 1): 0
```

We can validate the message has been received via Azure IoT Hub by monitoring incoming events on the end-point:

```bash
az iot hub monitor-events --hub-name Baldacchino-IOTHubStarting event monitor, use ctrl-c to stop...{"event": {"origin": "Mosquitto","module": "","interface": "","component": "","payload": "'{key:value}'"}}
```

## Conclusion

We just walked through how you can use Mosquitto Client Tools (mosquitto_pub / mosquitto_sub) to publish and subscribe MQTT messages to Azure IoT Hub and you did it not via the [Azure Portal](https://portal.azure.com/) but via Azure CLI :flexed_biceps::flexed_biceps:. Whilst this is a baby step, it is the step we need to take before we can look at MQTT broker (the Mosquitto daemon) to Cloud (Azure IoT Hub) replication.

MQTT is the de-facto IoT protocol for devices, from Alexa through to lights and locks and devices on the factory floor. It is as close to an IoT protocol standard that we have today. Azure IoT on the other hand is your gateway in to the world of Cloud. The Azure Cloud platform contains more than 200 (and growing) products and services designed to help you bring your solutions to life, solve today’s challenges and create the future. Join me in part 2 of this multi-part blog series where I will be extending upon what we have just built and will look at broker to Cloud communication before feeding the thousands of events that occur in my house daily in to Azure IoT Hub and beyond.


# Part 2

In a [previous post](https://techcommunity.microsoft.com/t5/internet-of-things/mosquitto-client-tools-and-azure-iot-hub-the-beginning-of/ba-p/2824717), I spoke about using Mosquitto client tools to publish to [Azure IoT Hub](https://azure.microsoft.com/en-au/services/iot-hub), which was the beginning and I am on a journey in my [new role](https://www.linkedin.com/in/shanebaldacchino/) to explore Azure IoT and understand the art of the possible. What is possible, I guess we will find out. Who has the longest showers? Who wakes up first? Which devices are powered on, yet there is no activity? These questions and many can all be answered from the thousand's events occurring each day in my house, but first we need to get these events in to Azure and whilst using Mosquitto client tools (mosquitto_pub / mosquitto_sub) proved it could be done, it's not tenable for volume. Speaking of volume, 10,250 flowed through Mosquitto on a Saturday over a 24-hour period. Not an insignificant volume.  
  
The easiest path here is to not have these devices publish to Azure IoT Hub, but to have Mosquitto replicate all messages locally through to Azure IoT Hub. But is there a better way? There is. Enter Azure [IoT Edge](https://azure.microsoft.com/en-au/services/iot-edge/#iotedge-overview). Btw how cool is the logo in the logs! ASCII art is the best!

![thumbnail image 1 of blog post titled 
Replacing Mosquitto with Azure IoT Edge
](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/321138i0A7DCE27037064F5/image-size/large?v=v2&px=999)

## What is Azure IoT Edge?

Azure IoT Edge was new to myself as I embarked on this journey, and you know what, it makes a lot of sense. My take on Azure IoT Edge is, that it’s a modern bridge / broker, for the modern age. After all, all Mosquitto does is talk MQTT, which is great for my needs, but there are other protocols out there, that whilst I may not use today, I could use tomorrow with the inclusion of new devices. They say a picture describes one thousand words, to me this picture accurately describes Azure IoT Edge.

![thumbnail image 2 of blog post titled 
Replacing Mosquitto with Azure IoT Edge
](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/321100i46B04829001A7F40/image-size/large?v=v2&px=999)

But that was my take, officially this is how Microsoft describes Azure IoT Edge: 

Azure IoT Edge is a fully managed service built on Azure IoT Hub. Deploy your cloud workloads – artificial intelligence, Azure and third-party services, or your own business logic – to run on Internet of Things (IoT) edge devices via standard containers. By moving certain workloads to the edge of the network, your devices spend less time communicating with the cloud, react more quickly to local changes and operate reliably even in extended offline periods. 

The merit of this platform is hard to deny 

-   Operate offline or with intermittent connectivity
-   Modules for multiple protocols (MQTT being one of them, that said it is in [public preview](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-publish-subscribe?view=iotedge-2020-11))
-   Code using SDK’s (C, C#, Node, Python, Java)
-   Manipulate and control IoT Edge using the same tools one uses to manage Azure (AZ-CLI)
-   Zero-touch provisioning of edge devices
-   Modules run containers via the Moby engine

So many ticks for my needs and more. I want to replace Mosquitto as a local MQTT broker and I want to get these messages in to Azure IoT Hub. Azure IoT Edge will enable me to do this. 

Before we get started its worth talking about the modular approach of IoT Edge, and that becomes evident when you issue a ‘docker ps’ to list containers. IoT Edge is made up of the following three components:

1.  **IoT Edge modules** are containers that run Azure services, third-party services or custom code. They are deployed to IoT Edge-enabled devices and execute locally on those devices.
2.  The **IoT Edge runtime** runs on each IoT Edge-enabled device and manages the modules deployed to each device.
3.  The **Cloud-based interface** remotely monitors and manages IoT Edge-enabled devices.

Let’s get into this, but if the picture I painted in your head above was not clear, this diagram will help, and yes, we are going to run IoT Edge on a Raspberry Pi. This what we will build in this post.

![thumbnail image 3 of blog post titled 
Replacing Mosquitto with Azure IoT Edge
](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/321087i2B005BC4FC3B59E4/image-size/large?v=v2&px=999)

That’s right we are going to build out Azure IoT Edge using a Raspberry Pi, configure it with Azure IoT Hub. Today we will be following the Microsoft [documentation](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-symmetric?view=iotedge-2020-11&tabs=azure-portal) (3 articles in total) in order to install Azure IoT Edge but along the way I will provide you some insights to guide you through this process

## High Level Steps

As we walk through this guide, there is quite a lot to do to make this ‘magic’ happen. At a high level they are. 

1.  Install Requirements
2.  Install The Container Engine
3.  Install Azure IoT Edge
4.  Create Edge IoT Device & Extract Connection String
5.  Configure And Start Azure IoT Edge
6.  Validate Azure IoT Edge Is Running
7.  Enable MQTT
8.  Define Local MQTT Device
9.  Test Publishing

## Variables You Will Need To Substitute

This post serves as a high level walkthrough for establishing communication between your local MQTT devices and Azure IoT Edge and through to Azure IoT Hub. You will be presented with outputs from CLI from your Azure subscription, you will need to substitute these values. Some key values you need to substitute.

```json
Resource-group
IOT-Hub name
Device name
Child device name
IP address of edge hub
SAS token
Connection string
```

## Public Preview Warning

It must be noted that at the time of authoring this post (November 2021) this is not a supported architecture, but it will be, and as a such we need to make a few tweaks to our configuration to make this work. This process should only get easier.

-   Support for ARM64 (Raspberry Pi) is in public preview
-   Support for MQTT is in public preview

Speaking of making this easier. There is an [IoT Edge configuration tool](https://github.com/Azure/iot-edge-config) to help make our lives easier and configuring IoT Edge. How-ever this tool only supports Tier 1 operating systems and the use of a Raspberry Pi with ARM64 is not supported. Hopefully soon, but today we are pioneering and getting our hands dirty, which isn’t a bad thing as we are really learning how IoT Edge works.

## Step 1: Install Requirements

Under Raspbian a ‘sudo apt-get’ install will not be able to install IoT Edge because it lacks details to the Microsoft repository. This can be remedied with a few simple commands.

```bash
curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
```

We will then copy the generated list to the sources.list.d directory.

```bash
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
```

and install the Microsoft GPG public key.

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

We will now install the container engine Azure IoT Edge relies upon.

```bash
sudo apt-get update
```

If we look at the output of the ‘apt-get update’ we can see the Microsoft repositories are now listed.

```bash
pi@raspberrypi:~ $ sudo apt-get update
Get:1 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch InRelease [29.8 kB]
Get:2 http://raspbian.raspberrypi.org/raspbian buster InRelease [15.0 kB]      
Get:3 http://archive.raspberrypi.org/debian buster InRelease [32.6 kB]                              
Get:4 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main arm64 Packages [3,904 B]        
Get:5 http://phoscon.de/apt/deconz buster InRelease [2,857 B]       
Get:6 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main all Packages [546 B]
Get:7 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main armhf Packages [14.7 kB]
Get:8 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main amd64 Packages [11.9 kB]
Get:9 http://raspbian.raspberrypi.org/raspbian buster/main armhf Packages [13.0 MB]
Get:10 http://archive.raspberrypi.org/debian buster/main armhf Packages [393 kB]
Get:11 http://phoscon.de/apt/deconz buster/main armhf Packages [716 B]
Get:12 http://raspbian.raspberrypi.org/raspbian buster/contrib armhf Packages [58.8 kB]                                                                                                             
Get:13 http://raspbian.raspberrypi.org/raspbian buster/non-free armhf Packages [104 kB]                                                                                                              
Fetched 13.7 MB in 11s (1,204 kB/s)                                                                                                                                                                  
Reading package lists... Done
N: Repository 'http://raspbian.raspberrypi.org/raspbian buster InRelease' changed its 'Suite' value from 'stable' to 'oldstable'
```

## Step 2: Install The Container Engine

Raspbian now knows where to look for packages so we can get started installing the container engine (Moby) to which Azure IoT Edge relies upon. Azure IoT Edge relies on an OCI-compatible container runtime. The Moby engine is the only container engine officially supported with Azure IoT Edge.I am installing the latest version of Azure IoT Edge, if you need to install older versions please see [Microsoft’s documentation](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-symmetric?view=iotedge-2020-11&tabs=azure-portal#offline-or-specific-version-installation-optional).

```bash
sudo sudo apt-get install moby-engine
```

This will provide output like that below

```bash
Reading package lists... Done
Building dependency tree      
Reading state information... Done
The following package was automatically installed and is no longer required:
  python-colorzero
Use 'sudo apt autoremove' to remove it.
The following additional packages will be installed:
  apparmor moby-cli
Suggested packages:
  apparmor-profiles-extra apparmor-utils cgroupfs-mount | cgroup-lite
The following NEW packages will be installed:
  apparmor moby-cli moby-engine
0 upgraded, 3 newly installed, 0 to remove and 87 not upgraded.
Need to get 51.9 MB of archives.
After this operation, 198 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main armhf moby-cli armhf 3.0.13+azure-0 [18.5 MB]
Get:2 http://mirror.internode.on.net/pub/raspbian/raspbian buster/main armhf apparmor armhf 2.13.2-10 [438 kB]
Get:3 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main armhf moby-engine armhf 3.0.13+azure-0 [33.0 MB]
Fetched 51.9 MB in 6s(8,476kB/s)                                                                                                                                                                   
Preconfiguring packages ...
Selecting previously unselected package apparmor.
(Reading database ... 105747 files and directories currently installed.)
Preparing to unpack .../apparmor_2.13.2-10_armhf.deb ...
Unpacking apparmor (2.13.2-10) ...
Selecting previously unselected package moby-cli.
Preparing to unpack .../moby-cli_3.0.13+azure-0_armhf.deb ...
Unpacking moby-cli (3.0.13+azure-0) ...
Selecting previously unselected package moby-engine.
Preparing to unpack .../moby-engine_3.0.13+azure-0_armhf.deb ...
Unpacking moby-engine (3.0.13+azure-0) ...
Setting up apparmor (2.13.2-10) ...
Created symlink /etc/systemd/system/sysinit.target.wants/apparmor.service → /lib/systemd/system/apparmor.service
Setting up moby-engine (3.0.13+azure-0) ...
Adding group `docker' (GID 124) ...
Done.
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service →/lib/systemd/system/docker.service.
Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
Setting up moby-cli (3.0.13+azure-0) ...
Processing triggers for man-db (2.8.5-2) ...
Processing triggers for systemd (241-7~deb10u7+rpi1) ...
```

## Step 3: Install Azure IoT Edge

After the installation of the container engine, we are now set to install Azure IoT Edge

```bash
sudo apt-get install aziot-edge
```

This will provide output like that below.

```bash
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following package was automatically installed and is no longer required:
  python-colorzero
Use 'sudo apt autoremove' to remove it.
The following additional packages will be installed:
  aziot-identity-service
The following NEW packages will be installed:
  aziot-edge aziot-identity-service
0 upgraded, 2 newly installed, 0 to remove and 87 not upgraded.
Need to get 7,937 kB of archives.
After this operation, 35.9 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main armhf aziot-identity-service armhf 1.2.3-1 [3,063 kB]
Get:2 https://packages.microsoft.com/debian/stretch/multiarch/prod stretch/main armhf aziot-edge armhf 1.2.4-1 [4,874 kB]
Fetched 7,937 kB in 2s (4,133 kB/s)
Selecting previously unselected package aziot-identity-service.
(Reading database ... 105939 files and directories currently installed.)
Preparing to unpack .../aziot-identity-service_1.2.3-1_armhf.deb ...
Unpacking aziot-identity-service (1.2.3-1) ...
Selecting previously unselected package aziot-edge.
Preparing to unpack .../aziot-edge_1.2.4-1_armhf.deb ...
Unpacking aziot-edge (1.2.4-1) ...
Setting up aziot-identity-service (1.2.3-1) ...
Created symlink /etc/systemd/system/sockets.target.wants/aziot-certd.socket → /lib/systemd/system/aziot-certd.socket.
Created symlink /etc/systemd/system/sockets.target.wants/aziot-identityd.socket → /lib/systemd/system/aziot-identityd.socket.
Created symlink /etc/systemd/system/sockets.target.wants/aziot-keyd.socket → /lib/systemd/system/aziot-keyd.socket.
Created symlink /etc/systemd/system/sockets.target.wants/aziot-tpmd.socket → /lib/systemd/system/aziot-tpmd.socket.
Setting up aziot-edge (1.2.4-1) ...
===============================================================================

                              Azure IoT Edge

  IMPORTANT: Please configure the device with provisioning information.

  Please create a new system configuration.

  You can quickly configure the device for manual provisioning with a connection string using:

    sudo iotedge config mp --connection-string '...'

  For other provisioning options, copy the template file
  at /etc/aziot/config.toml.edge.template to /etc/aziot/config.toml,
  update it with your device information, then apply the configuration to
  the IoT Edge services with:

    sudo iotedge config apply

===============================================================================
Created symlink /etc/systemd/system/sockets.target.wants/aziot-edged.mgmt.socket → /lib/systemd/system/aziot-edged.mgmt.socket.
Created symlink /etc/systemd/system/multi-user.target.wants/aziot-edged.service → /lib/systemd/system/aziot-edged.service.
Created symlink /etc/systemd/system/sockets.target.wants/aziot-edged.workload.socket → /lib/systemd/system/aziot-edged.workload.socket.
Processing triggers for man-db (2.8.5-2) ...
```

## Step 4: Create Edge IoT Device & Extract Connection String

We are going to pivot away from our SSH terminal here to Azure CLI (you could use the Azure portal) as we need to create an Edge IoT Device in Azure IoT Hub and create a SAS Policy for authentication.

```bash
 az iot hub device-identity create --device-id Pi-IOTEdge --hub-name Baldacchino-IOTHub --edge-enabled
```

This will provide output like that below (keys redacted).

```json
{
  "authentication": {
    "symmetricKey": {
      "primaryKey": "*******",
      "secondaryKey": "*******"
    },
    "type": "sas",
    "x509Thumbprint": {
      "primaryThumbprint": null,
      "secondaryThumbprint": null
    }
  },
  "capabilities": {
    "iotEdge": true
  },
  "cloudToDeviceMessageCount": 0,
  "connectionState": "Disconnected",
  "connectionStateUpdatedTime": "0001-01-01T00:00:00",
  "deviceId": "Pi-IOTEdge",
  "deviceScope": "ms-azure-iot-edge://PiIotEdge-637698647485926814",
  "etag": "MTIwNTI0NjYy",
  "generationId": "637698647485926814",
  "lastActivityTime": "0001-01-01T00:00:00",
  "parentScopes": [],
  "status": "enabled",
  "statusReason": null,
  "statusUpdatedTime": "0001-01-01T00:00:00"
}
```

We now need to extract the device connection string which will then be passed into the Azure IoT Edge config.toml file.

```bash
az iot hub device-identity connection-string show --device-id Pi-IOTEdge  --hub-name Baldacchino-IOTHub
```

This will provide output like that below (keys redacted). Copy and paste this into your favourite IDE as we will soon need this connection string.

```json
{
  "connectionString": "HostName=Baldacchino-IOTHub.azure-devices.net;DeviceId=Pi-IOTEdge;SharedAccessKey=**********"
}

```

## Step 5: Configure And Start Azure IoT Edge

So we have just installed Azure IoT Edge Hub but at this stage it is sitting dormant waiting for configuration. Specifically it needs its cloud identity and authentication information. Use the connection string value from the prior step. I am using the –force value as if you do make a mistake with your connection string the –force parameter will allow you to reset this. SAS Key has been redacted.

```bash
sudo iotedge config mp --connection-string 'HostName=Baldacchino-IOTHub.azure-devices.net;DeviceId=Pi-IOTEdge;SharedAccessKey=*************' --force
```

This will provide output similar to that below.

```bash
Azure IoT Edge has been configured successfully!
The configuration has been written to /etc/aziot/config.toml
To apply the new configuration to services, run:

    sudo iotedge config apply -c '/etc/aziot/config.toml'

WARNING: This configuration is not suitable when using IoT Edge as a gateway.
```

We now are ready to start Azure Iot Edge and apply the configuration in the toml file.

```bash
sudo iotedge config apply -c '/etc/aziot/config.toml'
```

This will provide output like that below.

```bash
Note: Symmetric key will be written to /var/secrets/aziot/keyd/device-id
Azure IoT Edge has been configured successfully!

Restarting service for configuration to take effect…
Stopping aziot-edged.service…Stopped!
Stopping aziot-identityd.service…Stopped!
Stopping aziot-keyd.service…Stopped!
Stopping aziot-certd.service…Stopped!
Stopping aziot-tpmd.service…Stopped!
Starting aziot-edged.mgmt.socket…Started!
Starting aziot-edged.workload.socket…Started!
Starting aziot-identityd.socket…Started!
Starting aziot-keyd.socket…Started!
Starting aziot-certd.socket…Started!
Starting aziot-tpmd.socket…Started!
Starting aziot-edged.service…Started!
Done.
```

## Step 6: Validate Azure IOT Edge Is Running

So, I had a lot of issues with IoT Edge initially. What the documentation doesn’t easily tell you is how to view the logs. Remember IoT Edge is modular. These two commands will help you view the status of both IoT EdgeHub and IoT EdgeAgent.These two commands are case sensitive, and I suggest you run these and frequently.

```bash
iotedge logs edgeHub
iotedge logs edgeAgent
```

If you are seeing stack traces or errors here, there are problems. During this journey I received errors due to a process being bound to TCP 443 (as seen below) and incorrectly trying to enable MQTT. The logs are your friends.

```bash
<3> 2021-10-19 21:53:58.311 +00:00 [ERR] - Edge agent plan execution failed.
System.AggregateException: One or more errors occurred. (Error calling start module edgeHub: Could not start module edgeHub
        caused by: Could not start module edgeHub
        caused by: driver failed programming external connectivity on endpoint edgeHub (63137b88b3877ab069523c3ccd2c5f2dc1f031b3232e0d6f6c89da66bf349c78): Error starting userland proxy: listen tcp 0.0.0.0:443: bind: address already in use)
```

## Step 7: Enable MQTT

Since IoT Edge MQTT broker is currently in public preview, you need to also set the following environment variables to true on the edgeHub module to enable the MQTT broker. We have been using AZ-CLI during our journey, thats great because there is no way to enable MQTT in the Azure Portal.

```json
experimentalFeatures__enabled = true
experimentalFeatures__mqttBrokerEnabled = true
```

We need to deploy a JSON manifest file and this file can be found in the Microsoft Docs website in the following article –[Communicate with your IoT hub using the MQTT](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-publish-subscribe?view=iotedge-2020-11#appendix---sample-deployment-manifest). Below is the JSON you need. Copy and paste this and save it as a JSON file to which we will pass into AZ CLI. You will see both experimental features are set to true and that TCP 1833 and 8883 (TLS) are being bound.

```json
{
   "modulesContent":{
      "$edgeAgent":{
         "properties.desired":{
            "schemaVersion":"1.1",
            "runtime":{
               "type":"docker",
               "settings":{
                  "minDockerVersion":"v1.25",
                  "loggingOptions":"",
                  "registryCredentials":{
                    
                  }
               }
            },
            "systemModules":{
               "edgeAgent":{
                  "type":"docker",
                  "settings":{
                     "image":"mcr.microsoft.com/azureiotedge-agent:1.2",
                     "createOptions":"{}"
                  }
               },
               "edgeHub":{
                  "type":"docker",
                  "status":"running",
                  "restartPolicy":"always",
                  "settings":{
                     "image":"mcr.microsoft.com/azureiotedge-hub:1.2",
                     "createOptions":"{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":{\"HostPort\":\"443\"}],\"1883/tcp\":[{\"HostPort\":\"1883\"}]}}}"
                  },
                  "env":{
                     "experimentalFeatures__mqttBrokerEnabled":{
                        "value":"true"
                     },
                     "experimentalFeatures__enabled":{
                        "value":"true"
                     },
                     "RuntimeLogLevel":{
                        "value":"debug"
                     }
                  }
               }
            },
            "modules":{
              
            }
         }
      },
      "$edgeHub":{
         "properties.desired":{
            "schemaVersion":"1.2",
            "routes":{
               "Upstream":"FROM /messages/* INTO $upstream"
            },
            "storeAndForwardConfiguration":{
               "timeToLiveSecs":7200
            },
            "mqttBroker":{
               "authorizations":[
                  {
                     "identities":[
                        "{{iot:identity}}"
                     ],
                     "allow":[
                        {
                           "operations":[
                              "mqtt:connect"
                           ]
                        }
                     ]
                  },
                  {
                     "identities":[
                        "{{iot:identity}}"
                     ],
                     "allow":[
                        {
                           "operations":[
                              "mqtt:publish",
                              "mqtt:subscribe"
                           ],
                           "resources":[
                              "test_topic"
                           ]
                        }
                     ]
                  }
               ]
            }
         }
      }
   }
}
```

Using AZ-CLI we need to deploy this deployment JSON to your IoT Edge and we can do this by running the following command. In the code below, I saved this json as mqttdeploy.json

```bash
az iot edge set-modules --device-id Pi-IOTEdge --hub-name Baldacchino-IOTHub --content .\mqttdeploy.json
```

After you run this, IoT Hub will push a configuration change to your IoT Edge and you can validate this in the logs.

```bash
<6> 2021-10-19 21:56:28.231 +00:00 [INF] - Obtained Edge agent twin from IoTHub with desired properties version 7 and reported properties version 14500.
<6> 2021-10-19 21:56:29.066 +00:00 [INF] - Plan execution started for deployment 7
<6> 2021-10-19 21:56:29.074 +00:00 [INF] - Executing command: "Command Group: (\n  [Stop module edgeHub]\n  [Start module edgeHub]\n  [Saving edgeHub to store]\n)"
<6> 2021-10-19 21:56:29.087 +00:00 [INF] - Executing command: "Stop module edgeHub"
<6> 2021-10-19 21:56:29.145 +00:00 [INF] - Executing command: "Start module edgeHub"
<6> 2021-10-19 21:56:30.568 +00:00 [INF] - Executing command: "Saving edgeHub to store"
<6> 2021-10-19 21:56:30.616 +00:00 [INF] - Plan execution ended for deployment 7
```

But more specifically what the logs will confirm, is that MQTT is enabled and a telnet to 1833 picks up. Woot!

```bash
<6> 2021-10-20 23:58:33.873 +00:00 [INF] [EdgeHub] - Experimental features configuration: {"Enabled":true,"DisableCloudSubscriptions":false,"DisableConnectivityCheck":false,"EnableMqttBroker":true}
```

## Step 8: Define Local MQTT Device

We are almost there. Right now, if you try and send messages to the IoT Edge via MQTT, it will pick up but you will receive an authorization error. We need to define each MQTT device within Azure IoT and also set the parent to child relationship. We can do this via AZ-CLI. In my example I am using the local device name of ‘IotEdge-DeviceChild’.

```bash
az iot hub device-identity children add -d Pi-IOTEdge --child-list IotEdge-DeviceChild-n Baldacchino-IOTHub
```

## Step 9: Test Publishing

We are now done and are ready for testing and will use mosquitto_pub to simulate a device. Mosquitto_pub .To publish a MQTT message to Azure, you can not just any topic name. It must be in the following format.

```json
devices/{DeviceID}/messsages/events/
```

In my example

-   IotEdge-DeviceChild is my DeviceID
-   I have obtained the SAS signature for the device
-   My EdgeHub IP address is 10.0.0.150 (remember we are publishing locally)

```bash
mosquitto_pub -t "devices/IotEdge-DeviceChild/messages/events/" -i "IotEdge-DeviceChild" -u "Baldacchino-IOTHub.azure-devices.net/IotEdge-DeviceChild/?api-version=2018-06-30" -P "SharedAccessSignature sr=Baldacchino-IOTHub.azure-devices.net%2Fdevices%2FIotEdge-DeviceChild&sig=5oZbwgye99edq2%2Fc%2BWERB%2BeQukic9cTBuSDi%2BAYgd54%3D&se=1642568789" -h 10.0.0.150 -V mqttv311 -m '{"Message":"Hello World"}' -d

Client IotEdge-DeviceChild sending CONNECT
Client IotEdge-DeviceChild received CONNACK (0)
Client IotEdge-DeviceChild sending PUBLISH (d0, q0, r0, m1, 'devices/IotEdge-DeviceChild/messages/events/', ... (24 bytes))
Client IotEdge-DeviceChild sending DISCONNECT
```

We can validate the message has been received via Azure IoT Hub by monitoring incoming events on the end-point

```bash
az iot hub monitor-events --hub-name Baldacchino-IOTHub
Starting event monitor, use ctrl-c to stop...
{
"event": {
"origin": "IotEdge-DeviceChild",
"module": "",
"interface": "",
"component": "",
"payload": "'{Message:Hello World}'"
}
}
```

## Conclusion

Thanks for sticking this one out. If you made it to here, well done as this was a long post!! :flexed_biceps::flexed_biceps:We covered a lot of ground and hopefully this guide was helpful. We learnt about Azure IoT Edge, a fully managed service built on Azure IoT Hub. It does a heap, one of those things it does is allows you to publish MQTT messages locally to it and those messages are replicated in to Azure IoT Hub. Today, MQTT and ARM64 is experimental so keep it for testing. Is this the only way we replicate our messages to Azure IoT Hub? Stay tuned as in part 3 I look at how we can leverage Azure SDK’s and the Paho-MQTT library as I build my own bridge before settling on a mechanism to replicate my houses telemetry in to Azure.  
  
Think-big and happy automating.

# part 3

![thumbnail image 1 captioned Paho MQTT with the Azure IoT Device SDK = Awesome!](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/332428i464343280630F8D0/image-size/medium?v=v2&px=400)Paho MQTT with the Azure IoT Device SDK = Awesome!

I have been on a journey to explore Azure IoT and push the thousands of events that flow through my local MQTT broker (Mosquitto) in to Azure IoT Hub. From [direct connection to Azure IoT Hub](https://techcommunity.microsoft.com/t5/internet-of-things/mosquitto-client-tools-and-azure-iot-hub-the-beginning-of/ba-p/2824717 "Mosquitto Client Tools and Azure IoT Hub. The beginning of something good....") (via MQTT and SAS tokens) through to [Azure IoT Edge running locally with MQTT](https://techcommunity.microsoft.com/t5/internet-of-things/replacing-mosquitto-with-azure-iot-edge/ba-p/2898126 "Replacing Mosquitto with Azure IoT Edge"). I have been able to achieve my goals with varying levels of success, but have a few concerns on the approaches I have tried thus far.

-   Direct-Connection to Azure IoT Hub introduces latency to the cloud.
-   Authentication, from SAS tokens to X509 certificates, its not anonymous and some of my tiny devices (Tasmota) dont bode well.
-   Topic structure, it is defined (_devices/{DeviceID}/messsages/events/_) and not free form. It means reconfiguration, which isn’t hard, but a lot of friction.

Channeling my inner Steve Ballmer (developer, developer, developer), it’s time to build. My goals for building a solution

1.  No reconfiguration of any of my MQTT devices (Home Assistant, PLC, Arduino Mega 2560, ~75 Tasmota devices).
2.  Bridge my existing MQTT broker (Mosquitto) in to Azure IoT Hub.
3.  Must run on aarch64 architecture, a Raspberry Pi.

Pretty lofty goals, you may even say I am being lazy, but the reality is I want a low friction away to derive operational intelligence from the many thousands of events each day (read below, its over 10K per day!)And for that we need to get our hands dirty, write some code and use SDK’s.

## What we are going to build

**![thumbnail image 2 captioned Using Paho + Azure IOT SDK we will listen to Mosquitto](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/329063iA401F924EACCD51E/image-size/large?v=v2&px=999)Using Paho + Azure IOT SDK we will listen to Mosquitto**

To overcome, the limitations described above we are going to build an application in Python using the Python MQTT library Paho and the Azure Python IoT SDK. Lets quickly talk about both of these.

## Paho MQTT

[Paho](https://www.eclipse.org/paho/index.php?page=clients/python/index.php) is a Python client class which enable applications to connect to an [MQTT](http://mqtt.org/) broker to publish messages, to subscribe to topics and receive published messages. It also provides some helper functions to make publishing one off messages to an MQTT server very straightforward. This is what we will be using to listen to messages on our [Mosquitto](https://mosquitto.org/) broker. For examples and more you can more information about the Paho MQTT module on the [pypi.org](https://pypi.org/project/paho-mqtt/) website.To install Paho, you can use PIP.

```bash
pip install paho-mqtt
```

and can leverage Paho with

```python
import paho.mqtt.client as mqtt
```

## Azure IoT SDK for Python

Once the messages have been read by Paho from Mosquitto we need to get these in to Azure IoT. The [Azure IoT SDKs for Python](https://github.com/Azure/azure-iot-sdk-python) enables us to do away with MQTT and speak directly to the service in Python. The SDK takes care of ‘Authentication’, ‘Send device-to-cloud message’, ‘Receive cloud-to-device messages’, ‘Device twins’, ‘Direct methods’, ‘Connection status and error reporting’, ‘Retry policies’ and ‘Upload file to blob’.A lot of the heavy lifting I need is being performed by this SDK for us. To install Azure IoT SDK for Python you can use PIP. For code examples and more you can find more information about this device module on the [pypi.org](https://pypi.org/project/azure-iot-device/) website.

```bash
pip install azure-iot-device
```

and can leverage Azure IoT SDK for Python

```python
from azure.iot.device import IoTHubDeviceClien
```

Lets write some code.

## Code Summary

See the steps below as I tease out this solution or my [GitHub repo](https://github.com/baldacchino/pahomqtt_to_azureiot) for the full Python script. To give you a better understanding on how this works I will break it down in to the logical steps below required to receive messages from Mosquitto over MQTT using Paho and to then re-publish them in to Azure IoT Hub using the Azure IoT SDK for Python.

### Step 1 – Import Modules

We need to use modules, mainly Paho and Azure IoT to provide additional functionality

```python
import paho.mqtt.client as mqtt
import os
import asyncio
import uuid
from azure.iot.device.aio import IoTHubDeviceClient
from azure.iot.device import Message
from datetime import datetime
```

### Step 2 – Connect To Mosquitto & Subscribe To MQTT Topics

After declaring our modules we need to connect to our MQTT broker, we will do this with a function (on_connect)

```python
client = mqtt.Client(MQTTClientName)  
client.on_connect = on_connect 

def on_connect(client, userdata, flags, rc):  # The callback for when the client connects to the broker
    print(str(datetime.now()) + " | Connecting to MQTT Broker : " + MQTTBrokerIP)
    print(str(datetime.now()) + " | Connected with result code {0}".format(str(rc))) 
    print(str(datetime.now()) + " | We are connected!")
    print()
    print(str(datetime.now()) + " | Subscribing to MQTT Topics")
    print(str(datetime.now()) + " | Subscribing to " + MQTTTopicSubscribe)
    client.subscribe(MQTTTopicSubscribe)
    print()
```

After connecting, we need to tell our MQTT broker what topics we want to subscribe to. This way we can be more precise on what we want to replicate in to Azure. We can use MQTT topic filters to do this. Initially I started with a # but decided to use a single level wildcard +.

```python
MQTTTopicSubscribe = "stat/+/POWER" #MQTT Topic Filter
```

> Plus sign (+): It is a single level wildcard that matches any name for a specific topic level. We can use this wildcard instead of specifying a name for any topic level in the topic filter.Hash (#): It is a multi level wildcard that we can use only at the end of the topic filter, as the last level and matches any topic whose first levels are the same as the topic levels specified at the left-hand side of the # symbol.
> 
> [Understanding wildcards | MQTT Essentials – A Lightweight IoT Protocol (packtpub.com)](https://subscription.packtpub.com/book/application_development/9781787287815/1/ch01lvl1sec18/understanding-wildcards)

### Step 3 – Listen For Messages

We have now subscribed to MQTT topics and we need to listen and act on these incoming messages. I am taking the MQTT Topic and MQTT Payload, and passing these in to the my python Azure function (not an Azure Function ;)) which will push the payload in to Azure.

```python
client.on_message = on_message  

def on_message(client, userdata, msg):  # The callback for when a PUBLISH message is received from the server.
    global mqtt_topic
    mqtt_topic = msg.topic
    global mqtt_payload
    mqtt_payload = str(msg.payload)
    print(str(datetime.now()) + " | Message received")
    print(str(datetime.now()) + " | MQTT Topic and Payload: " + msg.topic + " " + str(msg.payload)[2:][:-1])  # Print a received msg
    asyncio.run(azure())
```

### Step 4 – Send Messages To Azure

With the MQTT Topic and Payload we can now push these messages in to Azure. I am sending these as a JSON object. I have had to massage the MQTT payload as my PLC is adding a few extra values I dont need.

```python
async def azure():
    # Create instance of the device client using the connection string
    device_client = IoTHubDeviceClient.create_from_connection_string(AzureIOTHub_conn_str)
     
    # Connect the device client.
    await device_client.connect()
    print(str(datetime.now()) + " | Async connection established to Azure IOT")

    # Send a single message
    print(str(datetime.now()) + " | Sending message to Azure IOT Hub")
    msg = Message("{ \"DateTime\": \"" + str(datetime.now()) + "\", \"MQTT Topic\": \"" + mqtt_topic + "\", \"Payload\": \"" + mqtt_payload[2:][:-1] + "\" }")
    msg.message_id = uuid.uuid4()
    msg.content_encoding = "utf-8"
    msg.content_type = "application/json"

    
    await device_client.send_message(msg)
    print(str(datetime.now()) + " | Message sent, tearing down Azure IOT Hub connection")
    print()

    # Finally, shut down the client
    await device_client.shutdown()
```

## Pulling It All Together

Here is a complete copy of the above, plus a bit more. Assuming you have installed Paho and Azure IoT installed via PIP. You could cut and paste the below or clone my [GitHub repository.](https://github.com/baldacchino/pahomqtt_to_azureiot)

```python
import paho.mqtt.client as mqtt
import os
import asyncio
import uuid
from azure.iot.device.aio import IoTHubDeviceClient
from azure.iot.device import Message
from datetime import datetime

# -----------------------------------------------------------------------
# EDIT BELOW THIS LINE

ScriptVersion = "1.0"
ModifiedDate = "Monday 15, November 2021"
MQTTBrokerIP = "10.0.0.200" #IP Address of your MQTT Broker
MQTTTopicSubscribe = "stat/+/POWER" #MQTT Topic Filter
MQTTClientName = "RaspiPI4" #Used to identify the device to your MQTT Broker
AzureIOTHub_conn_str = "********************************************************" #Azure IOT Hub Connection String

# EDIT ABOVE THIS LINE
# -----------------------------------------------------------------------

def on_connect(client, userdata, flags, rc):  # The callback for when the client connects to the broker
    print(str(datetime.now()) + " | Connecting to MQTT Broker : " + MQTTBrokerIP)
    print(str(datetime.now()) + " | Connected with result code {0}".format(str(rc))) 
    print(str(datetime.now()) + " | We are connected!")
    print()
    print(str(datetime.now()) + " | Subscribing to MQTT Topics")
    print(str(datetime.now()) + " | Subscribing to " + MQTTTopicSubscribe)
    client.subscribe(MQTTTopicSubscribe)
    print()

def on_message(client, userdata, msg):  # The callback for when a PUBLISH message is received from the server.
    global mqtt_topic
    mqtt_topic = msg.topic
    global mqtt_payload
    mqtt_payload = str(msg.payload)
    print(str(datetime.now()) + " | Message received")
    print(str(datetime.now()) + " | MQTT Topic and Payload: " + msg.topic + " " + str(msg.payload)[2:][:-1])  # Print a received msg
    asyncio.run(azure())
    

async def azure():
    # Create instance of the device client using the connection string
    device_client = IoTHubDeviceClient.create_from_connection_string(AzureIOTHub_conn_str)
     
    # Connect the device client.
    await device_client.connect()
    print(str(datetime.now()) + " | Async connection established to Azure IOT")

    # Send a single message
    print(str(datetime.now()) + " | Sending message to Azure IOT Hub")
    msg = Message("{ \"DateTime\": \"" + str(datetime.now()) + "\", \"MQTT Topic\": \"" + mqtt_topic + "\", \"Payload\": \"" + mqtt_payload[2:][:-1] + "\" }")
    msg.message_id = uuid.uuid4()
    msg.content_encoding = "utf-8"
    msg.content_type = "application/json"

    
    await device_client.send_message(msg)
    print(str(datetime.now()) + " | Message sent, tearing down Azure IOT Hub connection")
    print()

    # Finally, shut down the client
    await device_client.shutdown()

print("*********************************************************************")
print("*                                                                   *")
print("*                                                                   *")
print("*               MQTT --> Azure IOT Bridge                           *")
print("*                                                                   *")
print("*                                                                   *")
print("*                                                                   *")
print("* shane@baldacchino.net                                             *")
print(f"* Version : {ScriptVersion}                                                     *")
print(f"* Modified Date : {ModifiedDate}                          *")
print("*                                                                   *")
print("*********************************************************************")


client = mqtt.Client(MQTTClientName)  
client.on_connect = on_connect 
print(str(datetime.now()) + " | Listening for messages")
print()
client.on_message = on_message  
client.connect(MQTTBrokerIP, 1883, 60)  # Connect to (broker, port, keepalive-time)
client.loop_forever()  # Start networking daemon
try:
    asyncio.run(azure())
except:
    pass #continue on errors - used to solve internet connectivity issues.
```

## Seeing This In Action

Lets drop to a video to see this in working end-to-end, to validate messages are flowing in to Azure IoT Hub I can use the Azure CLI (AZ-CLI) to monitor the output.

```powershell
az iot hub monitor-events --output table --device-id devicename --hub-name hubname --output json
```

For the purpose of this demo, I have left a handful of messages at QoS level 2 and set LWT (Last Will and Testament) to true.

After 24 hours of running, we can see I have published 10.52K of messages in to Azure IoT Hub and there are certain ebbs and flows that occur in my house. You can even tell my kids have screen time late in the afternoon as the number of messages drop considerably. There is temperature, humidity and other signals but movement (captured by passive infrared sensors) almost vanishes from the event stream.

![thumbnail image 3 captioned Can you see where my children have screen time?](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/329070iB5A16F2A839FB2F5/image-size/large?v=v2&px=999)Can you see where my children have screen time?

## Conclusion

There are many ways to skin this code cat. My requirements was to publish messages in to Azure and we have been able to achieve this via different ways (I am sure there is more). Automation is a journey, which path will you take?

We illustrated a transparent side-car approach that will listen to an existing broker, on topics you desire and push these in to Azure IoT, all without making any configuration changes (the most important thing for my implementation).

Are there any draw backs? Sure there are. Right now this is one way in direction (simplex) and allows me to push messages in to Azure IoT Hub but not receive messages back. All of my devices locally are represented as a single Azure IoT device which is less than ideal.

Azure IoT Edge or direct MQTT publishing to Azure IoT Hub would be duplex communication resulting in each local device being represented as a unique device in Azure IoT Hub.

The Azure IoT SDK for Python is capable of duplex communication (Receive cloud-to-device message) but I have yet to implement it. Will I? I am unsure but its nice to know I can. Today I am using this approach to pass in telemetry and am able to distinguish my devices based on the MQTT topic structure. My data will be in a future post massaged within Azure removing the constraints of MQTT. There is many ways to do this, from a pure code approach (I mentioned above the Azure IoT SDK for Python has a lot more smarts) though to [Device Twins](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins) and [Module Twins.](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-module-twins) Personally, I like the SDK approach, it's my code, my choices on what I do, but I do understand this is not for everyone. We now have my messages, my events, in Azure and now its time to make some friends and learn how to derive operational intelligence from visualisations through to machine learning and beyond.

Think big and happy building!

# part 4

I have been on a journey to explore Azure IoT and push the thousands of events that flow through my local MQTT broker (Mosquitto) into Azure IoT.

After my last post in using the [Azure IoT SDK for Python](https://techcommunity.microsoft.com/t5/internet-of-things-blog/bridge-building-mosquitto-mqtt-to-azure-iot-using-paho-mqtt-and/ba-p/2999643) conjunction with Paho MQTT I thought my work here was complete. But I have just recently been made aware that there is [native support](https://techcommunity.microsoft.com/t5/internet-of-things-blog/arduino-library-for-azure-iot/ba-p/3034455) for various Arduino devices by Microsoft and Espressif. How awesome is that!

Before you get too excited given the requirements of such libraries, this is not going to work on your Arduino Uno, Arduino Mega 2560 and so on. Support for Azure IoT Hub is (for now) reserved for the newer generation of boards from Espressif ([ESP32](https://github.com/Azure/azure-sdk-for-c-arduino/blob/main/examples/Azure_IoT_Hub_ESP32/readme.md), [ESP8266](https://github.com/Azure/azure-sdk-for-c-arduino/blob/main/examples/Azure_IoT_Hub_ESP8266/readme.md)) and the Realtek [Ameba D](https://github.com/Azure/azure-sdk-for-c-arduino/blob/main/examples/Azure_IoT_Hub_RealtekAmebaD/readme.md). These boards can contain megabytes, not kilobytes of RAM, multi-core CPU's and are able to load in a TCP/IP stack, MQTT and so on.

If there is a theme for my house, it is bookended with reliability, and with that, it's time to put my rack-mounted Raspberry Pi away and adopt a microcontroller. A Raspberry Pi, as great as it is, is an SBC (Single Board Computer) that needs to be updated, watered and fed. It uses a file system, a flash memory subsystem. How does this bode for reliability, and have you ever had a corrupt file system on a microcontroller?

Like any good, opinionated architect, I would urge you to stop, put away your Raspberry Pi's and take a different approach: a microcontroller.

Today, I leverage around 30 outputs on an Arduino Mega 2560 with an Ethernet and PoE shield using MQTT (The pub/sub client library) but it's time to modernise, and given my love of ESP devices with Tasmota, I decided to purchase an ESP32 for this very task.In this post I will illustrate how to build a bridge from Mosquitto MQTT into Azure IoT Hub using this ESP32 device.

I covered in a [prior post](https://automation.baldacchino.net/?p=888) why I am going down this path of publishing telemetry to Azure IoT Hub, along with the several ways I have illustrated how one can go about achieving this goal. From direct connection to Azure IoT Hub (via MQTT and SAS tokens) through to Azure IoT Edge running locally with MQTT and finally the SDK's.I have been able to achieve my goals with varying levels of success but have a few concerns on the approaches I have tried thus far.

-   Direct-Connection to Azure IoT Hub introduces latency to the cloud.
-   Authentication, from SAS tokens to X509 certificates: it's not anonymous and some of my tiny devices (Tasmota) dont bode well.
-   Topic structure: it is defined (devices/{DeviceID}/messsages/events/) and not free form. It means reconfiguration, which isn't hard, but a lot of friction.
-   Reliability: all solutions thus far have relied on a OS which require patching, updating and are even whilst small an administrative burden.

My goals for building a solution

1.  No reconfiguration of any of my MQTT devices (Home Assistant, PLC, Arduino Mega 2560, ~75 Tasmota devices).
2.  Bridge my existing MQTT broker (Mosquitto) in to Azure IoT.
3.  Run on microcontroller, as I want to be reliable.

Pretty lofty goals, you may even say I am being lazy, but the reality is I want a low friction away to derive operational intelligence from the many thousands of events each day (read below, it's over 10K per day!)

## What we are going to build

![](https://automation.baldacchino.net/wp-content/uploads/2021/12/image-1024x418.png)

To overcome, the limitations described above we are going to use an ESP32 microcontroller with C++ code with a libraries. Just incase you are not familar, let me introduce you to the ESP32.

## ESP32

Where do I start? What is not love about this SOC? The ESP32 is a modern, powerful Arduino compliant microcontroller that power many devices from my irrigation controller ([Opensprinkler](https://opensprinkler.com/)) through to my kids learning robot ([MBot](https://www.makeblock.com/steam-kits/mbot)) they are either using an ESP32 or an older derivative such as an ESP8266. Today I am using this as a software bridge but there is a plethora of I/O and support for PWM, I2c and more which make them a versatile all rounder.

> The ESP32 is a series of low-cost, low-power [system on a chip](https://en.wikipedia.org/wiki/System_on_a_chip) [microcontrollers](https://en.wikipedia.org/wiki/Microcontroller) with integrated [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) and dual-mode [Bluetooth](https://en.wikipedia.org/wiki/Bluetooth). The ESP32 series employs either a [Tensilica](https://en.wikipedia.org/wiki/Tensilica) Xtensa LX6 microprocessor in both dual-core and [single-core](https://en.wikipedia.org/wiki/Single-core) variations, Xtensa LX7 dual-core microprocessor or a [single-core](https://en.wikipedia.org/wiki/Single-core) [RISC-V](https://en.wikipedia.org/wiki/RISC-V) microprocessor and includes built-in antenna switches, RF [balun](https://en.wikipedia.org/wiki/Balun), power amplifier, low-noise receive amplifier, filters, and power-management modules. ESP32 is created and developed by [Espressif Systems](https://en.wikipedia.org/w/index.php?title=Espressif_Systems&action=edit&redlink=1), a Shanghai-based Chinese company, and is manufactured by [TSMC](https://en.wikipedia.org/wiki/TSMC) using their 40 nm process. It is a successor to the [ESP8266](https://en.wikipedia.org/wiki/ESP8266) microcontroller.
> 
> [ESP32 - Wikipedia](https://en.wikipedia.org/wiki/ESP32)

Let's build.

## Code Summary

See the steps below as I tease out this solution or my [GitHub repo](https://github.com/baldacchino/pahomqtt_to_azureiot) for the full Arduino sketch. To give you a better understanding on how this works I will break it down in to the logical steps below required to receive messages from Mosquitto over MQTT using

'PubSubClient' and to then re-publish them in to Azure IoT Hub using the 'Esp32MQTTClient'.

### Step 1 - Arduino IDE - Add ESP32 to the Board Manager

The Arduino IDE does not know about the ESP32 so the very first step we need to do leverage the the Arduino IDE's 'Board Manager' capability to provide support for the ESP32. In the Arduino IDE, open 'Preferences' and enter in one of the following URL's

-   Stable release : [https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json](https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json)
-   Development release : [https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_dev_index.json](https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_dev_index.json)

![](https://automation.baldacchino.net/wp-content/uploads/2022/01/image-1.png)

Open 'Boards Manager' from 'Tools' > 'Board' menu. Search for an install _'ESP32'_. Select your specific ESP32 board from the menu post installation.

![](https://automation.baldacchino.net/wp-content/uploads/2022/01/image-2.png)

Restart the Arduino IDE.

### Step 2 - We Need A Library - PubSubClient

Whilst we now have support for the ESP32, we need to add a library that will allow us to subscribe to and receive MQTT messages from our Mosquitto broker. For this very purpose we need a MQTT library. There are many but I have used 'PubSubClient' in the past on other projects without any issues. To install, 'Tools' > 'Manage Libraries' > 'PubSubClient'

![](https://automation.baldacchino.net/wp-content/uploads/2022/01/image-3.png)

### Step 3 - Author Some Code (Libraries and Variables)

After validating your board is working (I would suggest uploading a Blink sketch) we can start coding. This example is based off the 'Examples > ESP32 Azure IoT Arduino > Simple MQTT';

We need to include some libraries, we will be using the Wi-Fi (for connectivity), PubSubClient (for Mosquitto MQTT) and the ESP32MQTTClient (for Azure IoT Hub).

```cpp
#include <WiFi.h>
#include <PubSubClient.h>
#include "Esp32MQTTClient.h"
 
const char* ssid = "Wifi SSID";
const char* password =  "Wifi Password
const char* mqttServer = "IP Address of MQTT Server";
const int mqttPort = 1883;
String MQTTTopic;
String MQTTPayload;
```

Regarding Azure IoT Hub you will need to define your connection string. This post does not cover creating an IoT Hub or creating a device and assumed you have created this prior. See [Use the Azure portal to create an IoT Hub | Microsoft Docs](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) for more information on creating an Azure IoT Hub, adding a device and obtaining a device connection string.

```cpp
//Azure IOT Hub Setup
static const char* connectionString = "*******************=";
static bool hasIoTHub = false;
```

### Step 4 - Author Some Code (Setup Function: Connect to Wi-Fi , Azure and Mosquitto MQTT)

Our 'setup' function will establish connection to our LAN via Wi-Fi and then connect in to Azure where as the 'MQTTConnect' function not only connects to our local MQTT broker, but it defines the MQTT topics to subscribe to. You can subscribe to multiple MQTT topics by having multiple subscribe lines. You can also use MQTT wildcard filters to match events using fewer subscriptions.

> Plus sign (+): It is a single level wildcard that matches any name for a specific topic level. We can use this wildcard instead of specifying a name for any topic level in the topic filter.
> 
> Hash (#): It is a multi level wildcard that we can use only at the end of the topic filter, as the last level and matches any topic whose first levels are the same as the topic levels specified at the left-hand side of the # symbol.
> 
> [Understanding wildcards | MQTT Essentials – A Lightweight IoT Protocol (packtpub.com)](https://subscription.packtpub.com/book/application_development/9781787287815/1/ch01lvl1sec18/understanding-wildcards)

The serial monitor is handy in debugging any issues either with Wi-Fi or connecting in to Azure IoT Hub.

```cpp
client.on_message = on_message  

void setup() {
  //Set baud rate
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.println("ESP32 : Connecting to WiFi...");
  }
  Serial.println("ESP32 : WiFi connected");
  Serial.println("ESP32 : IP address: ");
  Serial.println(WiFi.localIP());
  //Set MQTT details
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
 
  //Connect to Azure IOT
  if (!Esp32MQTTClient_Init((const uint8_t*)connectionString))
  {
    hasIoTHub = false;
    Serial.println("Azure IoT Hub : Initializing IoT hub failed.");
    return;
  }
  hasIoTHub = true;
 
}
    
void MQTTConnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("MQTT : Attempting MQTT connection...");
    // Attempt to connect
    if (client.connect("ESP32Client")) {
      Serial.println("MQTT : Connected");
      // Once connected, publish an announcement...
      client.publish("stat/ESP32/IP_Address","Your IP Address");
      //Subscribe to topics, one topic per line.
      client.subscribe("stat/+/POWER");      
    } else {
      Serial.print("MQTT : Failed to connect to MQTT , rc=");
      Serial.print(client.state());
      Serial.println("MQTT : Trying again to connect to MQTT in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}
```

### Step 4 - Author Some Code (MQTT Call Back & Publish To Azure)

After the setup functions we now need to create a function that will listen for incoming MQTT messages that match our subscription (callback), extract the topic and payload before massaging this data and sending to Azure via another function (AzureIoTHub).

```cpp
void callback(char* topic, byte* payload, unsigned int length) {

  MQTTTopic = String(topic);
  MQTTPayload = ""; 
  for (int i = 0; i < length; i++) {
    // Serial.print((char)payload[i]); - Use for debugging
    MQTTPayload = String(MQTTPayload + (char)payload[i]);
  }    
}


void AzureIoTHub() {
  if (hasIoTHub)
      {
        String tempString;
        tempString = "{" + MQTTTopic + ":" + MQTTPayload + "}";
        if (Esp32MQTTClient_SendEvent(tempString.c_str()))
        {
          Serial.println("Azure IoT Hub : Sending data to Azure IoT Hub succeed");
        }
        else
        {
          Serial.println("Azure IoT Hub : Failure...");
        }
      MQTTPayload = "";
      MQTTTopic = "";

   }
}
```

### Step 5 - Author Some Code (Our Main Loop)

The main loop is leveraging all of these functions and its logic can be best sumarised in to a few points. Connect to MQTT if there is no connection

If there is a MQTT Topic/Message which was decoded via our 'callback' function send this to Azure IoT Hub and re-connect if there is no connection.

```cpp
void loop() {
   //Connect to MQTT and reconnect if connection drops
   if (!client.connected()) {
     MQTTConnect();
   }
   //Respond to messages received
   if (MQTTTopic != "") { 
      Serial.println("MQTT : Topic is [" + MQTTTopic +"]");
      Serial.println("MQTT : Payload is [" + MQTTPayload + "]");
      AzureIoTHub(); 
  }
  client.loop();
}
```

### Pulling It All Together

Here is a complete copy of the above, plus a bit more. You could cut and paste the below or clone my [GitHub repository.](https://github.com/baldacchino/ESP32-MQTT_To_AzureIOT)

```cpp
#include <WiFi.h>
#include <PubSubClient.h>
#include "Esp32MQTTClient.h"
 
const char* ssid = "****";
const char* password =  "****";
const char* mqttServer = "****";
const int mqttPort = 1883;
String MQTTTopic;
String MQTTPayload;

//Azure IOT Hub Setup
static const char* connectionString = "****";
static bool hasIoTHub = false;
 
WiFiClient espClient;
PubSubClient client(espClient);
 
void callback(char* topic, byte* payload, unsigned int length) {

  MQTTTopic = String(topic);
  MQTTPayload = ""; 
  for (int i = 0; i < length; i++) {
    // Serial.print((char)payload[i]); - Use for debugging
    MQTTPayload = String(MQTTPayload + (char)payload[i]);
  }    
}



void MQTTConnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("MQTT : Attempting MQTT connection...");
    // Attempt to connect
    if (client.connect("ESP32Client")) {
      Serial.println("MQTT : Connected");
      // Once connected, publish an announcement...
      client.publish("stat/ESP32/IP_Address","Your IP Address");
      //Subscribe to topics, one topic per line.
      client.subscribe("stat/+/POWER");      
    } else {
      Serial.print("MQTT : Failed to connect to MQTT , rc=");
      Serial.print(client.state());
      Serial.println("MQTT : Trying again to connect to MQTT in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void setup() {
  //Set baud rate
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.println("ESP32 : Connecting to WiFi...");
  }
  Serial.println("ESP32 : WiFi connected");
  Serial.println("ESP32 : IP address: ");
  Serial.println(WiFi.localIP());
  //Set MQTT details
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
 
  //Connect to Azure IOT
  if (!Esp32MQTTClient_Init((const uint8_t*)connectionString))
  {
    hasIoTHub = false;
    Serial.println("Azure IoT Hub : Initializing IoT hub failed.");
    return;
  }
  hasIoTHub = true;
 
}

void loop() {
   //Connect to MQTT and reconnect if connection drops
   if (!client.connected()) {
     MQTTConnect();
   }
   //Respond to messages received
   if (MQTTTopic != "") { 
      Serial.println("MQTT : Topic is [" + MQTTTopic +"]");
      Serial.println("MQTT : Payload is [" + MQTTPayload + "]");
      AzureIoTHub(); 
  }
  client.loop();
}

void AzureIoTHub() {
  if (hasIoTHub)
      {
        String tempString;
        tempString = "{" + MQTTTopic + ":" + MQTTPayload + "}";
        if (Esp32MQTTClient_SendEvent(tempString.c_str()))
        {
          Serial.println("Azure IoT Hub : Sending data to Azure IoT Hub succeed");
        }
        else
        {
          Serial.println("Azure IoT Hub : Failure...");
        }
      MQTTPayload = "";
      MQTTTopic = "";

   }
}
```

### Seeing This In Action

Lets drop to a video to see this in working end-to-end, to validate messages are flowing in to Azure IoT Hub I can use the Azure CLI (AZ-CLI) to monitor the output coupled with the Arduino Serial monitor.

```powershell
az iot hub monitor-events --output table --device-id devicename --hub-name hubname --output json
```

For the purpose of this demo, I have left a handful of messages at QoS level 2 and set LWT (Last Will and Testament) to true.

After 24 hours of running, we can see I have published 10.52K of messages in to Azure IoT Hub and there are certain ebbs and flows that occur in my house.

![](https://automation.baldacchino.net/wp-content/uploads/2021/11/image-1-1024x321.png)

24 hour period of messages flowing in to Azure IoT Hub

## Conclusion

There are many ways to skin this code cat. My requirements was to publish messages in to Azure and we have been able to achieve this via different ways (I am sure there is more). Automation is a journey, which path will you take?

We illustrated a transparent side-car approach that will listen to an existing broker, on topics you desire and push these in to Azure IoT, all without making any configuration changes (the most important thing for my implementation). This method runs on a microcontroller, consumes less than 5w of power and just works.

Are there any draw backs? Sure there are. Right now this is one way in direction (simplex) and allows me to push messages in to Azure IoT but not receive messages back.

Personally, I like this approach, it combines the elegance of a SDK as it's my code and couples the reliability of a microcontroller. It's my code, my choices on what I do, but I do understand this is not for everyone. We now have my messages, my events, in Azure and it's time to make some friends and learn how to derive operational intelligence from visualizations through to machine learning and beyond.

Think big and happy building

---
<!-- enrich:containment -->
*Part of: damianflynn*
