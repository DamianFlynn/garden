---
title: "Configuration the Ubiquity EdgeRouter with WireGuard"
date: "2019-10-05"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-799b3640.jpg"
Status: "Published"
Tags:
  [
    "VPN"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "799b3640-cd41-4ee1-bb6f-3c52c0790aef",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/7b33b953-85fd-4276-8616-7e42ab3a62da/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=ba09a32225ba20ba6b2b8a4412984805e0e683552bdf4c3aeda64ab73a9cd2ab&X-Amz-SignedHeaders=host&x-id=GetObject",
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
    "url": "https://www.notion.so/Configuration-the-Ubiquity-EdgeRouter-with-WireGuard-799b3640cd414ee1bb6f3c52c0790aef",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:36:43.231Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

Virtual Private Networks are unmissable; however with many states now banning and actively blocking these tunnels the search for an alternative approach is appropriate, if we are to protect our identity and intellectual property.

One technology which claims to have fantastic throughput when compared to the stable IPSEC solutions, and based on benchmarks I have calculated leaves OpenVPN protocols in the dust, Wireguard positions itself as an in-kernel VPN solution which is very easy to implement and highly secure.

This post will attempt to address the first challenge of implementing the technology, and later we can run our own benchmarks to see how in reality performance measures up.

# Network Overview

The Environment I will implement will contain two endpoints, both of which are directly connected to the public internet.

The following table, defines the configuration points of the environment

  # Node Installation

## Ubuntu 16.04

### Azure

```shell
export azResourceGroup="USEast_VPNGateway"
export azLocation="eastus"

# VNET supports 32 subnets from 192.168.192.0 - 192.168.199.255
export azVnetName="USEast_Networks"
export azVnetPrefix="192.168.192.0/21"

# Allocating the first /24 subnet for the VPNGateway
export azGWSubnetName="USEast_VPNGateway"
export azGWSubnetPrefix="192.168.192.0/24"

# Define the Gateways WAN Interface
export azGWWanNicName="USEast_VPNGateway_WAN"
export azGWWanNicPrivateIp="192.168.192.4"
export azGWWanNicPublicIpName="USEast_VPNGateway_WAN_PublicIP"
export azGWWanNicPublicIpDNS="useast-gw"

# Define the Gateway VM
export azGWVmName="Gateway-USEast"
export azGWVmSize="Standard_A1_v2"
export azGWVmUsername="damian"

# Create a resource group.
az group create \
   --name $azResourceGroup \
   --location $azLocation

# Create a virtual network with one subnet
az network vnet create \
   --name $azVnetName \
   --resource-group $azResourceGroup \
   --location $azLocation \
   --address-prefix $azVnetPrefix \
   --subnet-name $azGWSubnetName \
   --subnet-prefix $azGWSubnetPrefix
   
# Create a public IP address resource
# Append the --allocation-method Static option for the allocation to be static
# The DnsName must be unique within the
az network public-ip create \
   --name $azGWWanNicPublicIpName \
   --resource-group $azResourceGroup \
   --location $azLocation \
   --dns-name $azGWWanNicPublicIpDNS \
   --allocation-method Static

# Create a network interface connected to the VNet 
# with a static private IP address and associate the 
# public IP address# resource to the NIC.
az network nic create \
   --name $azGWWanNicName \
   --resource-group $azResourceGroup \
   --location $azLocation \
   --subnet $azGWSubnetName \
   --vnet-name $azVnetName \
   --private-ip-address $azGWWanNicPrivateIp \
   --public-ip-address $azGWWanNicPublicIpName
   
# Create a new VM with the NIC
az vm create \
   --name $azGWVmName \
   --resource-group $azResourceGroup \
   --location $azLocation \
   --image UbuntuLTS \
   --size $azGWVmSize \
   --nics $azGWWanNicName \
   --authentication-type password \
   --admin-username $azGWVmUsername
```

# WireGuard Installation

## Ubuntu 16.04

Installing is quick and simple, we just need to register the repository, add its content to our catalog index, and install the two packages we require. The following script acomplishs this for us

```shell
apt-get update
apt-get upgrade
add-apt-repository ppa:wireguard/wireguard
apt-get update
apt-get install wireguard-dkms wireguard-tools
```

## EdgeRouter

Download the corresponding package from `https://github.com/Lochnair/vyatta-wireguard/releases`.
You will need to select the version based on your hardware

  You can choose to upload to the routing, or ssh login routing, sudo su to root , the implementation of similar orders

```shell
curl https://github.com/Lochnair/vyatta-wireguard/releases/download/0.0.20170612-2/wireguard-octeon-0.0.20170612-2.deb -o wireguard-octeon-0.0.20170612-2.deb
```

After the download is complete, you can install the package with the following command

```shell
sudo dpkg -i ./wireguard-ralink-${RELEASE}.deb
```

# Configuration

## Virtual Interfaces

Creating the Virtual Interface which will be the endpoint for out Point to Point tunnel is not complex. Convention with Wireguard is to name the interface starting with `wg0`.

### Ubuntu 16.04

In the Linux shell, we can add the new interface by leveraging the `ip` tools

```shell
ip link add dev wg0 type wireguard
ip address add dev wg0 192.168.2.1/24
```

### EdgeRouter

In configuration mode, we can leverage the `set interfaces` command to establish the interface

```shell
configureset interfaces wireguard wg0 address 192.168.2.2/24
```

## Security Keys

WireGuard is a asymmetric cryptography key-based VPN solution. We therefore need to generate a key pair for each node . We do this by creating a private key and then deriving a public key from it. Wireguard’s own userspace tool `wg` assists with this task.

Lets create the key pair for the first node, which we will refer to as the *server*…

```shell
wg genkey > server_private.key
wg pubkey > server_public.key < server_private.key
```

And, the second pair we need for the second node, which we can refer to as the *router*…

```shell
wg genkey > router_private.key
wg pubkey > router_public.key < router_private.key
```

> Important: For security we should be storing the key pair only on the same system on which it is used. If you created both key pairs on the same node, then ensure that you transfer the private key securely to the other node and remove it from original node.

This should leave us with two files for the *server* and another two files for the *router*. Make sure you don’t confuse these two pair’s! The tunnel won’t work if the private and public keys of the endpoints are not correctly distributed!

For illustrative purposes, this is what the content of each of the Key files looks like

  ## Server Configuration

### Ubuntu 16.04

Create a configuration file named `wireguard.conf` and store it for example in `\etc\wireguard\`

```plain text
[Interface]
ListenPort = 51820
PrivateKey = "insert `server_private.key` content goes here"

[Peer]
Endpoint = ip:port of endpoint (88.81.97.90:51820)
PublicKey = "insert `router_public.key` content goes here"
AllowedIPs = 0.0.0.0/0
```

Once defined, we can link the configuration to Wireguard, and then turn up the Virtual Interface

```shell
wg setconf wg0 /etc/wireguard/wireguard.conf
ip link set up dev wg0
```

### Auto Load on Server Restart

Using the `rc.local` file, we can have the tunnel endpoint automatically created at system boot

```shell
# /etc/rc.local
# Add, Configure and establish the Wireguard Tunnel
ip link add dev wg0 type wireguard
ip address add dev wg0 192.168.2.1/24
wg setconf wg0 /etc/wireguard/wireguard.conf
ip link set up dev wg0
# Lets Configure the Firewall next...
```

### EdgeRouter

On the edge router, we can complete the configuration as follows

```shell
set interfaces wireguard wg0 listen-port 51820
set interfaces wireguard wg0 route-allowed-ips false
set interfaces wireguard wg0 private-key /config/auth/router_private.key
set interfaces wireguard wg0 peer "insert `server_public.key` content goes here" description "insert 'Nickname'"
set interfaces wireguard wg0 peer "insert `server_public.key` content goes here" allowed-ips 0.0.0.0/0
set interfaces wireguard wg0 peer "insert `server_public.key` content goes here" endpoint ip:port of endpoint (40.71.229.107:51820)
set interfaces wireguard wg0 peer "insert `server_public.key` content goes here" persistent-keepalive 25

commit
save
```

## Validation

### Server

```shell
sudo wg
interface: wg0
  public key: 3cv0P5Q8o9rR4nyq3mx8iTid2CMjab4nrqbo5lgm4lc=
  private key: (hidden)  listening port: 51820
peer: NkVVh4yt+ZO9FVgMtImHesXoWba1Wzrb5HXrvMg+rjY=
  endpoint: 88.81.97.90:51820
  allowed ips: 0.0.0.0/0
  latest handshake: 36 seconds ago
  transfer: 572 B received, 156 B sent
```

### Router

```shell
sudo wg
interface: wg0
  public key: NkVVh4yt+ZO9FVgMtImHesXoWba1Wzrb5HXrvMg+rjY=
  private key: (hidden)  listening port: 51820
peer: 3cv0P5Q8o9rR4nyq3mx8iTid2CMjab4nrqbo5lgm4lc=
  endpoint: 40.71.229.107:51820
  allowed ips: 0.0.0.0/0
  latest handshake: 5 seconds ago
  transfer: 92 B received, 452 B sent
```

## Perfomance Testing

My router is connected to a 30Mb/30Mb service hosted by Westnet, and my average speedtest.net results at the time of running this next test was as follows

  ### Server

```shell
$ sudo apt install iperf3
$ iperf3 -s
-----------------------------------------------------------Server listening on 5201
-----------------------------------------------------------
```

### Router

The iPref3 application is pre-installed with the EdgeRouter so we just need to run the tool, which should automatically connect with the server we have listening

```shell
$ iperf3 -c 192.168.2.1
Connecting to host 192.168.2.1, port 5201
[  4] local 192.168.2.2 port 53159 connected to 192.168.2.1 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr[  4]   0.00-1.09   sec   768 KBytes  5.75 Mbits/sec    0
[  4]   1.09-2.10   sec  1.12 MBytes  9.43 Mbits/sec    0
[  4]   2.10-3.10   sec  1.75 MBytes  14.7 Mbits/sec    0
[  4]   3.10-4.10   sec  2.88 MBytes  24.1 Mbits/sec    0
[  4]   4.10-5.09   sec  4.38 MBytes  36.7 Mbits/sec    0
[  4]   5.09-6.10   sec  3.38 MBytes  28.3 Mbits/sec  171
[  4]   6.10-7.09   sec  2.50 MBytes  21.0 Mbits/sec    0
[  4]   7.09-8.09   sec  2.88 MBytes  24.1 Mbits/sec    0
[  4]   8.09-9.10   sec  3.00 MBytes  25.1 Mbits/sec    0
[  4]   9.10-10.09  sec  3.00 MBytes  25.2 Mbits/sec    0
- - - - - - - - - - - - - - - - - - - - - - - - -[ ID] Interval           Transfer     Bandwidth       Retr[  4]   0.00-10.09  sec  25.6 MBytes  21.3 Mbits/sec  171         sender
[  4]   0.00-10.09  sec  24.4 MBytes  20.3 Mbits/sec              receiver
iperf Done.
```

The report should also be presented in the console of the server, similar to the following

```shell
Accepted connection from 192.168.2.2, port 53158
[  5] local 192.168.2.1 port 5201 connected to 192.168.2.2 port 53159
[ ID] Interval           Transfer     Bandwidth[  5]   0.00-1.00   sec   457 KBytes  3.74 Mbits/sec
[  5]   1.00-2.00   sec   919 KBytes  7.53 Mbits/sec
[  5]   2.00-3.00   sec  1.59 MBytes  13.3 Mbits/sec
[  5]   3.00-4.00   sec  2.46 MBytes  20.6 Mbits/sec
[  5]   4.00-5.00   sec  3.78 MBytes  31.7 Mbits/sec
[  5]   5.00-6.00   sec  3.19 MBytes  26.7 Mbits/sec
[  5]   6.00-7.00   sec  2.61 MBytes  21.9 Mbits/sec
[  5]   7.00-8.00   sec  2.87 MBytes  24.1 Mbits/sec
[  5]   8.00-9.00   sec  3.00 MBytes  25.1 Mbits/sec
[  5]   9.00-10.00  sec  3.01 MBytes  25.3 Mbits/sec
[  5]  10.00-10.19  sec   598 KBytes  25.2 Mbits/sec
- - - - - - - - - - - - - - - - - - - - - - - - -[ ID] Interval           Transfer     Bandwidth       Retr[  5]   0.00-10.19  sec  25.6 MBytes  21.1 Mbits/sec  171             sender
[  5]   0.00-10.19  sec  24.4 MBytes  20.1 Mbits/sec                  receiver
```

### Results

The results of the test over the tunnel, when compared to my available bandwidth turns out.

  Very Impressive!

### Enable IPv4 routing

First make sure IP forwarding is enabled by adding the following to `/etc/sysctl.conf`

```plain text
net.ipv4.ip_forward=1
```

Run the following command to apply the above setting:

```shell
sudo sysctl -p
```

# Router Policy Based Routing

## Group of devices to route

```shell
configureset firewall group address-group VPN_USEast address 172.16.1.197
set firewall group address-group VPN_USEast address 172.16.1.198
commitexit
```

### Table for route

Next, the configuration is the route table.
192.168.2.1 is wg0 network remote gateway address, wg0 local ip is 192.168.2.2, so do not need additional configuration 192.168.2.0/24 device routing.

```shell
configureset protocols static table 5 route 0.0.0.0/0 next-hop 192.168.2.1
#set protocols static table 1 interface-route 0.0.0.0/0 next-hop-interface wg0commitexit
```

```shell
show ip route table 5
```

### Define a Modify Policy

The firewall configuration is as follows

```shell
configure

set firewall modify VPN_USEast_ROUTE rule 10 description 'Traffic from VPN_USEast to wg0'
set firewall modify VPN_USEast_ROUTE rule 10 source group address-group VPN_USEast
set firewall modify VPN_USEast_ROUTE rule 10 modify table 5
```

### Apply the Policy to the Switch interface

```shell
set interfaces switch switch0 firewall in modify VPN_USEast_ROUTE
```

### Apply the Policy

```shell
set service nat rule 5003 description VPN_USEast_MASQ
set service nat rule 5003 log disable
set service nat rule 5003 outbound-interface wg0
set service nat rule 5003 source group address-group VPN_USEast
set service nat rule 5003 type masquerade
```

### Linux

> This is to be removed

```shell
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wg0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wg0 -o eth0 -j ACCEPT
```

# Installation

# Configuration

Copy the output of the last command down to the server

After configuring the device, configure the nat service

```shell
configure

set service nat rule 5032 outbound-interface wg0
set service nat rule 5032 type masquerade

commit
save
```

The rest is to configure the routing rules, and can refer to my

# Configuration Reference

A firewall is just a set of rules that get run over every packet that comes or goes through the device it’s running on. The rules look at things like which interface the packet is coming or going from, what ports it’s using, what protocols it’s using, the IPs involved, etc.. It uses the packet’s properties to determine some sort of action like dropping the packet, rewriting it, or accepting it. When all the conditions of a rule match a packet, the action is applied to that packet. In Linux, the de facto firewall program is called iptables. It’s been around for decades and is incredibly powerful. On the other hand it’s often criticized for being complex and unforgiving. Alternatives like the “uncomplicated firewall” (ufw) have sprung up to try and make firewall configuration easier, but under the hood all they are doing is writing iptables rules. I don’t think the basics of iptables are that complicated though if you just give it a chance. So let me explain how it works at a high level and I’ll demonstrate some basic rules that can lock down your router as well, if not better, than a commercial router.

## Tables and Chains

Iptables works on these things called tables (obviously). There are 5 different tables it uses: raw, mangle, nat, filter, and security. Each of these tables are applied at a different stage of packet processing and so they can be used to achieve different things. This is just the basics though, so all we’re going to focus on are the nat and filter tables.

Each of these tables contain chains, which are just a list of rules. There are 5 default chains called, *PREROUTING*, *INPUT*, *FORWARD*, *OUTPUT*, and *POSTROUTING*. Not every chain is applicable in every table though. For example the nat table only uses *PREROUTING* and *POSTROUTING* chains and the filter table only uses *INPUT*, *FORWARD*, and *OUTPUT* chains. For now lets focus on *INPUT*, *FORWARD*, and *OUTPUT*.

When a packet goes though the Linux network stack the chain of rules that get applied depends on where the packet is coming from and where it is going. If the packet is destined for the machine the firewall is running on, it would get run through the *INPUT* chain (the packet is an input to this system). If it is destined for some other machine then it gets run through the *FORWARD* table (the packet is being forwarded to another system). If the packet originated from the machine the firewall is running on it gets run through the *OUTPUT* table (the packet is an output of this system).

Like I said before, each of these chains is just a list of rules. Once the appropriate chain is picked based on the packet’s source and destination the rules are run in order from the start of the list until a rule is matched. Each rule has a policy associated with it that says what to do with the packet if that rule is matched. The most common policies are *ACCEPT* and *DROP* which allow the packet through the firewall or deny it respectively. There are a lot more policies that let you gracefully reject packets, jump around to other rule chains, and more, but again, this is just the basics.

So what happens if no rules are matched? In fact this is what’s happening on your router right now. Since you haven’t added any rules there are no rules to match. Each chain has a default policy which says what to do if no rules are matched. By default the policy is *ACCEPT*. See:

```shell
$> sudo iptables -LChain INPUT (policy ACCEPT)target     prot opt source               destination
Chain FORWARD (policy ACCEPT)target     prot opt source               destination
Chain OUTPUT (policy ACCEPT)target     prot opt source               destination
```

This command lists *(-L)* the chains, rules, and default policies of an iptables table. By default it lists the filter table. To list a different table like nat you could add `-t nat` to the command. You can see for that each of the chains *INPUT*, *FORWARD*, and *OUTPUT* the policy if no rules are matched is *ACCEPT*. This is what I mean by your router is not secure right now. It will literally accept anything, from anywhere, going to anywhere. If you don’t understand why this is bad for something connected to the public internet then you probably aren’t the type of person that should be building their own router.

A default *ACCEPT* policy is *OK* for the *OUTPUT* table. We’ll assume that the router hasn’t been hacked and any traffic it wants to send out to the world is acceptable. For the *INPUT* and *FORWARD* tables, which process packets from the outside world, you definitely want to change the default policy to *DROP*.

> WARNING: DO NOT DO THIS OVER SSH. IT WILL DROP YOUR SSH CONNECTION:

```shell
$> sudo iptables -P INPUT DROP
$> sudo iptables -P FORWARD DROP
```

And see the change:

```shell
$> sudo iptables -LChain INPUT (policy DROP)target     prot opt source               destination
Chain FORWARD (policy DROP)target     prot opt source               destination
Chain OUTPUT (policy ACCEPT)target     prot opt source               destination
```

OK now you’re safe from haxors. You’re router is dropping all your LAN traffic too though so you’re also kind of fucked. We’ll need to add some more rules to allow the traffic we actually want.

## Adding Rules

By default iptables rules are not persistent. This is good because if you fuck something up you can just reboot the machine and everything will be fine. Once you’ve got a working set of rules though you’ll want them to be applied automatically at boot. There are some packages like `iptables-persistent` and other bullshit like that to do it for you, but for simple folk there’s `/etc/rc.local`. Just put your commands in there and they’ll get run on every boot.

OK, now for some rules. I highly recommend running these on the command line and testing everything works before putting them into `/etc/rc.local` if you’re doing this over SSH.

### Input Chain Rules

First of all, let’s accept anything from the loopback interface:

```shell
$> sudo iptables -A INPUT -i lo -j ACCEPT
```

And lets also allow anything on the LAN to send traffic to the router itself:

```shell
$> sudo iptables -A INPUT -i wg0 -j ACCEPT
```

You should now be able to see these rules in the INPUT chain *(*`*-v*`* just enables verbose output to show the interface names the rule applies to)*:

```shell
$> sudo iptables -L -vChain INPUT (policy DROP 50250 packets, 10M bytes) pkts bytes target  prot opt in               out  source    destination
1140K  199M ACCEPT  all  --  lo               any  anywhere  anywhere
  14M 3023M ACCEPT  all  --  wg0  any  anywhere  anywhere
...
```

These rules go in the *INPUT* chain because we want them to apply to traffic destined to the router itself.
* The `-A` just means append to the end of the current list of rules.
> Remember the rules are evaluated in order until a rule is matched so the order these things are inserted into the chain matters.
* The `-i` option says to match this rule the packet must be coming from the **wg0 interface** *(which is my LAN interface)*.
* Finally the `-j ACCEPT` says the policy to apply is to accept the packet.
Putting that all together we get “a packet from the LAN interface destined to the router should be accepted”.

Next, let’s allow traffic from the WAN to the router. We don’t just want the router to accept any packets from the WAN though. It should only accept packets that are part of a connection the router itself initiated. This prevents any random person on the internet from sending traffic to the router, but still ensures the router can still receive responses from the internet when it wants to. Since iptables does connection tracking its simple to make a rule like this with just a few more options:

```shell
$> sudo iptables -A INPUT -i eth0 -m conntrack \        --ctstate ESTABLISHED,RELATED -j ACCEPT
```

Most of those options should make sense already, but let me explain the new ones: `-m` and `--ctstate`.

* m option specifies a “match” to use. This is just some extra condition the packet must match for the rule to be applied. In this case we want to match on connection state so the conntrack matching extension is used.
* ctstate option says which types of connections should be matched. To allow traffic from connections initiated by the router the rule needs to match ESTABLISHED and RELATED packets. This means a packet will be accepted if it is part of an already established TCP connection, or if it is related to a TCP connection in the process of being set up (router sends SYN, the SYN/ACK the server responds with is a “related” packet).
### Forward Chain Rules

So now we can send traffic from the LAN to the router, from the router to the LAN, and from the router to the WAN. But what about from the LAN to the WAN? For that we will need forwarding rules. Recall rules in the FORWARD table apply to packets from somewhere other than the router going to somewhere other than the router (neither source or destination IP is the router’s). The packets are just being forwarded through. The rules are very similar to the ones from before.

To accept traffic being forwarded from the LAN to the WAN:

```shell
$> sudo iptables -A FORWARD -i wg0 -o eth0 -j ACCEPT
```

In other words “accept packets coming in the LAN interface and going out the WAN interface”. Just like the router, we should accept traffic from the WAN going to the LAN if, and only if, the LAN initiated the connection:

```shell
$> sudo iptables -A FORWARD -i eth0 -o wg0 -m conntrack \        --ctstate ESTABLISHED,RELATED -j ACCEPT
```

Alrighty, that should do it for filter table rules. There’s still ony problem though. If a LAN device sends a packet to the WAN the source IP address will be a LAN address (something in the 192.168.0.0/24 space). These IPs are unrouteable on the public internet and will be dropped the second the packet leaves your house. Instead the router should change the source IP address on outgoing packets to its WAN IP. Then when a reply is received it should change the destination IP back to the LAN IP address and forward it along. This procedure is called Network Address Translation, aka NAT, and this is where the nat table comes into play.

### NAT

To translate IP addresses between local addresses and publicly routable addresses, we’ll need to add some rules to the PREROUTING and POSTROUTING chains of the nat table. These chains allow you to modify packets when they’re received and as they are being transmitted respectively. So to translate LAN IP addresses to the router’s WAN IP address we will have to add a POSTROUTING rule that rewrites the address just before the packet is sent out. Fortunately, iptables has built in things to make this easy. There is a policy called MASQUERADE that will do all the translation for us. All we need to do is add a rule and iptables will take care of all the NAT rewriting on outgoing and incoming packets that match the rule.

```shell
$> sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

To see that it worked you can add `-t nat` to the `iptables -L` command to see the nat table rules:

```shell
$> sudo iptables -t nat -L -vChain PREROUTING (policy ACCEPT 2130 packets, 684K bytes)pkts bytes target prot opt in out source destination
Chain INPUT (policy ACCEPT 1640 packets, 661K bytes)pkts bytes target prot opt in out source destination
Chain OUTPUT (policy ACCEPT 15342 packets, 1363K bytes)pkts bytes target prot opt in out source destination
Chain POSTROUTING (policy ACCEPT 15337 packets, 1362K bytes)pkts bytes target     prot opt in  out   source   destination
1200 83980 MASQUERADE all  --  any eth0  anywhere anywhere
```

Boom, that’s it, you should have a working router. Test things out and make sure everything is working properly. If so then make sure to add all the iptables rules to your `/etc/rc.local` so they will be added after every boot. The completed thing should look something like this:

```shell
# /etc/rc.local# Default policy to drop all incoming packets
iptables -P INPUT DROP
iptables -P FORWARD DROP
# Accept incoming packets from localhost and the LAN interface
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -i wg0 -j ACCEPT
# Accept incoming packets from the WAN if the router initiated# the connection
iptables -A INPUT -i eth0 -m conntrack \
    --ctstate ESTABLISHED,RELATED -j ACCEPT
# We also need to accept SSH to this interface
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# Forward LAN packets to the WAN
iptables -A FORWARD -i wg0 -o eth0 -j ACCEPT
# Forward WAN packets to the LAN if the LAN initiated the# connection
iptables -A FORWARD -i eth0 -o wg0 -m conntrack \
    --ctstate ESTABLISHED,RELATED -j ACCEPT
# NAT traffic going out the WAN interface
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# rc.local needs to exit with 0
exit 0
```

Now stop, and back things up. Unless you want to do all that configuration over if something goes awry it’s a good idea to copy all the configuration files somewhere safe.

## Azure Hosting

If you choose to host your Ubuntu Wireguard gateway in Azure, then the Azure virtual network will require the route tables to be modified to route the packets from the virtual network interface of the Ubuntu server correctly.

Recall that in our environment, we established a Wireguard tunnel between our Ubuntu server and Edgerouter, which is running as *192.168.2.0/24*, with the remote network behind the Edgerouter been defined as *172.16.1.0/24*.

### Create the UDR for the back-end subnet

To create the route table and route needed for the back-end subnet Run the following command

```shell
export azVnetUDR="USEast_UDR"
az network route-table create --name $azVnetUDR \
   --resource-group $azResourceGroup \
   --location $azLocation
```

Run the following command to create a route in the route table to send all traffic destined to the Edgerouter subnet (172.16.1.0/24) to the Gateway VM (192.168.192.4):

```shell
export azVnetUDRRoutes="USEast-Gateway-Routes"
az network route-table route create --name $azVnetUDRRoutes \
   --resource-group $azResourceGroup \
   --route-table-name $azVnetUDR \
   --address-prefix 192.168.192.0/24 \
   --next-hop-type VirtualAppliance \
   --next-hop-ip-address 192.168.192.4
```

Run the following logging command to create a route in the route table to send all traffic destined to the Edgerouter subnet (192.168.2.0/24) to the Gateway VM (192.168.192.4):

```shell
export azVnetUDRRoutes="USEast-Gateway-NAT-Routes"
az network route-table route create --name $azVnetUDRRoutes \
   --resource-group $azResourceGroup \
   --route-table-name $azVnetUDR \
   --address-prefix 192.168.2.0/24 \
   --next-hop-type VirtualAppliance \
   --next-hop-ip-address 192.168.192.4
```

Run the following command to associate the route table with the Gateways subnet (called *default*):

```shell
az network vnet subnet update --name $azGWSubnetName \
   --resource-group $azResourceGroup \
   --route-table $azVnetUDR \
   --vnet-name $azVnetName
```

### Enable IP forwarding on FW1

To enable IP forwarding in the NIC used by Gateway, complete the following steps:

```shell
az network nic update \
   --resource-group Wireguard_USEast \
   --name wireguard-useast835 \
   --ip-forwarding true
```

## Router

```json
firewall {
  all-ping enable
  broadcast-ping disable
  group {
     network-group PRIVATE_NETS {
       network 192.168.0.0/16
       network 172.16.0.0/12
       network 10.0.0.0/8
     }
   }
   ipv6-receive-redirects disable
   ipv6-src-route disable
   ip-src-route disable
   log-martians disable
   modify balance {
     rule 10 {
       action modify
       description "do NOT load balance lan to lan"
       destination {
         group {
           network-group PRIVATE_NETS
         }
       }
       modify {
         table main
       }
     }
     
     rule 20 {
       action modify
       description "do NOT load balance destination public address"
       destination {
         group {
           address-group ADDRv4_eth0
         }
       }
       modify {
         table main
       }
     }
     
     rule 30 {
       action modify
       description "do NOT load balance destination public address"
       destination {
         group {
           address-group ADDRv4_eth1
         }
       }
       modify {
         table main
       }
     }
     
     rule 70 {
       action modify
       modify {
         lb-group G
       }
     }
   }
   name WAN_IN {
     default-action drop
     description "WAN to internal"
     rule 10 {
       action accept
       description "Allow established/related"
       state {
         established enable
         related enable
       }
     }
     rule 20 {
       action drop
       description "Drop invalid state"
       state {
         invalid enable
       }
     }
   }
   name WAN_LOCAL {
     default-action drop
     description "WAN to router"
     
     rule 10 {
       action accept
       description "Allow established/related"
       state {
         established enable
         related enable
       }
     }
     rule 20 {
       action drop
       description "Drop invalid state"
       state {
         invalid enable
       }
     }
   }
   receive-redirects disable
   send-redirects enable
   source-validation disable
   syn-cookies enable
}
interfaces {
   ethernet eth0 {
     address 88.81.97.90/29
     address 88.81.97.91/29
     address 88.81.97.92/29
     address 88.81.97.93/29
     address 88.81.97.94/29
     description WAN
     duplex auto
     firewall {
       in {
         name WAN_IN
       }
       local {
         name WAN_LOCAL
       }
     }
     speed auto
   }
   ethernet eth1 {
     address dhcp
     description "WAN 2"
     duplex auto
     firewall {
       in {
         name WAN_IN
       }
       local {
         name WAN_LOCAL
       }
     }
     speed auto
   }
   ethernet eth2 {
     duplex auto
     speed auto
   }
   ethernet eth3 {
     duplex auto
     speed auto
   }
   ethernet eth4 {
     duplex auto
     speed auto
   }
   loopback lo {
   }
   switch switch0 {
     address 172.16.1.1/24
     description Local
     firewall {
       in {
       }
     }
     mtu 1500
     switch-port {
       interface eth2 {
       }
       interface eth3 {
       }
       interface eth4 {
       }
       vlan-aware disable
     }
   }
   wireguard wg0 {
     address 192.168.2.2/24
     listen-port 51820
     peer 3cv0P5Q8o9rR4nyq3mx8iTid2CMjab4nrqbo5lgm4lc= {
       allowed-ips 0.0.0.0/0
       description "Azure USEast"
       endpoint 40.71.229.107:51820
     }
     private-key /config/auth/wg.private
     route-allowed-ips false
   }
}
load-balance {
   group G {
     interface eth0 {
     }
     interface eth1 {
       failover-only
     }
     lb-local enable
     lb-local-metric-change disable
   }
}
protocols {
   static {
     route 0.0.0.0/0 {
       next-hop 88.81.97.89 {
       }
     }
   }
}
service {
   dns {
     forwarding {
       cache-size 150
       listen-on switch0
     }
   }
   gui {
     http-port 80
     https-port 443
     older-ciphers enable
   }
   nat {
     rule 5000 {
       description "masquerade for WAN"
       outbound-interface eth0
       type masquerade
     }
     rule 5002 {
       description "masquerade for WAN 2"
       outbound-interface eth1
       type masquerade
     }
   }
   ssh {
     port 22
     protocol-version v2
   }
   unms {
     disable
   }
}
system {
   conntrack {
     expect-table-size 4096
     hash-size 4096
     table-size 32768
     tcp {
       half-open-connections 512
       loose enable
       max-retrans 3
     }
   }
   host-name ubnt
   login {
     user ubnt {
       authentication {
         encrypted-password $...
       }
       level admin
     }
   }
   name-server 8.8.8.8
   name-server 8.8.4.4
   ntp {
     server 0.ubnt.pool.ntp.org {
     }
     server 1.ubnt.pool.ntp.org {
     }
     server 2.ubnt.pool.ntp.org {
     }
     server 3.ubnt.pool.ntp.org {
     }
   }
   syslog {
     global {
       facility all {
         level notice
       }
       facility protocols {
         level debug
       }
     }
   }
   time-zone UTC
   traffic-analysis {
     dpi enable
     export enable
   }
}
/* Warning: Do not remove the following line. */
/* === vyatta-config-version: "config-management@1:conntrack@1:cron@1:dhcp-relay@1:dhcp-server@4:firewall@5:ipsec@5:nat@3:qos@1:quagga@2:system@4:ubnt-pptp@1:ubnt-unms@1:ubnt-util@1:vrrp@1:webgui@1:webproxy@1:zone-policy@1" === */
/* Release version: v1.9.7+hotfix.2.5010181.170818.0412 */
```

