---
title: "Configuring the Web Application Firewall with PowerShell."
date: "2019-10-03"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-af2a74cb.jpg"
Categories:
  [
    "Infrastructre As Code"
  ]
Status: "Published"
Tags:
  [
    "Powershell",
    "WAF"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "af2a74cb-8d5d-4867-acee-dd45dcbd2cee",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/e56b5926-2ff1-4a3a-bffb-b80fbfe228bf/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=99b0c5d92d08592ff5e762fb721e91d9726d62124a33e7933db4a6eb8ce6c5f2&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.690Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Configuring-the-Web-Application-Firewall-with-PowerShell-af2a74cb8d5d4867aceedd45dcbd2cee",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:37:35.832Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

Microsoft Azure Application Gateway is a Layer 7 application delivery controller (ADC) offered as a service in Azure. It provides load balancing, SSL termination, end-to-end SSL, URL path-based routing, and basic web application firewall (WAF) functionality.

Working with the WAF, I usually build a basic configuration in the Portal before exporting the ARM JSON, which, then becomes my primary method to working on this service.

**Why JSON you may ask…**

  One of my biggest gripes with the Azure Firewall solutions currently is based on thier CRUD *(Create, Read, Update and Delete)* interface. It always results in a workflow from hell, training along the lines of ‘*Edit, Save,* ***WAIT,***, *Edit, Save,* ***WAIT***’ in a painful loop. What should be a fast and straightforward configuration update, typically is a process that must be executed over many hours, preferably on a second screen.
  
  **Powershell**

  While I spend a large amount of my working time sitting in VS code, with a terminal logged into Azure with both Powershell and Azure CLI, I do not every recall trying to work with the Application Gateway from this interface ever!
  
  I was asked to check the HTTP Timeout on one of the Firewalls I have access to, and send the details to a colleague; my first port of call was JSON, and then realized that this is a bit ugly for the request in hand.

A quick PowerShell command should sort this out, which of course has even more Idiosyncrasy.

# Working With Application Gateways in PowerShell

Wait for it (Remember my CRUD comment). Well, The PowerShell implementation is restricted by that same API limitations. The first step for updating any existing Gateway is to load the whole gateway configuration from Azure into a PowerShell object.

```powershell
$appGw = Get-AzApplicationGateway -Name p-ap1pub-waf01
```

From here, all the changes we make are to the PowerShell object `$appGw` until we are ready to commit the gateway back to Azure with the following command:

```powershell
Set-AzApplicationGateway -ApplicationGateway $appGw
```

## Making changes

Understanding that all configurations are going to be applied to our in-memory object `$appGw`; we can use any of the available PowerShell commandlets to alter this object, and once ready to validate we must push these changes back to Azure with the `Set-AzApplicationGateway`

> Now you will understand why I ignore this rubbish and work with the ARM JSON!

Let’s take a swift tour of working with this object to establish an end-to-end SSL listener, which would be atypical of any implemented configuration.

## Backend

### Backend Pool

The backend pool for our web servers can be created using FQDNs or IPs; I usually implement this initially using IP addresses.

```powershell
Add-AzApplicationGatewayBackendAddressPool -ApplicationGateway $appGW -Name "AppPool" -BackendIPAddresses "192.168.####101", "192.168.1.102"$appGwBackPool = Get-AzApplicationGatewayBackendAddressPool -ApplicationGateway $appGW -Name "AppPool"
```

### Backend SSL Encryption

Frequently, we establish the connection as a genuine end-to-end encrypted connection; which implies that the Web Application Gateway should have an authentication certificate, so it only forward’s traffic to a backend server if it has the expected SSL certificate.

To enable this, we must upload the public certificate used by the backend servers.

```powershell
Add-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $appGW -Name "AppPoolPublicCert" -CertificateFile ".\myAppPublicCertifcate.cer"$appGwBackPoolCert = Get-AzureRmApplicationGatewayAuthenticationCertificate -ApplicationGateway $appGW -Name "AppPoolPublicCert"
```

### Configure the Backend Service

Now, we can configure the HTTPS Protocol and authentication certificate the WAF will use to validate the backend servers.

```powershell
Add-AzApplicationGatewayBackendHttpSettings -ApplicationGateway $appGW -Name "AppPoolHTTPS" -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $appGwBackPoolCert$appGwBackPoolHTTPS = Get-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $AppGW -Name "AppPoolHTTPS"
```

### Frontend

Now we can switch focus on build the frontend configuration.

### A public SSL certificate.

The certificate should be in PFX format with a password. We add this to the gateway object as follows:

```powershell
Add-AzApplicationGatewaySslCertificate -ApplicationGateway $appGw -Name "FrontCert" -CertificateFile ".\myExternalFacingCertWithPrivateKey.pfx" -Password "myP@ssw0rd!"$appGwFrontCert = Get-AzApplicationGatewaySslCertificate -ApplicationGateway $appGW -Name "FrontCert"
```

### HTTPS Port (TCP 443)

Ensure that we have TCP 443 allowed on our gateway.

```powershell
Add-AzApplicationGatewayFrontendPort -ApplicationGateway $appGw -Name "FrontHTTPS" -Port “443”
$appGwFrontPort = Get-AzApplicationGatewayFrontendPort -ApplicationGateway $appGw -Name "FrontHTTPS"
```

### IP Configuration Object

The last part of this jigsaw is the Frontend IP Configuration.

```powershell
$appGwFrontIPConfig = Get-AzApplicationGatewayFrontendIPConfig -ApplicationGateway $AppGw -Name "FrontIP"
```

### My Applications Listener

Now, we can combine the parts above to create a listener object.

```powershell
Add-AzApplicationGatewayHttpListener -ApplicationGateway $appGW -Name "appListener" -Protocol Https -FrontendIPConfiguration $appGwFrontIPConfig -FrontendPort $appGwFrontPort -HostName "fqdn.myapp.site" -RequireServerNameIndication true -SslCertificate $AppGwFrontCert$appGwListener = Get-AzureRmApplicationGatewayHttpListener -ApplicationGateway $appGW -Name "appListener"
```

### Routing Flow

Now we glue these two concepts together and build the route from the listener to the server.

```powershell
Add-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appGW -Name "AppRule" -RuleType basic -BackendHttpSettings $appGwBackPoolHTTPS -HttpListener $appGwListener -BackendAddressPool $appGwBackPool
```

### Publish

That’s - Now we publish and test to see if we got it working, or messed up.

```powershell
Set-AzApplicationGateway -ApplicationGateway $appGw
```

## Reporting Configuration

Ok, Distraction aside, we started this post, as we needed to get a quick report on all the listeners configured, including the associated timeout each has currently defined.

With the knowledge of how to work with the App Gateway in PowerShell, this request is indeed trivial to complete

```powershell
> $appGw.BackendHttpSettingsCollection | select name, port, protocol, requesttimeout
Name                             Port Protocol RequestTimeout
----                             ---- -------- --------------http_t-www.mysite.com              80 Http                 20https_t-www.mysite.com            443 Https                20
```

