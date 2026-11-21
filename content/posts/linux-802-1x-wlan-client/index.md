---
title: Linux 802.1x WLAN Client
date: '2020-03-21T00:00:00Z'
draft: true
slug: linux-802-1x-wlan-client
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/linux-802-1x-wlan-client/index.md
neocortex_source_sha: 71524629b9f83810b302c703d71e586ae103a1dc4a17269020f67f4498b395a2
description: Guide to installing an alternative Radius service, for those who may
  have implemented Windows NPS Server - License Free and Open Source, Enterprise Ready
tags:
- RADIUS
- PKI
- Linux
- 802.1X
- Wi-Fi
categories:
- Networking
- Identity and Security
series:
- RADIUS in a Cloud World
featuredImage: banner.png
---

Attaching Windows and MacOS to a 802.1x network is assisted using wizards, or enterprise management features, however for Linux this is typically a slightly different experience

Assuming you have access to Issue a Certificate which will be sufficient to authenticate the connection, for example a User Certificate or Computer Certificate we can utilize this on our system to establish a connection.

You may need to try both, as your backend Radius service which is validating the 802.1x connection maybe requiring a specific certificate for this authentication flow.

## Requesting a User Certificates

From a Windows system, that is already trusted on the environment

* Open mmc
* Request a new certificate
  * Personal -> Certificates -> All Tasks –> Request New Certificate…
* Export the certificate with private key *.pfx*

Now you have the Exported certificate, including its private key, we should transfer it to the Linux system

## Convert the `.pfx` certificate

We can use OpenSSL to convert our PFX file into the required certificate files on the target system.

The client certificate

```bash
openssl pkcs12 -in exported.pfx -clcerts -nokeys -out client.pem
```

The private key

```bash
openssl pkcs12 -in exported.pfx -nocerts -out client.key -nodes
```

The CA Cert

```bash
openssl pkcs12 -in exported.pfx -cacerts -nokeys -chain -out ca.pem
```

### Configure supplicant

I copied the all the certificates into the `/etc/wpa_supplicant` folder.

The next step is to configure the supplicant to use the certificates `/etc/wpa_supplicant/wpa_supplicant.conf` to read similar to the following

```conf
#######
network={
    ssid="YourSSID"     
    key_mgmt=WPA-EAP    
    proto=WPA2
    eap=TLS
    pairwise=CCMP
    group=CCMP
    identity="MetzgerC"
    ca_cert="/etc/wpa_supplicant/ca.pem"
    client_cert="/etc/wpa_supplicant/client.pem"
    private_key="/etc/wpa_supplicant/client.key"
    private_key_passwd="THE-PASSWORD-YOU-HOPEFULLY-WROTE-DOWN"
}
```

Afterwards we need to tell the wLan Interface to use this configuration. Edit the `/etc/network/interfaces`

```conf
iface wlan0 inet dhcp   
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

---
<!-- enrich:containment -->
*Part of: damianflynn*
