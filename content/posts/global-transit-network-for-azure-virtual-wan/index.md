---
title: "Global Transit Network For Azure Virtual WAN"
date: "2019-11-07"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
Categories:
  [
    "Azure Network Connectivity"
  ]
series:
  [
    "Ignite 2019"
  ]
Status: "Published"
Tags:
  [
    "Conference"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "0f9cc744-c421-487d-bc6b-6a014bb616fc",
    "created_time": "2024-07-11T14:01:00.000Z",
    "last_edited_time": "2024-07-19T15:22:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": {
      "type": "external",
      "external": {
        "url": "https://www.notion.so/images/page-cover/nasa_bruce_mccandless_spacewalk.jpg"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Global-Transit-Network-For-Azure-Virtual-WAN-0f9cc744c421487dbc6b6a014bb616fc",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:11:01.667Z"
last_edited_time: "2024-07-19T15:22:00.000Z"
---

The Journey started with the concept of VNets, with work loads, and have evolved in the direction of Subnets, and quickly became a very complex list of islands which were disconnected

* Security
* Public Cloud
* SaaS, Internet
* Users
* Branch Offices
Virtual WAN is a managed service

* Managed by Microsoft with global scale, and multplie endpoints.
* Each Hub can support 60Gb of connectivity;
  * Including 20Gb of ExpressRoute.
  * 20Gb of User VPN
  * 20Gb Site to Site
  * Supports 10K users per hub, 1000 sites per hub
* Transit Routing
* Cloud Network orchestration
  * Automation large scale branch, SDWAN CPE connectivity
  # Overview

Simplified networking, ease of user operations, and cost savings:

* Any-to-Any Connectivity
* Full mesh hubs
* Branch to Azure
* Branch to Branch
* VPN <-> ExpressRoute
* User VPN <-> Site
* vNet to vNet
## Whats New

* Any-to-Any connectivity (Preview)
* Express Route , User VPN (Point to Site) GA
* ExpressRoute Encryption
* Multi Link Azure Path Selection
* Custom IPSec
* Connect VNG VPN to Virtual WAN
* Available in Gove Cloud and China
* Azure Firewall integration (preview)
* Pricing
## Virtual WAN Types

### Basic

* VPN Only
* Branch to Azure
* Branch to Branch
* Connect VNET
* DIY VNet Peering (VNet to VNet - no transitive)
### Standard = Basic +

Multi Link Support in VPN Sites

Dynamic traffic distribution across ISP at the branch site

## Express Route (Standard VWan)

20Gb aggregate throughput

Private Connectivity

* Requires Premium Circuit
* In Global Reach LocationExpressRoute VPN Interconnect
* ExpressRoute and Site-to-Site/Point-to-Site User VPNExpressRoute to ExpressRoute (Premium)
## Express Route Encryption

IPSec over Express Route (Azure Azure Private IP)

## User VPN

IPSec and OpenVPN support for up to 10K users

## Azure Firewall

Firewall in Virtual Hub
Centralised Policy and route managmenet
* VNET to Inernet via Firewall
* Branhc to ingtern via the firewall

## MSP Partner Program

Announced in July 2019 - in the Azure Marketplace

## Pricing

Connections, Traffic, Aggregate via the Hubs

Connection Unit
* Site to Site VPN 0.05/hour
* User VPN 0.03/hour

Scale Unit
1 Unit = .361/h 500Mb
1 ER Scale Unit = 0.42/hr 2Gbos

Virtual Hub
* Basic Hub - Free
* Standard - 0.25/hour

## Zero Thrust Networking

### Microsegmention

* Segment
Prevent Lateral Movement and data exfilration
* Protect
* Connect

Cloud Native Services, all software defined resources implement the Defence in Depth offer, the resources included are:
* Azure Firewall
* Azure Web Application Firewall
* Azure Private Link
* Azure DDoS Protection
* Virtual Network
* Network Security Groups
* User Defined Routes
* Load Balancer

Network Segmentation
Host Based - With agent Installed
HyperVistor Baed - VMWare NSX
Network Based - Softwaew Defined Networking

1. Subscription
Logic isolation of environemtn and all resoruces
1. Virtual Network
Isoared and highly secure enviroonment to run virtual machines and applications
1. Network Security Group
Enforce and control network traffic securitly rules to allow or deny traiffc fro a vnet or vm
1. Web Application Firewall
Application specific network security
1. Azure Firewall
