---
title: "IoT - Azure IoT Operations"
date: "2025-04-25T23:07:00.000Z"
lastmod: "2025-11-20T23:23:00.000Z"
draft: true
Categories:
  [
    "IoT",
    "Azure Edge Devices"
  ]
series:
  [
    "Azure IoT Operations"
  ]
Status: "Draft"
Tags:
  [
    "IaC",
    "Linux",
    "IoT",
    "DevOps",
    "Docker"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "1e0eb56e-a1c3-8046-af39-e4c5ca860492",
    "created_time": "2025-04-25T23:07:00.000Z",
    "last_edited_time": "2025-11-20T23:23:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": null,
    "icon": null,
    "parent": {
      "type": "data_source_id",
      "data_source_id": "235a5f88-c313-46d9-84b5-9f168a1633b7",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/IoT-Azure-IoT-Operations-1e0eb56ea1c38046af39e4c5ca860492",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:46:44.039Z"
last_edited_time: "2025-11-20T23:23:00.000Z"
EXPIRY_TIME: "2026-01-06T13:46:30.917Z"
---

An Azure Arc-enabled Kubernetes cluster is a prerequisite for deploying Azure IoT Operations. This article describes how to prepare a cluster before you deploy Azure IoT Operations. This article includes guidance for Ubuntu


> [!warning] ⚠️ 
> Microsoft supports Azure Kubernetes Service (AKS) Edge Essentials for deployments on Windows and K3s for deployments on Ubuntu. If you want to deploy Azure IoT Operations to a multi-node solution, use K3s on Ubuntu.
Starting from an Ubuntu server, which has be Arc Enabled, we can use this as a base for the Azure IoT Operations.

1. Azure Arc enabling an Ubuntu Server
1. K3s Kubernettes on Ubuntu (Single or Multi-Node)
1. Azure IoT Operations
# IoT Operations

We discuss Azure IoT Operations *deployments* and *instances*, which are two different concepts:

* An Azure IoT Operations deployment describes all of the components and resources that enable the Azure IoT Operations scenario. These components and resources include:
  * An Azure IoT Operations instance
  * Arc extensions
  * Custom locations
  * Resources that you can configure in your Azure IoT Operations solution, like assets and asset endpoints.
  * An Azure IoT Operations instance is the parent resource that bundles the suite of services that are defined in What is Azure IoT Operations? like MQTT broker, data flows, and connector for OPC UA.
When we talk about deploying Azure IoT Operations, we mean the full set of components that make up a *deployment*. Once the deployment exists, you can view, manage, and update the *instance*.

![Image](img-1e0eb56e-image.png)

## Prerequisites


### Storage Account for Schemas

Schema registry requires an Azure Storage account with hierarchical namespace and public network access enabled. When creating a new storage account, we need to provide a name, for example `pwe1iotstore`  

```bash
# Set variables
RESOURCE_GROUP="p-we1iot"
LOCATION="westeurope"

STORAGE="pwe1iotstore"

# Setupp the Storage Account, it will be used for the Schema
az storage account create \
  --name "$STORAGE" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --sku Standard_LRS \
  --kind StorageV2 \
  --enable-hierarchical-namespace true \
  --min-tls-version TLS1_2 \
  --allow-blob-public-access false \
  --tags 'Purpose=SchemaRegistry,Shared=true'
  
# Get the Storage Account Resource ID "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.Storage/storageAccounts/pwe1iotstore"
STORAGE_ID=$(az storage account show \
  --name "$STORAGE" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv) 
```


We next need a container, that we can use to host the Schemas.

```bash
SCHEMA_CONTAINER="schemas"

# Create the Scheama Container
az storage container create \
  --name "$SCHEMA_CONTAINER" \
  --account-name "$STORAGE" \
  --auth-mode login
```

### IoT Namespace

Create the Namespace

```bash
NAMESPACE="p-we1iot"

# Create the Name Space that we will use for registry
az iot ops ns create \
  --name "$NAMESPACE" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --tags 'Purpose=DeviceRegistry,Shared=true'
  
NAMESPACE_ID=$(az iot ops ns show \
  --name "$NAMESPACE" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv)
```

### Schema Registry

```bash
SCHEMA_REGISTRY="p-we1iot-schema"

# Create the Schema Registry, with the Namespace and Storage Container
az iot ops schema registry create \
  --name "$SCHEMA_REGISTRY" \
  --resource-group "$RESOURCE_GROUP" \
  --registry-namespace "$NAMESPACE" \
  --sa-resource-id "$STORAGE_ID" \
  --sa-container "$SCHEMA_CONTAINER" \
  --location "$LOCATION" \
  --tags 'Purpose=SchemaRegistry,Shared=true'
  
# Get the New Schema Registry Resource ID '8132265e-8c4f-4fa3-85f1-f283ab0c9fab'
SCHEMA_REGISTRY_ID=$(az iot ops schema registry show \
  --name "$SCHEMA_REGISTRY" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv) 
```

Perfect! The schema registry was created with a system-assigned managed identity. Now we need to grant this identity permissions to the storage account:

```bash
# Delegate Storage Blob Data Contributor to the Registery
az role assignment create \
  --assignee "$SCHEMA_REGISTRY_ID" \
  --role "Storage Blob Data Contributor" \
  --scope "$STORAGE_ID"
```

### Key Vault

We will use the Key-vault to support the Managed Identity's ability to retrieve secrets and certificates  securely. As before, lets proceed to deploy a new Key Vault

```bash
KEYVAULT="p-we1iot-kv"

# Lets create the keyvault
az keyvault create \
  --name "$KEYVAULT" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --enable-rbac-authorization true \
  --public-network-access Enabled \
  --tags 'Purpose=Secrets,Shared=true'

# Get The Keyvault Resource ID  
KEYVAULT_ID=$(az keyvault show --name "$KEYVAULT" --query id -o tsv)
```

### Managed Identity For Secrets

We need 2 managed identities for this solution, for both we will be using **User Assigned Managed Identities **

```bash
SECRETS_IDENTITY="p-we1iot-secrets-mi"

# Create managed identity for IOT Secrets
az identity create \
  --name "$SECRETS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --tags 'Purpose=KeyVaultAccess,Shared=true'


# Get all the identity details we'll need
SECRETS_MI_RESOURCE_ID=$(az identity show \
  --name "$SECRETS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv)

SECRETS_MI_PRINCIPAL_ID=$(az identity show \
  --name "$SECRETS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --query principalId -o tsv)

SECRETS_MI_CLIENT_ID=$(az identity show \
  --name "$SECRETS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --query clientId -o tsv)
  

```


We need to delegate the new managed identity the **Key Vault Secret Offices privilages**

```bash
az role assignment create \
  --assignee "$SECRETS_MI_PRINCIPAL_ID" \
  --role "Key Vault Secrets Officer" \
  --scope "$KEYVAULT_ID"
```


### Managed identity for Azure IoT Operations Components

```bash
IOTOPS_IDENTITY="p-we1iot-n013edge-mi"

# Create managed identity for n013edge building
az identity create \
  --name "$IOTOPS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --tags 'Building=013,Purpose=IoTOperations'

{
  "clientId": "37a6d03f-6fd0-4dbd-9bdb-0f35bfa2b04e",
  "id": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourcegroups/p-we1iot/providers/Microsoft.ManagedIdentity/userAssignedIdentities/p-we1iot-n013edge-mi",
  "location": "westeurope",
  "name": "p-we1iot-n013edge-mi",
  "principalId": "d58d9e9b-0a90-4f6c-98c5-638c5b9abe5d",
  "resourceGroup": "p-we1iot",
  "systemData": null,
  "tags": {
    "Building": "013,Purpose=IoTOperations"
  },
  "tenantId": "7d918dfe-bd63-478b-b8b3-2f252b011527",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}

# Get all the identity details we'll need
IOTOPS_MI_RESOURCE_ID=$(az identity show \
  --name "$IOTOPS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv)

IOTOPS_MI_PRINCIPAL_ID=$(az identity show \
  --name "$IOTOPS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --query principalId -o tsv)

IOTOPS_MI_CLIENT_ID=$(az identity show \
  --name "$IOTOPS_IDENTITY" \
  --resource-group "$RESOURCE_GROUP" \
  --query clientId -o tsv)

```



# Portal Deployment Tool

The Azure portal deployment experience is a helper tool that generates a deployment command based on your resources and configuration. The final step is to run an Azure CLI command.

## Portal Helper

In the [Azure portal](https://portal.azure.com/), search for and select **Azure IoT Operations**.

![Image](img-1e0eb56e-image.png)


1. Select Create.
1. On the Basics tab, provide the following information:
  **Expand table**
  
    ![Image](img-1e0eb56e-image.png)
  
  1. Select Next: Configuration.
1. On the Configuration tab, provide the following information:
  **Expand table**
  
    ![Image](img-1e0eb56e-image.png)
  
  1. On the Dependency management page
  1. We can drop down the Namespace and select the iotops we created earlier. 
  1. However, you will notice that the drop down for the Schema Registry is empty. This is a bug, so, just click on Create New for the page to popup
    ![Image](img-1e0eb56e-image.png)
    
    In the page to **Add a new schema registry **type in the name of the *Storage Account**** ***for the **Schema registry name** field, and in the **Schema registry namespace **field, type the namespace we will use, for example `iotops`
    
    ![Image](img-1e0eb56e-image.png)
    
    Now, click the link to **Select Azure Storage Container **link, and you will magically be naavigated to the storage account we named in the form which we created earlier, and can then locate the conainer we created. Once we have selected the container, which we called `schemas` we can save, and this will update the form again.
    
    
    ![Image](img-1e0eb56e-image.png)
    
    1. Finally, we must select either the Test settings or the Secure settings deployment option. If you aren't sure which is right for your scenario, then default to Secure Settings. 
    This is what I am going to do in this environment also.
    
      ![Image](img-1e0eb56e-image.png)
    
    We will select the **Subscription which we are using for the IoT Operations deployment.** Then sequentially, we will select the Keyvault we prepared to be used for the secrets needed for the secure configuration `iotops`.
    Then for the **User assigned managed identity for secrets**, we will select the `itops-secrets`, and for the **User assigned managed identity for Azure IoT Operations components**, we will select the identity `iotops-components`.
    
    
The final page in the wizard, provides us with all the CLI commands we need to execute to actually deploy IoT Operations.

# Deploying IoT Operations

Now, for the fun part, we will execute each of the commands in the wizard in sequence in a terminal:

1. Sign in to Azure CLI interactively with a browser even if you already signed in before using az login. If you don't sign in interactively, you might get an error that says Your device is required to be managed to access your resource when you continue to the next step to deploy Azure IoT Operations. Install the latest Azure IoT Operations CLI extension.
  ```bash
  az upgrade
  az extension add --upgrade --name azure-iot-ops
  ```
  
  ### Prepare the cluster for Azure IoT Operations deployment. 

We will run the provided [az iot ops init](https://learn.microsoft.com/en-us/cli/azure/iot/ops#az-iot-ops-init) command.


> [!tip] 💡 
> The init command only needs to be run once per cluster. If you're reusing a cluster that already had Azure IoT Operations version 0.8.0 deployed on it, you can skip this step.
```bash
CLUSTER_NAME="p-we1iot-n013edge"
RESOURCE_GROUP="p-we1iot"
LOCATION="westeurope"

az iot ops init  \
  --resource-group "$RESOURCE_GROUP" \
  --cluster "$CLUSTER_NAME"
  

Azure IoT Operations                                                                                                                                                                        
Workflow correlation Id: 35f310b8-c0d5-4660-8ae6-0abb5bd429bf                                                                                                                               
                                                                                                                                                                                            
   Pre-Flight                                                                                                                                                                            
     ✔ Ensure registered resource providers                                                                                                                                              
     ✔ Enumerate pre-flight checks                                                                                                                                                       
-> Enablement                                                                                                                                                                            
     ✔ What-If evaluation                                                                                                                                                                
     * Install foundation layer                                                                                                                                                          
       • certManager: 0.6.2 stable                                                                                                                                                       
       • secretStore: 1.0.2 stable                                                                                                                                                       
                                                                                                                                                                                            
⠏ Done. ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   Elapsed: 0:07:57                                                                                                                         
                                                                                                                                                                                            
p-we1iot-n013edge
└── extensions
    ├── azure-secret-store
    └── cert-manager
```

This command might take several minutes to complete. You can watch the progress in the deployment progress display in the terminal.

### Deploy Azure IoT Operations.

Our cluster is enabled for observability, therefore we are going to add the following optional parameters to the `create` command:

  This command might take several minutes to complete. You can watch the progress in the deployment progress display in the terminal.

```bash
# Deploy IOT Operations  
az iot ops create \
  --name "$CLUSTER_NAME" \
  --cluster "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --sr-resource-id "$SCHEMA_REGISTRY_ID" \
  --ns-resource-id "$NAMESPACE_ID" \
  --broker-mem-profile tiny \
  --broker-frontend-replicas 1 \
  --broker-frontend-workers 1 \
  --broker-backend-part 1 \
  --broker-backend-workers 1 \
  --broker-backend-rf 2 \
  --ops-config secure \
  --add-insecure-listener true \
  --subscription 2ae5ef90-259a-4fd0-9b35-8611ffb26417 \
  --custom-location t-we1iot-n013edge-cl-1097 \
  --ops-config observability.metrics.openTelemetryCollectorAddress=aio-otel-collector.azure-iot-operations.svc.cluster.local:4317 \
  --ops-config observability.metrics.exportInternalSeconds=60
```

Once the command starts, we will be presented with a deployment progress view. 

This is going to be slow, 20-30 minutes to be expected

```bash
Azure IoT Operations                                                                                                                                                                     
Workflow correlation Id: cdfef77a-7923-4553-8215-5ccef5febe72                                                                                                                            
                                                                                                                                                                                         
   Pre-Flight                                                                                                                                                                            
     ✔ Ensure registered resource providers                                                                                                                                              
     ✔ Enumerate pre-flight checks                                                                                                                                                       
-> Deploy IoT Operations                                                                                                                                                                 
       v1.2.112 stable                                                                                                                                                                   
     * Create instance p-we1iot-n013edge                                                                                                                                                 
     - Apply default resources         
                              
    ⠸ Done. ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   Elapsed: 0:12:30
    t-we1iot-n013edge
    └── extensions
        ├── azure-arc-containerstorage
        ├── azure-iot-operations-platform
        └── azure-secret-store
        
                                                                                                                                                                                                                                                                                                             
p-we1iot-n013edge
├── extensions
│   ├── azure-iot-operations-iwy2n
│   ├── azure-secret-store
│   └── cert-manager
└── customLocations
    └── t-we1iot-n013edge-cl-1097
        └── resources
            └── p-we1iot-n013edge
 
```


### Enable secret sync for the deployed Azure IoT Operations instance. 

This command:

* Creates a federated identity credential using the user-assigned managed identity.
* Adds a role assignment to the user-assigned managed identity for access to the Azure Key Vault.
* Adds a minimum secret provider class associated with the Azure IoT Operations instance.
```shell
# IoT Ops needs access to the KeyVault
az iot ops secretsync enable  \
  --instance "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --kv-resource-id "$KEYVAULT_ID" \
  --mi-user-assigned "$SECRETS_MI_RESOURCE_ID"
  
{
  "extendedLocation": {
    "name": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.ExtendedLocation/customLocations/t-we1iot-n013edge-cl-1097",
    "type": "CustomLocation"
  },
  "id": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.SecretSyncController/azureKeyVaultSecretProviderClasses/spc-ops-4f74e85",
  "location": "westeurope",
  "name": "spc-ops-4f74e85",
  "properties": {
    "clientId": "61a254cf-6abb-4860-8d78-e0053c782d46",
    "keyvaultName": "p-we1iot-kv",
    "provisioningState": "Succeeded",
    "tenantId": "7d918dfe-bd63-478b-b8b3-2f252b011527"
  },
  "resourceGroup": "p-we1iot",
  "systemData": {
    "createdAt": "2025-11-20T14:53:51.4089914Z",
    "createdBy": "damian.flynn@innofactor.com",
    "createdByType": "User",
    "lastModifiedAt": "2025-11-20T14:53:55.8497189Z",
    "lastModifiedBy": "319f651f-7ddb-4fc6-9857-7aef9250bd05",
    "lastModifiedByType": "Application"
  },
  "type": "microsoft.secretsynccontroller/azurekeyvaultsecretproviderclasses"
}

```

### Assign a user-assigned managed identity to the deployed Azure IoT Operations instance.

This command creates a federated identity credential using the OIDC issuer of the indicated connected cluster and the Azure IoT Operations service account.

```shell
# Give the Azure IoT Ops Identity access
az iot ops identity assign \
  --name "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --mi-user-assigned "$IOTOPS_MI_RESOURCE_ID"
  
{
  "extendedLocation": {
    "name": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.ExtendedLocation/customLocations/t-we1iot-n013edge-cl-1097",
    "type": "CustomLocation"
  },
  "id": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.IoTOperations/instances/p-we1iot-n013edge",
  "identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourcegroups/p-we1iot/providers/Microsoft.ManagedIdentity/userAssignedIdentities/p-we1iot-n013edge-mi": {
        "clientId": "37a6d03f-6fd0-4dbd-9bdb-0f35bfa2b04e",
        "principalId": "d58d9e9b-0a90-4f6c-98c5-638c5b9abe5d"
      }
    }
  },
  "location": "westeurope",
  "name": "p-we1iot-n013edge",
  "properties": {
    "adrNamespaceRef": {
      "resourceId": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.DeviceRegistry/namespaces/p-we1iot"
    },
    "defaultSecretProviderClassRef": {
      "resourceId": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.SecretSyncController/azureKeyVaultSecretProviderClasses/spc-ops-4f74e85"
    },
    "provisioningState": "Succeeded",
    "schemaRegistryRef": {
      "resourceId": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.DeviceRegistry/schemaRegistries/p-we1iot-schema"
    },
    "version": "1.2.112"
  },
  "resourceGroup": "p-we1iot",
  "systemData": {
    "createdAt": "2025-11-20T14:41:12.7362948Z",
    "createdBy": "damian.flynn@innofactor.com",
    "createdByType": "User",
    "lastModifiedAt": "2025-11-20T19:01:52.7553508Z",
    "lastModifiedBy": "319f651f-7ddb-4fc6-9857-7aef9250bd05",
    "lastModifiedByType": "Application"
  },
  "type": "microsoft.iotoperations/instances"
}
```

>  To enable OPC UA Asset Discovery, utilize the 'az iot ops rsync enable' command to enable resource sync rules. Learn more 
## Verify deployment

We now have completed the deployment of Azure IoT Operations, we can now do a number of checks to ensure everything is ok

After the deployment is complete, use [az iot ops check](https://learn.microsoft.com/en-us/cli/azure/iot/ops#az-iot-ops-check) to evaluate IoT Operations service deployment for health, configuration, and usability. The *check* command can help you find problems in your deployment and configuration.

```bash
az iot ops check --svc broker --detail-level 2

This command is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus

──────────────────────────── Evaluation for {broker} service deployment ────────────────────────────

Post deployment checks ─────────────────────────────────────────────────────────────────────────────

    ✔ Enumerate MQTT Broker API resources                                                           
                                                                                                    
      ✔ mqttbroker.iotoperations.azure.com/v1 API resources                                         
            Broker                                                                                  
            BrokerAuthentication                                                                    
            BrokerListener                                                                          
            BrokerAuthorization                                                                     

    ✔ Evaluate MQTT Brokers                                                                         
                                                                                                    
      ✔ MQTT Brokers in namespace {azure-iot-operations}                                            
        - Expecting 1 broker resource per namespace. Detected 1.                                    
                                                                                                    
        - Broker {default}                                                                          
            Status:                                                                                 
                Provisioning Status {Succeeded}, Starting to provision Broker resources.            
                Health Status {Available}.                                                          
                                                                                                    
            Cardinality                                                                             
                - Expecting backend partitions >=1. Actual 1.                                       
                - Expecting backend redundancy factor >=1. Actual 2.                                
                - Expecting backend workers >=1. Actual 1.                                          
                - Expecting frontend replicas >=1. Actual 1.                                        
                                                                                                    
            Broker Diagnostics                                                                      
                logs:                                                                               
                    level: info                                                                     
                metrics:                                                                            
                    prometheusPort: 9600                                                            
                selfCheck:                                                                          
                    intervalSeconds: 30                                                             
                    mode: Enabled                                                                   
                    timeoutSeconds: 60                                                              
                traces:                                                                             
                    cacheSizeMegabytes: 16                                                          
                    mode: Enabled                                                                   
                    selfTracing:                                                                    
                        intervalSeconds: 30                                                         
                        mode: Enabled                                                               
                    spanChannelCapacity: 1000                                                       
                                                                                                    
            Diagnostics Service {aio-broker-diagnostics-service} detected.                          
                bincode-listener-service port 9700 protocol TCP                                     
                protobuf-listener-service port 9800 protocol TCP                                    
                aio-broker-metrics-service port 9600 protocol TCP                                   

                                                                                                    
        Runtime Health                                                                              
            ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓    
            ┃ Pod Name                         ┃ Phase   ┃ Conditions                          ┃    
            ┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩    
            │ aio-broker-diagnostics-probe-0   │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-frontend-0            │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-backend-1-0           │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-backend-1-1           │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-authentication-0      │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-health-manager-0      │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-diagnostics-service-0 │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            ├──────────────────────────────────┼─────────┼─────────────────────────────────────┤    
            │ aio-broker-operator-0            │ Running │ Pod Ready To Start Containers: True │    
            │                                  │         │ Pod Initialized: True               │    
            │                                  │         │ Pod Readiness: True                 │    
            │                                  │         │ Containers Readiness: True          │    
            │                                  │         │ Pod Scheduled: True                 │    
            │                                  │         │                                     │    
            └──────────────────────────────────┴─────────┴─────────────────────────────────────┘    

    ✔ Evaluate MQTT Broker Listeners                                                                
                                                                                                    
      ✔ Broker Listeners in namespace {azure-iot-operations}                                        
        - Expecting >=1 broker listeners per namespace. Detected 2.                                 
                                                                                                    
        - Broker Listener {default}. Broker reference {default} is Valid.                           
            Status:                                                                                 
                Provisioning Status {Succeeded}, BrokerListener created successfully.               
                Health Status {Unknown}.                                                            
            Port: 18883                                                                             
            Authentication reference: {default} is valid.                                           
            TLS:                                                                                    
              Cert Manager certificate spec:                                                        
                  issuerRef:                                                                        
                      group: cert-manager.io                                                        
                      kind: ClusterIssuer                                                           
                      name: azure-iot-operations-aio-certificate-issuer                             
                  privateKey:                                                                       
                      algorithm: Ec256                                                              
                      rotationPolicy: Always                                                        
                                                                                                    
        - Broker Listener {default-insecure}. Broker reference {default} is Valid.                  
            Status:                                                                                 
                Provisioning Status {Succeeded}, BrokerListener created successfully.               
                Health Status {Unknown}.                                                            
            Port: 1883                                                                              
                                                                                                    
      ✔ Service {aio-broker} of type ClusterIp                                                      
            Cluster IP: 10.43.25.4                                                                  
                                                                                                    
      ✔ Service {aio-broker-insecure} of type LoadBalancer                                          
            - Expecting >=1 ingress rule. 1.                                                        
                                                                                                    
            Ingress                                                                                 
                - ip: 10.13.100.11                                                                  

    ✔ Evaluate MQTT Broker Authentications                                                          
                                                                                                    
      ✔ Broker Authentications in namespace {azure-iot-operations}                                  
                                                                                                    
        - Broker Authentication {default}. Broker reference {default} is Valid.                     
            Status:                                                                                 
                Provisioning Status {Succeeded}, BrokerAuthentication created successfully.         
                Health Status {Unknown}.                                                            
            Expecting >=1 authentication methods. Detected 1.                                       
                - Service Account Token Method: {ServiceAccountToken} detected.                     
                    Audiences: ['aio-internal'] detected.                                           

    🔨 Evaluate MQTT Broker Authorizations                                                          
                                                                                                    
      🔨 Unable to fetch brokerauthorizations in any namespace.                                     


╭─────── Check Summary ───────╮
│ 25 check(s) succeeded.      │
│ 0 check(s) raised warnings. │
│ 0 check(s) raised errors.   │
│ 4 check(s) were skipped.    │
╰─────────────────────────────╯
```

Great, Now we can also ask the IoT Operations tools to run a check

```bash
# Lets check the cluster directly
# should all be 1/1 Running on a single node
kubectl -n azure-iot-operations get pods -l app.kubernetes.io/name=microsoft-iotoperations-mqttbroker -o wide
kubectl -n azure-iot-operations get sts | grep broker
kubectl -n azure-iot-operations get deploy | grep broker

# Get logs (backend store + frontend health):
# backend pods (two partitions; adjust names if different)
kubectl -n azure-iot-operations logs aio-broker-backend-1-0 -c broker
kubectl -n azure-iot-operations logs aio-broker-backend-1-1 -c broker

# frontend
kubectl -n azure-iot-operations logs aio-broker-frontend-0 -c broker

# PVCs must all be Bound
kubectl -n azure-iot-operations get pvc | grep broker

# Node resources (look for memory/disk pressure)
kubectl describe node n013edge | egrep -i 'memory pressure|disk pressure|Allocated resources|Non-terminated'
```


# Monitoring

## Deploy OpenTelemetery collection

1. Create a file named opentelemetry-values.yaml which we will use to configure the Helm Chart, and save this on your edge node.
  ```yaml
  mode: deployment
  fullnameOverride: aio-otel-collector
  image:
    repository: otel/opentelemetry-collector
    tag: 0.107.0
  
  config:
    receivers:
      otlp:
        protocols:
          grpc:
            endpoint: ":4317"
          http:
            endpoint: ":4318"
      jaeger: null
      prometheus: null
      zipkin: null
  
    processors:
      memory_limiter:
        limit_percentage: 80
        spike_limit_percentage: 10
        check_interval: 60s
  
    exporters:
      prometheus:
        endpoint: ":8889"
        resource_to_telemetry_conversion:
          enabled: true
        add_metric_suffixes: false
  
    extensions:
      health_check: {}
      memory_ballast:
        size_mib: 0
  
    service:
      extensions: [health_check]
      pipelines:
        metrics:
          receivers: [otlp]
          exporters: [prometheus]
        logs: null
        traces: null
      telemetry:
        metrics:
          address: "" # Disable legacy internal Prometheus endpoints
  
  resources:
    limits:
      cpu: '100m'
      memory: '512Mi'
  
  ports:
    metrics:
      enabled: true
      containerPort: 8888
      servicePort: 8888
      protocol: 'TCP'
    jaeger-compact:
      enabled: false
    jaeger-grpc:
      enabled: false
    jaeger-thrift:
      enabled: false
    zipkin:
      enabled: false
  ```
  
  1. Add the Open Telemetry Helm Chart to our cluster
  ```bash
  helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
  
  "open-telemetry" has been added to your repositories
  ```
  
  1. Check for Chart Update
  ```bash
  helm repo update
  
  Hang tight while we grab the latest from your chart repositories...
  ...Successfully got an update from the "open-telemetry" chart repository
  Update Complete. ⎈Happy Helming!⎈
  ```
  
  1. Finally, Deploy the chart, and reference our values file for the parameters to configure the chart.
  ```bash
  helm upgrade --install aio-observability open-telemetry/opentelemetry-collector -f otel-collector-values.yaml --namespace azure-iot-operations
  
  Release "aio-observability" does not exist. Installing it now.
  level=WARN msg="unable to find exact version; falling back to closest available version" chart=opentelemetry-collector requested="" selected=0.133.0
  NAME: aio-observability
  LAST DEPLOYED: Thu Nov 20 23:02:31 2025
  NAMESPACE: azure-iot-operations
  STATUS: deployed
  REVISION: 1
  DESCRIPTION: Install complete
  TEST SUITE: None
  NOTES:
  ```
  
  ### Check OpenTelemetry Collectors are online

To check that the OpenTelemetry metrics collection configuration worked correctly, follow these steps:

1. Check that the collector is deployed and working
  ```bash
  kubectl wait --for=condition=available --timeout=120s \
    deployment/aio-otel-collector \
    -n azure-iot-operations
    
  deployment.apps/aio-otel-collector condition met
  ```
  
  1. We can also check the pods, services and logs for sanity
  ```bash
  # Check the Pods are running
  kubectl get pods /
    -n azure-iot-operations /
    -l app.kubernetes.io/name=opentelemetry-collector
    
  NAME                                  READY   STATUS    RESTARTS   AGE
  aio-otel-collector-6779988db9-x5j7m   1/1     Running   0          9m47s
  
  # Then check that the service is active
  kubectl get svc /
    -n azure-iot-operations /
    -l app.kubernetes.io/name=opentelemetry-collector
    
  NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
  aio-otel-collector   ClusterIP   10.43.112.19   <none>        8888/TCP,4317/TCP,4318/TCP   9m59s
  
  # Finally, we can check for activity
  kubectl logs -n azure-iot-operations deployment/aio-otel-collector
  
  2025-11-20T23:02:42.640Z        info    service@v0.107.0/service.go:116 Setting up own telemetry...
  2025-11-20T23:02:42.640Z        info    service@v0.107.0/telemetry.go:96        Serving metrics {"address": "10.42.0.115:8888", "metrics level": "Normal"}
  2025-11-20T23:02:42.641Z        info    memorylimiter/memorylimiter.go:151      Using percentage memory limiter {"kind": "processor", "name": "memory_limiter", "pipeline": "metrics", "total_memory_mib": 512, "limit_percentage": 80, "spike_limit_percentage": 10}
  2025-11-20T23:02:42.641Z        info    memorylimiter/memorylimiter.go:75       Memory limiter configured       {"kind": "processor", "name": "memory_limiter", "pipeline": "metrics", "limit_mib": 409, "spike_limit_mib": 51, "check_interval": 60}
  2025-11-20T23:02:42.641Z        info    service@v0.107.0/service.go:195 Starting otelcol...     {"Version": "0.107.0", "NumCPU": 4}
  2025-11-20T23:02:42.641Z        info    extensions/extensions.go:36     Starting extensions...
  2025-11-20T23:02:42.641Z        info    extensions/extensions.go:39     Extension is starting...        {"kind": "extension", "name": "health_check"}
  2025-11-20T23:02:42.641Z        info    healthcheckextension@v0.107.0/healthcheckextension.go:32        Starting health_check extension     {"kind": "extension", "name": "health_check", "config": {"Endpoint":"10.42.0.115:13133","TLSSetting":null,"CORS":null,"Auth":null,"MaxRequestBodySize":0,"IncludeMetadata":false,"ResponseHeaders":null,"CompressionAlgorithms":null,"ReadTimeout":0,"ReadHeaderTimeout":0,"WriteTimeout":0,"IdleTimeout":0,"Path":"/","ResponseBody":null,"CheckCollectorPipeline":{"Enabled":false,"Interval":"5m","ExporterFailureThreshold":5}}}
  2025-11-20T23:02:42.641Z        info    extensions/extensions.go:56     Extension started.      {"kind": "extension", "name": "health_check"}
  2025-11-20T23:02:42.641Z        info    otlpreceiver@v0.107.0/otlp.go:102       Starting GRPC server    {"kind": "receiver", "name": "otlp", "data_type": "metrics", "endpoint": ":4317"}
  2025-11-20T23:02:42.641Z        info    otlpreceiver@v0.107.0/otlp.go:152       Starting HTTP server    {"kind": "receiver", "name": "otlp", "data_type": "metrics", "endpoint": ":4318"}
  2025-11-20T23:02:42.641Z        info    healthcheck/handler.go:132      Health Check state change       {"kind": "extension", "name": "health_check", "status": "ready"}
  2025-11-20T23:02:42.641Z        info    service@v0.107.0/service.go:221 Everything is ready. Begin running and processing data.
  2025-11-20T23:02:42.641Z        info    localhostgate/featuregate.go:63 The default endpoints for all servers in components have changed to use localhost instead of 0.0.0.0. Disable the feature gate to temporarily revert to the previous default.  {"feature gate ID": "component.UseLocalHostAsDefaultHost"}
  ```
  
  The collector is now receiving metrics on:

* OTLP gRPC: aio-otel-collector.azure-iot-operations.svc.cluster.local:4317
* OTLP HTTP: aio-otel-collector.azure-iot-operations.svc.cluster.local:4318
* Prometheus: aio-otel-collector.azure-iot-operations.svc.cluster.local:8889
## Configure Prometheus metrics collection

### Configure Prometheus metrics collection on your cluster.

1. Create a file named ama-metrics-prometheus-config.yaml and paste the following configuration:
  ```yaml
  apiVersion: v1
  data:
    prometheus-config: |2-
      scrape_configs:
        - job_name: otel
          scrape_interval: 1m
          static_configs:
            - targets:
              - aio-otel-collector.azure-iot-operations.svc.cluster.local:8889
        - job_name: aio-annotated-pod-metrics
          kubernetes_sd_configs:
            - role: pod
          relabel_configs:
            - action: drop
              regex: true
              source_labels:
                - __meta_kubernetes_pod_container_init
            - action: keep
              regex: true
              source_labels:
                - __meta_kubernetes_pod_annotation_prometheus_io_scrape
            - action: replace
              regex: ([^:]+)(?::\\d+)?;(\\d+)
              replacement: $1:$2
              source_labels:
                - __address__
                - __meta_kubernetes_pod_annotation_prometheus_io_port
              target_label: __address__
            - action: replace
              source_labels:
                - __meta_kubernetes_namespace
              target_label: kubernetes_namespace
            - action: keep
              regex: 'azure-iot-operations'
              source_labels:
                - kubernetes_namespace
          scrape_interval: 1m
  kind: ConfigMap
  metadata:
    name: ama-metrics-prometheus-config
    namespace: kube-system
  
  ```
  
  1. Apply the configuration file by running the following command:
  ```bash
  kubectl apply -f ama-metrics-prometheus-config.yaml
  
  ```
  
  
### Check Prometheus Collectors are online

To check that the Prometheus metrics collection configuration worked correctly, follow these steps:

1. Verify the ConfigMap was created:
  ```sql
  kubectl get configmap ama-metrics-prometheus-config -n kube-system
  
  configmap/ama-metrics-prometheus-config created
  ```
  
  1. Inspect the ConfigMap contents, Ensure the prometheus-config data matches your YAML:
  ```sql
  $ kubectl describe configmap ama-metrics-prometheus-config -n kube-system
  
  Name:         ama-metrics-prometheus-config
  Namespace:    kube-system
  Labels:       <none>
  Annotations:  <none>
  
  Data
  ====
  prometheus-config:
  ----
  scrape_configs:
    - job_name: otel
      scrape_interval: 1m
      static_configs:
        - targets:
          - aio-otel-collector.azure-iot-operations.svc.cluster.local:8889
    - job_name: aio-annotated-pod-metrics
      kubernetes_sd_configs:
        - role: pod
      relabel_configs:
        - action: drop
          regex: true
          source_labels:
            - __meta_kubernetes_pod_container_init
        - action: keep
          regex: true
          source_labels:
            - __meta_kubernetes_pod_annotation_prometheus_io_scrape
        - action: replace
          regex: ([^:]+)(?::\\d+)?;(\\d+)
          replacement: $1:$2
          source_labels:
            - __address__
            - __meta_kubernetes_pod_annotation_prometheus_io_port
          target_label: __address__
        - action: replace
          source_labels:
            - __meta_kubernetes_namespace
          target_label: kubernetes_namespace
        - action: keep
          regex: 'azure-iot-operations'
          source_labels:
            - kubernetes_namespace
      scrape_interval: 1m
  
  
  BinaryData
  ====
  
  Events:  <none>
  
  ```
  
  1. Check that the Prometheus agent (or Azure Monitor metrics addon) is running and using the config:
  ```sql
  $ kubectl get pods -n kube-system | grep ama-metrics
  
  ama-metrics-569df5f586-9qpzg                          2/2     Running     2 (7s ago)      4d23h
  ama-metrics-569df5f586-t5zhs                          2/2     Running     2 (66s ago)     4d23h
  ama-metrics-ksm-7fd5f48667-frw8q                      1/1     Running     0               4d23h
  ama-metrics-node-7qm6h                                2/2     Running     1 (4d23h ago)   4d23h
  ama-metrics-operator-targets-877cf7bf5-tnz8s          2/2     Running     2 (21s ago)     4d23h
  ```
  
  1. Look for pods like ama-metrics or similar and then check logs for errors:
  ```shell
  $ kubectl logs ama-metrics-569df5f586-9qpzg -n kube-system
  
  Defaulted container "prometheus-collector" out of: prometheus-collector, arc-msi-adapter
  2025/11/20 21:11:08 Starting inotify for watching config map update
  2025/11/20 21:11:08 Starting inotify for watching config map update
  MICROSOFT SOFTWARE LICENSE TERMS
  
  MICROSOFT Azure Arc-enabled Kubernetes
  
  This software is licensed to you as part of your or your company's subscription license for Microsoft Azure Services. You may only use the software with Microsoft Azure Services and subject to the terms and conditions of the agreement under which you obtained Microsoft Azure Services. If you do not have an active subscription license for Microsoft Azure Services, you may not use the software. Microsoft Azure Legal Information: https://azure.microsoft.com/en-us/support/legal/
  MODE=advanced
  CONTROLLER_TYPE=ReplicaSet
  CLUSTER=/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.Kubernetes/ConnectedClusters/p-we1iot-n013edge
  2025/11/20 21:11:08 Setting telemetry output to the default azurepubliccloud instance
  cp: omitting directory '/anchors/proxy/..2025_11_20_11_27_23.2344971365'
  2025/11/20 21:11:08 Warning copying /anchors/proxy/..2025_11_20_11_27_23.2344971365: exit status 1
  cp: omitting directory '/anchors/proxy/..data'
  2025/11/20 21:11:08 Warning copying /anchors/proxy/..data: exit status 1
  http_proxy=
  HTTP_PROXY=
  https_proxy=
  HTTPS_PROXY=
  NO_PROXY=,ama-metrics-operator-targets.kube-system.svc.cluster.local
  no_proxy=,ama-metrics-operator-targets.kube-system.svc.cluster.local
  HTTP_PROXY_ENABLED=false
  AZMON_AGENT_CFG_FILE_VERSION=ver1
  AZMON_AGENT_CFG_SCHEMA_VERSION=v1
  CONFIGMAP_VERSION=not_present
  ***********************Start Processing - parseSettingsForPodAnnotations***********************
  ***********************Start Processing - parsePrometheusCollectorConfig***********************
  2025/11/20 21:11:09 Pod annotation namespace regex configuration not found
  2025/11/20 21:11:09 pod annotation namespace regex configuration not found
  2025/11/20 21:11:09 Configure:Print the value of AZMON_AGENT_CFG_SCHEMA_VERSION: v1
  2025/11/20 21:11:09 AZMON_CLUSTER_ALIAS: ''
  2025/11/20 21:11:09 AZMON_CLUSTER_LABEL: p-we1iot-n013edge
  2025/11/20 21:11:09 Start prometheus-collector-settings Processing
  2025/11/20 21:11:09 Using configmap for default scrape settings...
  2025/11/20 21:11:09 config::Using scrape settings for kubelet: true
  2025/11/20 21:11:09 config::Using scrape settings for coredns: false
  2025/11/20 21:11:09 config::Using scrape settings for cadvisor: true
  2025/11/20 21:11:09 config::Using scrape settings for kubeproxy: false
  2025/11/20 21:11:09 config::Using scrape settings for apiserver: false
  2025/11/20 21:11:09 config::Using scrape settings for kubestate: true
  2025/11/20 21:11:09 config::Using scrape settings for nodeexporter: true
  ***********************End Processing - parsePrometheusCollectorConfig***********************
  ***********************Start Processing - parseDefaultScrapeSettings***********************
  2025/11/20 21:11:09 config::Using scrape settings for prometheuscollectorhealth: false
  2025/11/20 21:11:09 config::Using scrape settings for windowsexporter: false
  2025/11/20 21:11:09 config::Using scrape settings for windowskubeproxy: false
  2025/11/20 21:11:09 config::Using scrape settings for kappiebasic: true
  2025/11/20 21:11:09 config::Using scrape settings for networkobservabilityRetina: true
  2025/11/20 21:11:09 config::Using scrape settings for networkobservabilityHubble: true
  2025/11/20 21:11:09 config::Using scrape settings for networkobservabilityCilium: true
  2025/11/20 21:11:09 config:: Using scrape settings for acstor-capacity-provisioner: true
  2025/11/20 21:11:09 config:: Using scrape settings for acstor-metrics-exporter: true
  2025/11/20 21:11:09 config:: Using scrape settings for local-csi-driver: true
  2025/11/20 21:11:09 After replacing non-alpha-numeric characters with '_': p_we1iot_n013edge
  2025/11/20 21:11:09 End prometheus-collector-settings Processing
  config:: Using scrape settings for ztunnel: false
  config:: Using scrape settings for istio-cni: false
  config:: Using scrape settings for waypoint-proxy: false
  ***********************End Processing - parseDefaultScrapeSettings***********************
  ***********************Start Processing - parseDebugModeSettings***********************
  2025/11/20 21:11:09 The 'debug-mode' section is not present in the parsed data. Using default value: false
  2025/11/20 21:11:09 Unsupported/missing config schema version - 'v1', using defaults, please use supported schema version
  2025/11/20 21:11:09 Setting debug mode environment variable: false
  ***********************End Processing - parseDebugModeSettings***********************
  ***********************Start Processing - parseOpentelemetryMetricsSettings***********************
  2025/11/20 21:11:09 Setting AZMON_FULL_OTLP_ENABLED environment variable: false
  2025/11/20 21:11:09 ksm-config section NOT found in metricsConfigBySection
  ***********************End Processing - parseOpentelemetryMetricsSettings***********************
  ***********************Start Processing - parseKSMSettings***********************
  ***********************Start Processing - tomlparserTargetsMetricsKeepList***********************
  MINIMAL_INGESTION_PROFILE=true
  ***********************End Processing - tomlparserTargetsMetricsKeepList***********************
  ***********************Start Processing - tomlparserScrapeInterval***********************
  ***********************End Processing - tomlparserScrapeInterval***********************
  ***********************Start Processing - prometheusConfigMerger***********************
  2025/11/20 21:11:09 Updating scrape interval config for kubestateDefault.yml
  2025/11/20 21:11:09 scrapeInterval 30s
  2025/11/20 21:11:09 Starting to append keep list regex or minimal ingestion regex to kubestateDefault.yml
  2025/11/20 21:11:09 path acstorCapacityProvisionerDefaultFile: acstorCapacityProvisionerDefaultFile.yml
  2025/11/20 21:11:09 Updating scrape interval config for acstorCapacityProvisionerDefaultFile.yml
  2025/11/20 21:11:09 scrapeInterval 30s
  2025/11/20 21:11:09 Starting to append keep list regex or minimal ingestion regex to acstorCapacityProvisionerDefaultFile.yml
  2025/11/20 21:11:09 path acstorMetricsExporterDefaultFile: acstorMetricsExporterDefaultFile.yml
  2025/11/20 21:11:09 Updating scrape interval config for acstorMetricsExporterDefaultFile.yml
  2025/11/20 21:11:09 scrapeInterval 30s
  2025/11/20 21:11:09 Starting to append keep list regex or minimal ingestion regex to acstorMetricsExporterDefaultFile.yml
  2025/11/20 21:11:09 path LocalCSIDriverDefaultFile: localCSIDriverDefaultFile.yml
  2025/11/20 21:11:09 Updating scrape interval config for localCSIDriverDefaultFile.yml
  2025/11/20 21:11:09 scrapeInterval 30s
  2025/11/20 21:11:09 Starting to append keep list regex or minimal ingestion regex to localCSIDriverDefaultFile.yml
  2025/11/20 21:11:09 Done merging 4 default prometheus config(s)
  2025/11/20 21:11:09 Starting to merge default prometheus config values in collector template as backup
  Successfully set label limits in custom scrape config for job otel=
  Successfully set label limits in custom scrape config for job aio-annotated-pod-metrics=
  Done setting label limits for custom scrape config ...
  Merging default and custom scrape configs
  Done merging default scrape config(s) with custom prometheus config, writing them to file
  ***********************End Processing - prometheusConfigMerger, Done Merging Default and Custom Prometheus Config***********************
  AZMON_INVALID_CUSTOM_PROMETHEUS_CONFIG=false
  CONFIG_VALIDATOR_RUNNING_IN_AGENT=true
  prom-config-validator::Config file provided - /opt/promMergedConfig.yml
  prom-config-validator::Successfully generated otel config
  prom-config-validator::Loading configuration...
  prom-config-validator::Successfully loaded and validated prometheus config
  AZMON_SET_GLOBAL_SETTINGS=true
  2025/11/20 21:11:09 prom-config-validator::Use default prometheus config: 
  2025/11/20 21:11:09 meConfigFile: /usr/sbin/me.config
  2025/11/20 21:11:09 fluentBitConfigFile: /opt/fluent-bit/fluent-bit.yaml
  2025/11/20 21:11:09 checking health of token adapter after 1 secs
  ME_CONFIG_FILE=/usr/sbin/me.config
  2025/11/20 21:11:09 found token adapter to be healthy after 1 secs
  2025/11/20 21:11:09 export tokenadapterHealthyAfterSecs=1
  customResourceId=/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.Kubernetes/ConnectedClusters/p-we1iot-n013edge
  customRegion=westeurope
  2025/11/20 21:11:09 Waiting for 10s for token adapter sidecar to be up and running so that it can start serving IMDS requests
  ```
  
  1. (Optional) Query Prometheus for targets: If you have access to the Prometheus UI, go to the "Targets" page and verify that the jobs otel and aio-annotated-pod-metrics appear and are "UP".
If all these checks pass, your configuration is working. Let me know if you want to run any of these commands directly.



## Deploy dashboards to Grafana

Azure IoT Operations provides a [sample dashboard](https://github.com/Azure/azure-iot-operations/tree/main/samples/grafana-dashboard) designed to give you many of the visualizations you need to understand the health and performance of your Azure IoT Operations deployment.

Complete the following steps to install the Azure IoT Operations curated Grafana dashboards.

1. Clone or download the azure-iot-operations repository to get the sample Grafana Dashboard json file locally: https://github.com/Azure/azure-iot-operations.
1. Sign in to the Grafana console. You can access the console through the Azure portal or use the az grafana show command to retrieve the URL.
  ```bash
  az grafana show --name <GRAFANA_NAME> --resource-group <RESOURCE_GROUP> --query url -o tsv
  ```
  
  1. In the Grafana application, select the + icon.
  ![Image](img-1e0eb56e-image.png)
  
  1. Select Import dashboard.
1. Browse to the sample dashboard directory in your local copy of the Azure IoT Operations repository, azure-iot-operations > samples > grafana-dashboard, then select the aio.sample.json dashboard file.
  ![Image](img-1e0eb56e-image.png)
  
  1. When the application prompts, select your managed Prometheus data source.
  ![Image](img-1e0eb56e-image.png)
  
  1. Select Import.
  ![Image](img-1e0eb56e-image.png)
  
  

# IoT Operations Experience Portal

Go to the IoT Operations Experience Portal is located at **https://iotoperations.azure.com**

![Image](img-1e0eb56e-image.png)

## Operations Sites

![Image](img-1e0eb56e-image.png)

![Image](img-1e0eb56e-image.png)


