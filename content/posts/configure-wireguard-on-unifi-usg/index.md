---
title: "Configure Wireguard on UniFi USG"
date: "2019-08-17"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-a4bb7193.jpg"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/2daf9917-5d3a-419c-83a2-f2f96c72c010/wireguard.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=e11ac2f30f67f2e6b5b6664e07d1a6858ed42a223f4ecde7ab2b441a5c4b1e96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
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
    "url": "https://www.notion.so/Configure-Wireguard-on-UniFi-USG-a4bb7193fbd2413087126d424412486f",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:26:06.211Z"
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

