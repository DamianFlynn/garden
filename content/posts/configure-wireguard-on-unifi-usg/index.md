---
title: "Configure Wireguard on UniFi USG"
date: "2019-08-17"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Status: "Published"
Tags:
  [
    "VPN"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "a4bb7193-fbd2-4130-8712-6d424412486f",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/2daf9917-5d3a-419c-83a2-f2f96c72c010/wireguard.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=b12965d459e5188250aab4f4c08dd723a495ff304ea3f1aee22a0a13d0c66494&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Configure-Wireguard-on-UniFi-USG-a4bb7193fbd2413087126d424412486f",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:56:53.382Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

**Install the Wireguard Package**

SSH directly to your USG, and run the following commands:

```shell
curl -L https://github.com/Lochnair/vyatta-wireguard/releases/download/0.0.20190123/wireguard-ugw3-0.0.20190702-1.deb -o /tmp/wireguard.deb
dpkg -i /tmp/wireguard.deb
```

# Create the Tunnel Secrets

To keep stuff private, we will encrypt the traffic using a long password, known as a ‘Key’. To make sure this is unique, we will use a tool provided by Wireguard to make a random key for us.

```shell
cd /config/auth
umask 077
mkdir wireguard
cd wireguard
wg genkey > wg_private.key
wg pubkey < wg_private.key > wg_public.key
```

## Configure the Tunnels

While still connected to the USG, we will now create the Interface which will be our end of the tunnel. If we consider this as a Bridge, then as we configure this interface, we will provide the address for our side and also the address of the far side.

The far side is protected from just anyone connecting to it by using another long password (key) which we need to know before we can complete this process.

In this example 192.168.33.1 is assumed to be your network, you should change these to match your network space.

```shell
# Set the USG into configuration Mode
configure
# We start, by creating a new Network space for our side of the VPN
set interfaces wireguard wg0 address 10.192.10.2/32
# Configure the Port Wireguard will be listening with
set interfaces wireguard wg0 listen-port 51820
# Allow this interface to forward the traffic over our tunnel
set interfaces wireguard wg0 route-allowed-ips true
# Now, we need to tell the interface the address of the far side of the bridge
# And also the password to allow us connect
set interfaces wireguard wg0 peer <Insert-Public-Key-Of-Peer-Here> endpoint 14.28.207.179:51820

# Now, we will tell the far side of the bridge about our network
# This is to ensure that the far side lets our network get out of the tunnel
# The sample only allows the IPs 192.168.33.101 to 192.168.33.106 to cross over
# you can choose to let everything by using the address 0.0.0.0/0
set interfaces wireguard wg0 peer <Insert-Public-Key-Of-Peer-Here> allowed-ips 10.192.10.0/32
# Lets tell the interface where to find our long password we created earlier
set interfaces wireguard wg0 private-key /config/auth/wireguard/wg_private.key
# Make the changes active, save them and exit configuration mode
commit
save
exit
```

### Firewalls block traffic

And our tunnel is no exception, so we need to allow our new Tunnel Interface to be permitted to let the traffic flow. In this case we need to let the far side of the bridge connect back to us; after all there is no point sending traffic over if nothing can come back !

```shell
# Set the USG into configuration Modeconfigure
# Configure the firewall
set firewall name WAN_LOCAL rule 20 action accept
set firewall name WAN_LOCAL rule 20 protocol udp
set firewall name WAN_LOCAL rule 20 description 'WireGuard'
set firewall name WAN_LOCAL rule 20 destination port 51820
# Make the changes active, save them and exit configuration mode
commit
save
exit
```

Afterwards dump the `config.gateway.json` and put it in the controller so it do not get overwritten

### Reconfigure after an Update

Copy your backed up `config.gateway.json` to `/var/lib/unifi/data/sites/default` on the system running the Controller (which might also be a Cloud Key).

Then through the Controller Web UI navigate to **Devices**, click on the **USG** row and then in the **Properties** window navigate to **Config > Manage Device** and click **Provision**.

