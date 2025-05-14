---
title: "Web Application Gateway"
date: "2019-11-07"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
Categories:
  [
    "Networking Infrastructure & Security"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/950e4e3c-9674-4ff8-834f-d7aa43a79c87/banner-ignite2019-waf.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=6804f1d55aa2856188d3b5ff1fb93bb218ddc5ab7bf4b8021c91ccf21ba48b64&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-13T11:55:29.454Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Web-Application-Gateway-43f3797e5aef4e9ca5aef0fa7c2aa016",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:32.329Z"
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
