---
title: AD - Online Certificate Responder
date: '2021-04-15T00:00:00Z'
draft: true
slug: ad-online-certificate-responder
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/ad-online-certificate-responder/index.md
neocortex_source_sha: 2a57a13362e7e3907381a649e512f9c0e41518f15a33f59df20eaf9566d57ec1
description: Certificate Revocation Lists are typically published to a HTTP site as
  static files, however the OCSP service provides an always current, API to the PKI
  environment to validate certificate health
tags:
- IaaS
- PKI
- Delegation
categories:
- Azure
featuredImage: hero.png
---

## Prepair the Certificate Service

For the CA to be able to use OCSP, read permission to the private key must be given.

Add Read permissions to Network Service on the private key
* Open the Certificate Templates snap-in.
* Select the OCSP Response Signing template.
* Right-click it and click on properties.
  ![OCSP Cert Template](./index_2021-04-15-11-58-20.png)

* Go to tab security. Click on add.
* In the dialog, select from the list of object types computer.
  * Search for the CA/OCSP computer. Click OK.
* Select the newly created entry with the computer name of the OCSP responder and select *ALLOW* for **Read** and **Enroll** permissions.
  ![](./index_2021-04-15-11-59-26.png)

* Finish the task by clicking on OK.

## Publish the Certificate Template

Next, you will have to include the OCSP certificate in the list of available certificates of the CA.


* Open the CA snap-in 
* Select *Certificate Templates*
* Right click and choose “New Certificate Template to Issue”
  * Select the **OCSP Response Signing** certificate.

To check that it worked, select the certificate and open its properties.

## Online Responder Installation

```powershell
Add-WindowsFeature ADCS-Online-Cert
```

### Revocation Configuration

![Name the Revocation Configuration](./index_2021-04-15-10-34-25.png)

![Select the Certificate Location](./index_2021-04-15-10-36-48.png)

![Choose the CA Certificate](./index_2021-04-15-10-37-36.png)

![OCSP Response Signing Certificate](./index_2021-04-15-10-51-08.png)

![Revocation List Provider](./index_2021-04-15-10-52-21.png)

### Enable NONCE

Edit OCSP *Revocation Configuration* properties.

* Go to tab Signing and enable NONCE.
  ![](./index_2021-04-15-12-22-34.png)

### Test the OCSP

In case you get a signing certificate not available for the array controller, do a refresh of the node.

![Refresh the Configuration](./index_2021-04-15-12-27-36.png)

In the CA, an OCSP signing certificate must appear in the list of issued certificates.

## Configure CA

Open the Certification Authority snap-in.


* Open the properties of the CA.
* Open the **extensions** tab. By default, the CRL distribution point (CDP) list is shown. We need to change from **CDP** to **Authority Information Access** (AIA)
  
  ![CA Issuer Extension Properties](./index_2021-04-15-12-08-58.png)

* Click on **Add** to add a new location.

  Specify the locations from which users can obtain certificate revocation data. This is the URL under which the OCSP service is installed.Make sure that the clients can resolve the DNS name and communicate with the service.

  http://<ServerDNSName>/ocsp


* Select “Include in the online certificate status protocol (OCSP) extension”. This makes the OCSP URL available in the certificate.

  ![Enable the OCSP Response](./index_2021-04-15-12-19-29.png)

You will have to restart the CA service to make the new configuration effective.

---
<!-- enrich:containment -->
*Part of: damianflynn*
