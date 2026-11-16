---
title: Az Billing Subscription
date: '2022-11-07T00:00:00Z'
draft: true
slug: creating-azure-subscriptions-with-terraform
author: Damian Flynn
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/creating-azure-subscriptions-with-terraform/index.md
neocortex_source_sha: d41fb5df924616ae90c3d8d8b58b11bffa3bb7bc3b2add8acccb7be3c91e4c48
description: ''
tags: []
---

# Creating Azure Subscriptions with Terraform

---
#UiT #Concierge #WiP #EA #Subscriptions

Terraform Azure Subscriptions Ref from FLS work - `/Users/damianflynn/Developer/InnofactorOrg/fls/acf-fls-tf-subscriptions`

https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/direct-ea-administration

https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/cost-management-billing/manage/grant-access-to-create-subscription.md

https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/assign-roles-azure-service-principals

https://learn.microsoft.com/en-us/rest/api/billing/2019-10-01-preview/enrollment-account-role-assignments/put?tabs=HTTP#code-try-0

https://learn.microsoft.com/en-us/rest/api/billing/2019-10-01-preview/role-assignments/put?tabs=HTTP#code-try-0

https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/mca-setup-account
---

A little time back, I posted an article on working with the Azure [Enterprise Billing Services, for Privilage Delegation](/posts/azure-ea-delegating-privileges/). That article looked at the organisation structure of the Enterprise Agreement portal and how to establish an **account** which could be used for creating new subscriptions, associating them with the correct billing context.

Since then, the billing architecture has evolved, with more scenarios presented, and more features enabled, including different billing relationships, and new programatic options for establishing subscriptions.


If you’ve ever tried to create Azure Subscriptions with Terraform, azCLI or via ARM api you’ll know that it was not possible up until recently. Previously you had to open a different UI to choose a billing model and create a new Subscription (even if it was bound to the same AD Tenant as your existing Subscription). Terrform and AZ work in the context of a Subscription so it was assumed you would have one before you started, however sometimes we need to create these automatically. It looks like Microsoft are addressing this issue and converging their services and have introduced an API (preview) for programmatically creating subscriptions on the fly. 

## Add Subscription Extension

Before we begin, let's be sure we have the **Subscriptions** extension added to our AZ CLI instance, by running the following command in your active context:

```
az extension add --name subscription
```

## Creating Subscriptions

### Billing ID

Before we proceed to create any subscriptions, you are going to need to get the relevant Billing details, to which this subscription will be atttached before it will be possible to comission your subscription.

The following command should present the options available, to you, .

```
az billing enrollment-account list
```

::: note
If you get an empty reply back, then you need to have the delegation checked first, as we can not proceeed without the billing information
:::

Typically, the reply we get should look similar to the following

```json
[
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/7760268a-blah-blah-this-secretdf64c9",
    "name": "7760268a-blah-blah-this-secretdf64c9",
    "principalName": "azure-test@company.com",
    "type": "Microsoft.Billing/enrollmentAccounts"
  }
]

```

There are two main account types Enterprise and Dev/Test (get better prices on your Azure resources in dev!), The following table presents the Codes for the main subscription options available to us

|Code| Type |
|---|---|
|MS-AZR-0017P| Production |
|MS-AZR-0148P | Dev Test |

### Create the subscription using AZ Cli

The following command will be used for creating a new subscription

```bash
az account create --offer-type "MS-AZR-0017P" --display-name "p-gov" --enrollment-account-object-id "/providers/Microsoft.Billing/enrollmentAccounts/7760268a-blah-blah-this-secretdf64c9" --owner-object-id "<userObjectId>","0882673f-a426-418f-9681-0bd0c54f63b0"
```

### Create the Subscription using Terraform

```hcl
terraform {
  required_providers {
    azurerm = ">= 2.54.0"
  }
}


variable "billing_account_name" {
  description = "(required)"
  type        = string
}

variable "enrollment_account_name" {
  description = "(required)"
  type        = string
}

variable "timeouts" {
  description = "nested block: NestingSingle, min items: 0, max items: 0"
  type = set(object(
    {
      read = string
    }
  ))
  default = []
}


data "azurerm_billing_enrollment_account_scope" "this" {
  # billing_account_name - (required) is a type of string
  billing_account_name = var.billing_account_name
  # enrollment_account_name - (required) is a type of string
  enrollment_account_name = var.enrollment_account_name

  dynamic "timeouts" {
    for_each = var.timeouts
    content {
      # read - (optional) is a type of string
      read = timeouts.value["read"]
    }
  }

}

output "id" {
  description = "returns a string"
  value       = data.azurerm_billing_enrollment_account_scope.this.id
}

output "this" {
  value = azurerm_billing_enrollment_account_scope.this
}

```



```hcl
data "azurerm_billing_enrollment_account_scope" "example" {
  billing_account_name    = "61732179"
  enrollment_account_name = "149836"
}

resource "azurerm_subscription" "example" {
  subscription_name = "p-gov"
  billing_scope_id  = data.azurerm_billing_enrollment_account_scope.example.id
}
```

_**Note: Please make sure that you are using all the correct details which are can be found from EA portal.**_

```hcl
data "azurerm_subscription" "this" {
  # subscription_id - (optional) is a type of string
  subscription_id = var.subscription_id

  dynamic "timeouts" {
    for_each = var.timeouts
    content {
      # read - (optional) is a type of string
      read = timeouts.value["read"]
    }
  }

}

```

---
<!-- enrich:containment -->
*Part of: damianflynn*
