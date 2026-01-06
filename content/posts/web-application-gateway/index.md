---
title: "Web Application Gateway"
date: "2019-11-07"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
featuredImage: "./cover-43f3797e.jpg"
Categories:
  [
    "Networking Infrastructure & Security"
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
    "id": "43f3797e-5aef-4e9c-a5ae-f0fa7c2aa016",
    "created_time": "2024-07-11T13:40:00.000Z",
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
      "type": "file",
      "file": {
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/950e4e3c-9674-4ff8-834f-d7aa43a79c87/banner-ignite2019-waf.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZFGLRHR%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T122325Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGFTNuBcKvG%2Fqu6tyoZhaHiCAthGOPmVm2oOlRkNlUb2AiAa0pPJgPLeYdknz9V9O2n0LKhRt8L1pI%2B%2BLyEVKknprSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMV874XB%2BiWfZanK1xKtwDFHnP7nwIwd%2F2xvt263dUfOTRlpiMY40hPDK10aRtiQUFfsnTQpiv88qMsm7flrbTogSoABXPjeUNT28cidFl22wSHC3TPw%2FS0maDNXxWDltGpxXL9wR77hiOUPFjKcHnTEgDhhBOQ462O43ibvfi%2BIsshT0er%2ByhKJb8frAlcWR9wI4lrE9v4%2Bylkfl2LrJ8MjNTwGr%2BJwKyakrK4EFp7GUjFD5q1OvCjv9wMqh%2FxkiJX%2B3Ac2XIhva30lTMRnb9H8VcNTn8UXstXM22UNpLZol4SUrPKLLk6vFAkoOurj1vVaICAB3kIjtSv4MyeqmWnEDauOput2R%2FboeHzukjTYZ4mL4VdmKROu3SMMiZTZshgRn0bMV%2FJfQbjNTGHDrC1hloC0mf4W5yIdIqFhgj6tg10797H6mT%2BfZ1Qao%2BEDxroU3YJuWFBSDorueT9y9HrmcfjvOHt1103audHzoxteqFKWO%2BoSDj%2BrnLzBGmJAlNQCE9iANDYLrcchvG4IUVeBY9oTFxANPFjWyJQCLkDpud14oijx2nlcmgeUGfiUMMj9P%2BO5Dvm0NKSidW2v0QEt5xmzYKWPj6fcrZvTH50dqUgMfzuObw2LyRxjozU2xuf0MuZxEOPR%2Bp%2BqIwqN7zygY6pgGRHTbhlGjdNdN5dFMDxxjGtk7lSjm4m0iPPE9XVCEVQxNXpg6wmJK3SnKcMj%2FCo31220xDefnk11k0T849vBaB%2F38E%2FRaGPK80HxrOh2qGETRLV238eDnvetaW4cPhyx2w38nWIN690AMh0pNSIZF1dUfd8o1wTfzogzuuMtXcNLxkvLBEFgTOZbErVwJkeXVdWDZRC39oHFe%2FOqU4pZvEpmkYU7uX&X-Amz-Signature=d203f77fe5625a2fe689f69a5425f4f68d66be378a5e66b654a0a0ff1075ee8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-06T13:23:25.917Z"
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
    "url": "https://www.notion.so/Web-Application-Gateway-43f3797e5aef4e9ca5aef0fa7c2aa016",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:25:38.467Z"
last_edited_time: "2024-07-19T15:22:00.000Z"
---

Delivering PaaS Services Privately on Azure VNets with Private Link

  Mission Critical HTTP Applications, there are many things to consider

Personalised, Micro-Services, Rich Context…. To support this MS have a number of services i the Suite - Azure Frontdoor, Application Gateway, Azure CDN, Web Application Firewall, Azure Load Balancer, and Azure Traffic Manager

# Azure Application Gateway

Regional Gateway as a service

  Standard V2 SKU in GA, Currently Available in 26 regions, Builtin Zone Redundancy, Static VIP, HTTP Header/cookies insertion/modification

* Increased scale limits 20 -> 100 Listeners
* Key Vault integration and auto-renewal of SSL Certs
* AKS ingress Controller
## Autoscaling and Performance Improvements

* Grow and shrink based on app traffic requirements
* 5X better SSL offloads performance
* 500-50,000 connections/sec with RSA 2048 bit certs
* 30,000-3,000,000 persistent connections
* 2,500-250,000 reqs/sec
## Announcing General Availability:

* Frontend TLS cert integration with Azure Key Vault
* Utilized user-assigned managed identity access control for key vault
* User Certificates or secrets on key vault
* Polls every 4 hours to enable automatic cert renewal
* manual override of specific certificate version retrieval
* Manipulate Request and Response headers & cookies
  * Strip port from X-Forwarded-for header
  * Add security headers like HSTS and X-XSS-Protection
  * Common header manipulation ex HOST, SERVER
  ## AKS Ingress Control using Application Gateways

* Deployed using Helm
* Utilizes Pod-AAD for ARM authentication
* Tighter integration with AKS add on support coming
* Support URI path based, host based, SSL termination, SSL re-encryption, redirection, custom health probes, draining, cookie affinity
* Support for Lets Encrypt provide TLS certificates
* WAF fully supported with custom listener policy
* Support for multiple AKS as backend
* Support for mixed mode - both AKS and other backend types on the same Application Gateway
[**http://aka.ms/appgwaks**](https://aka.ms/appgwaks)

## Wild Card Listener

* Support for Wildcard characters in the listener host name
* Support for * and ? Characters in host name
* Associated wildcard or SAN certificates the service HTTPS enabled domains
* Send traffic to multiple tenant end points
## Diagnostics and logs enhancements

* TLS Protocol
* TLS Cipher
* Backend target server
* backend response code
* backend latency
## Metrics

* Backend response status code
* RPS healthy nodes
* End to End Latency
* Backend Latency
* Backend connect, first byte and last byte latency
## App Monitor Insights for Application Gateway

Single health and metic console for your entire cloud network No agent/configuration required

# Azure WAF - Cloud Native WEB Application Firewall

Unified WAF offering to protect your apps at network edge or region uniformly

## Public preview announced

Microsoft threats intelligence

* Protect agains automatic attacks
  * Managed good and bad bots with Azure BotManager Rule Set
  * Data is refreshed daily
  * Easy to configure in WAF policy
  * Helps increase your applications performance, by stopping aggressive crawlers.
  * Site and URI path specific WAF Policies
  * Customized WAF police at the region WAF
  * Assign different Policies to different sites
  * Site specific polices implies you can tune the WAF to suit the needs of each site independently
  * Geo filtering on regional WAF
  * Allow or Block a list of countries,
  * Support log mode
  * Rule Set for CRS 3.1 added (to be the default soon)
* Integration with Azure Sentinel
* Performance and concurrency enhancements
