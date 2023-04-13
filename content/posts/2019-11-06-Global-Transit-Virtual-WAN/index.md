---
title: Global Transit Network For Azure Virtual WAN
type: article 
layout: post 
description: Global Transit Network For Azure Virtual WAN
date: 2019-11-02 21:09:09
categories: ['Network']
tags: ['Az CLI', 'PowerShell', 'Enterprise Agreement']
authors: ['damian'] 
draft: True
image: /images/2019/11/06/banner.jpg
toc: false 
featured: false 
comments: false 
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


Simplosde networking exas of user operations svings
* Any to Any Connectiy
* Full mesh hubs
* branch to azure
* branch to branch
* VPN <-> Express Route
User VPN <->Site
* vNet to vNet



# Whats New

* Any-to-Any connectivity (Preview)
* Express Route , User VPN (Point to Site) GA
* ExpressRoute Encryption
* Multi Link Azure Path Selection
* Custom IPSec
* Connect VNG VPN to Virtual WAN
* Available in Gove Cloud and China
* Azure Firewall integration (preview)
* Pricing

# Virtual WAN Types

## Basic
* VPN Only
Branch to Azure
Brnach to Branch
Connect VNET
  DIY VNet Peering (VNet to VNet - no transitive)

## Standard = Basic + 

Multi Link Support in VPN Sites

Dynamic traffic distrivtion across ISP at the branch site

# Express Route (Standard VWan)

20Gb aggregate troughtput
Private COnnectivity
* Requires Premiun Cirtucit
* In Global Reach Location
ExpressRoute VPN Interconnect
* ExpressRoute and Site to site/Ponint to site User VPN
Express Route to Express Route (Premium)

# Express Route Encryption

IPSec over Express Route (Azure Azure Private IP)

# User VPN

IPSec and OpenVPN support for up to 10K users

# Azure Firewall

Firewall in  Virtual Hub
Centralised Policy and route managmenet
* VNET to Inernet via Firewall
* Branhc to ingtern via the firewall

# MSP Partner Program

Announced in July 2019 - in the Azure Marketplace

# Pricing

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



# Zero Thrust NEtworking

## Microsegmention

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
2. Virtual Network
  Isoared and highly secure enviroonment to run virtual machines and applications
3. Network Security Group
  Enforce and control network traffic securitly rules to allow or deny traiffc fro a vnet or vm
4. Web Application Firewall
  Application specific network security

5. Azure Firewall