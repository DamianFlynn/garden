---
title: "2. Arc-Enabled Kubernetes at the Edge: Deploying K3s for Azure IoT Operations"
date: "2026-01-08"
lastmod: "2026-01-06T22:41:00.000Z"
draft: true
series: "Azure IoT Operations"
Tags:
  [
    "Azure",
    "IoT",
    "DevOps"
  ]
Status: "Ready"
summary: "Unlock the power of edge computing with our comprehensive guide on deploying K3s for Azure IoT Operations, where you'll master Kubernetes orchestration, enhance your infrastructure with Azure Arc, and ensure production readiness for your IoT applications—all while optimizing resource usage and simplifying management."
Categories: "IoT"
NOTION_METADATA:
  {
    "object": "page",
    "id": "728ba96c-7e67-44e1-9c52-94e44fe3ae81",
    "created_time": "2025-11-19T19:50:00.000Z",
    "last_edited_time": "2026-01-06T22:41:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": null,
    "icon": {
      "type": "emoji",
      "emoji": "2️⃣"
    },
    "parent": {
      "type": "data_source_id",
      "data_source_id": "0cb08ce3-4a92-421c-8f01-a3d2151ce62e",
      "database_id": "f9f77549-5ec7-48f1-a25a-aaeed54650ab"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/2-Arc-Enabled-Kubernetes-at-the-Edge-Deploying-K3s-for-Azure-IoT-Operations-728ba96c7e6744e19c5294e44fe3ae81",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T22:45:33.568Z"
last_edited_time: "2026-01-06T22:41:00.000Z"
EXPIRY_TIME: "2026-01-06T23:45:29.812Z"
---

With your edge server now Arc-enabled and monitored through Azure, the next step is deploying Kubernetes to host Azure IoT Operations workloads. This guide walks through K3s installation, Arc connection, and comprehensive observability setup—transforming your Ubuntu server into a production-ready edge compute platform.

If you completed [Part 1: Azure Arc-Enabled Infrastructure](https://www.notion.so/439b8c4a958e4f6b9c159335ba96c10b), you have secure remote access, centralized monitoring, and update management. Now we'll add Kubernetes orchestration capabilities while maintaining that Azure integration.

**What you'll learn:**

* K3s installation and user configuration
* Arc-enabling your Kubernetes cluster
* Azure Monitor and Grafana integration for K8s
* Service account and RBAC setup for Azure Portal
* Remote cluster management via Azure Arc proxy
* Production readiness validation
## Why K3s for IoT Edge?

Kubernetes at the edge enables container orchestration, declarative deployments, and GitOps workflows. But full Kubernetes is heavyweight for edge scenarios—requiring significant CPU, memory, and operational complexity.

**K3s solves this by:**

* Minimal footprint: <512MB RAM, perfect for resource-constrained edge
* Single binary: Simplified installation and updates
* Opinionated defaults: Includes Traefik ingress, local storage provisioner, and CoreDNS
* Production-grade: CNCF-certified Kubernetes, runs same workloads as full K8s
* Edge-optimized: Designed for ARM and x86_64, tested on Raspberry Pi to bare metal
**Azure IoT Operations requirements:**

* Kubernetes 1.27+ (K3s provides this)
* Workload identity support (we'll enable via Arc)
* Persistent storage (K3s includes local-path provisioner)
* Observability integration (we'll configure Azure Monitor)
K3s is the foundation—Azure Arc adds hybrid management, and Azure IoT Operations provides the data plane for edge scenarios.

```mermaid
graph TB
    subgraph "Edge Infrastructure Stack"
        Ubuntu[Ubuntu Server 24.04<br/>Arc-Enabled]
        K3s[K3s Kubernetes<br/>Arc-Connected]
        AIO[Azure IoT Operations<br/>Future Post]
        Apps[Applications<br/>Node-RED, Connectors]
    end
    
    subgraph "Azure Control Plane"
        ArcServers[Arc Machines<br/>Server Management]
        ArcK8s[Arc Kubernetes<br/>Cluster Management]
        Monitor[Azure Monitor<br/>Unified Observability]
        Grafana[Azure Grafana<br/>Dashboards]
    end
    
    Ubuntu --> K3s
    K3s --> AIO
    AIO --> Apps
    
    Ubuntu -.->|Part 1| ArcServers
    K3s -.->|"Part 2 (This Post)"| ArcK8s
    AIO -.->|Part 3| ArcK8s
    Apps -.->|Part 4| ArcK8s
    
    ArcServers --> Monitor
    ArcK8s --> Monitor
    Monitor --> Grafana
    
    style K3s fill:#326CE5,color:#fff
    style ArcK8s fill:#0078D4,color:#fff
    style Monitor fill:#50E6FF,color:#000
```

## Prerequisites

Before proceeding, ensure you've completed **Part 1: Azure Arc-Enabled Infrastructure**:

✅ Ubuntu Server 24.04 LTS installed and Arc-enabled

✅ Azure Arc agent connected and reporting heartbeat

✅ SSH access configured (EntraID or local user)

✅ Azure Monitor Agent installed and collecting metrics

✅ Resource group created (e.g., `iot-ops`)

**Hardware Recommendations:**

For Azure IoT Operations, review capacity requirements from Part 1. Minimum:

* CPU: 4 cores
* Memory: 8GB RAM (K3s uses ~512MB, leaving room for workloads)
* Storage: 40GB+ with >20% free space
**My Environment:**

* Server: otedge001 (KVM VM with 4GB RAM, 2 cores)
* OS: Ubuntu 24.04.03 LTS
* Region: North Europe
* Resource Group: iot-ops
## K3s Installation

K3s provides a convenient installation script that configures Kubernetes as a systemd service. The default installation includes everything needed for Azure IoT Operations.

### Step 1: Run the Installation Script

SSH to your Arc-enabled server and execute the installer. The script detects your OS, downloads the appropriate K3s binary, and configures systemd services.

```bash
# Execute K3s installation script
sysadmin@otedge001:~$ curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644

[INFO]  Finding release for channel stable
[INFO]  Using v1.33.5+k3s1 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.33.5+k3s1/sha256sum-amd64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.33.5+k3s1/k3s
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Skipping installation of SELinux RPM
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, command exists in PATH at /usr/bin/ctr
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
```

**What Just Happened:**

1. Binary Installation: K3s binary installed to /usr/local/bin/k3s
1. Symlinks Created: kubectl, crictl, ctr point to K3s for compatibility
1. Systemd Service: K3s configured to start automatically on boot
1. Kubeconfig Generated: Written to /etc/rancher/k3s/k3s.yaml (root-only)
1. Cluster Started: K3s server node is now running
**Verify Installation:**

```bash
# Check K3s service status
sudo systemctl status k3s

# Verify cluster is accessible (as root)
sudo kubectl get nodes

# Check the cluster is ready
kubectl wait --for=condition=Ready node/n013edge --timeout=300s

node/n013edge condition met
```

### Step 2: Configure Non-Root Access

By default, K3s kubeconfig is only accessible by root. For operational convenience and Azure Arc integration, configure user-level access.

**Why This Matters:**

* Azure Arc connection requires kubectl access
* CI/CD pipelines run as non-root users
* GitOps tools (Flux, ArgoCD) need kubeconfig access
* Avoids security risks of running operations as root
**Create User Kubeconfig:**

```bash
# Setup user kubeconfig (using local IP 10.13.100.11 which is in the certificate)
mkdir -p ~/.kube
sudo cat /etc/rancher/k3s/k3s.yaml | sed "s/127.0.0.1/10.13.100.11/g" > ~/.kube/config

# Set secure permissions (only your user can read/write)
chmod 0600 ~/.kube/config

# Configure environment to use local config
export KUBECONFIG=~/.kube/config
echo 'export KUBECONFIG=~/.kube/config' >> ~/.bashrc

# Switch to K3s context
kubectl config use-context default

# CRITICAL: Set kubeconfig permissions for IoT Operations
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```

**Verify User Access:**

```bash
# Verify K3s is running
kubectl get nodes

NAME       STATUS   ROLES                  AGE    VERSION
n013edge   Ready    control-plane,master   5m3s   v1.33.5+k3s1

kubectl wait --for=condition=Ready node/n013edge --timeout=300s

node/n013edge condition met

# Check K3s version
kubectl version 

Client Version: v1.33.5+k3s1
Kustomize Version: v5.6.0
Server Version: v1.33.5+k3s1

kubectl version --client

Client Version: v1.31.5+k3s1
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3

sysadmin@otedge001:~$ kubectl get nodes
NAME        STATUS   ROLES                  AGE    VERSION
otedge001   Ready    control-plane,master   8m4s   v1.31.5+k3s1
```

**Expected Output:**

* Client version matches K3s installation
* Single node shows Ready status
* Roles include control-plane and master
### Step 3: Performance Optimization

For production edge deployments with multiple pods and file watchers (GitOps, monitoring), increase system limits to prevent resource exhaustion.

**Increase inotify Limits:**

Linux uses inotify for file system event monitoring. Kubernetes components (kubelet, container runtime) and applications (Flux, Prometheus) create many watches. Default limits cause "too many open files" errors under load.

```bash
# Increase user watch/instance limits
echo fs.inotify.max_user_instances=8192 | sudo tee -a /etc/sysctl.conf
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf

# Apply changes immediately
sudo sysctl -p
```

**Expected Output:**

```javascript
fs.inotify.max_user_instances = 8192
fs.inotify.max_user_watches = 524288
```

**Increase File Descriptor Limit:**

Each connection, socket, and file requires a file descriptor. Edge workloads (MQTT brokers, time-series databases) maintain many concurrent connections.

```bash
# Increase system-wide file descriptor limit
echo fs.file-max = 100000 | sudo tee -a /etc/sysctl.conf

# Apply changes
sudo sysctl -p
```

**Verify Applied Settings:**

```bash
sysadmin@otedge001:~$ sysctl fs.inotify.max_user_instances fs.inotify.max_user_watches fs.file-max
fs.inotify.max_user_instances = 8192
fs.inotify.max_user_watches = 524288
fs.file-max = 100000
```

**When to Adjust:**

* Default limits work for small deployments (<10 pods)
* Increase for GitOps (Flux watches Git repos)
* Increase for time-series databases (Prometheus, InfluxDB)
* Monitor with: cat /proc/sys/fs/inotify/max_user_watches
## Arc-Enable the K3s Cluster

With K3s running, the next step is connecting it to Azure Arc. This enables cluster management from Azure Portal, GitOps deployments, and Azure Monitor integration.

### Step 1: Install Azure CLI

The `az` CLI tool manages Azure resources from the command line. We'll use it to register the K3s cluster with Arc.

**Install Prerequisites:**

```bash
# Update package index
sudo apt update

# Install required packages
sudo apt install ca-certificates curl apt-transport-https lsb-release gnupg -y
```

**Add Microsoft Repository:**

```bash
# Import Microsoft signing key
curl -sL https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/microsoft.gpg

# Add Azure CLI repository
AZ_REPO=$(lsb_release -cs)
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
```

**Install Azure CLI:**

```bash
# Update package index with new repository
sudo apt update

# Install Azure CLI
sudo apt install azure-cli -y

# Verify installation
az version
```

**Expected Output:**

```json
{
  "azure-cli": "2.78.0",
  "azure-cli-core": "2.78.0",
  "azure-cli-telemetry": "1.1.0",
  "extensions": {}
}
```

### Step 2: Authenticate to Azure

Use device code authentication to sign in from the edge server.

```bash
az login
```

**Follow the authentication flow:**

1. Browser opens to https://microsoft.com/devicelogin
1. Enter the provided device code
1. Complete EntraID authentication (including MFA if enabled)
1. CLI confirms successful authentication
### Step 3: Register Resource Providers

Azure Arc for Kubernetes requires specific resource providers enabled in your subscription.

```bash
# Register Arc Kubernetes provider
az provider register --namespace 'Microsoft.Kubernetes'

# Register Kubernetes configuration provider
az provider register --namespace 'Microsoft.KubernetesConfiguration'

# Register extended location provider (for custom locations)
az provider register --namespace "Microsoft.ExtendedLocation"

# Register secret sync controller
az provider register --namespace "Microsoft.SecretSyncController"
```

**Verify Registration:**

```bash
# Check provider status
az provider show --namespace Microsoft.Kubernetes --query "registrationState"
```

Expected: `"Registered"` (may take 2-3 minutes)

### Step 4: Install Arc CLI Extensions

Azure CLI extensions add functionality for Arc and custom locations.

```bash
# Install Arc-enabled Kubernetes extension
az extension add --name connectedk8s --upgrade -y

# Install Kubernetes extensions management
az extension add --name k8s-extension --upgrade -y

# Install custom locations extension
az extension add --name customlocation --upgrade -y
```

**Expected Output:**

Each extension installs with version information and no errors.

### Step 5: Connect K3s to Azure Arc

This command registers your K3s cluster as an Arc-enabled resource, deploys Arc agents to the cluster, and establishes connectivity.

**Command Explained:**

* --name: Cluster name in Azure (must be unique in resource group)
* --location: Azure region (should match Arc server region)
* --resource-group: Existing resource group from Part 1
* --enable-oidc-issuer: Required for workload identity
* --enable-workload-identity: Required for Azure IoT Operations
* --disable-auto-upgrade: Prevents unplanned updates (upgrade manually)
```bash
CLUSTER_NAME="p-we1iot-n013edge"
RESOURCE_GROUP="p-we1iot"
LOCATION="westeurope"

# Connect K3s cluster to Azure Arc with OIDC and Workload Identity
az connectedk8s connect \
  --name "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --correlation-id "$(uuidgen)" \
  --enable-oidc-issuer \
  --enable-workload-identity \
  --disable-auto-upgrade \
  --tags 'Datacenter=NorthEurope,City=Ballina,StateOrDistrict=Mayo,CountryOrRegion=Ireland,Environment=Production,Project=OT,Building=013'

```

**What Happens During Connection:**

The process takes 20-30 minutes and performs these steps:

1. Validates Environment: Checks kubectl access, cluster connectivity
1. Checks Provider Registration: Verifies resource providers are registered
1. Runs Pre-Onboarding Checks: Validates cluster meets requirements
1. Generates Key Pair: Creates public/private keys for secure communication
1. Provisions Azure Resource: Creates Arc Kubernetes resource in Azure
1. Deploys Agents: Installs Arc components in azure-arc namespace
1. Establishes Connectivity: Opens outbound connection to Azure
**Installation Output (Abbreviated):**

```bash
Argument '--enable-oidc-issuer' is in preview and under development.
Argument '--enable-workload-identity' is in preview and under development.
This operation might take a while...

Step: Validating custom access token
Step: Checking Provider Registrations
Step: Setting KubeConfig
Step: Checking Connectivity to Cluster
Step: Do node validations
Step: Install Kubectl client if it does not exist
Step: Install Helm client if it does not exist
Step: Starting Pre-onboarding-check
Step: The required pre-checks for onboarding have succeeded.
Step: Generating Public-Private Key pair
Step: Azure resource provisioning has begun.
Step: Azure resource provisioning has finished.
Step: Starting to install Azure arc agents on the Kubernetes cluster.
Step: Agent state has reached terminal state.
```

**Final Output:**

```json
{
  "agentPublicKeyCertificate": "MIICCgKCA...",
  "agentVersion": "1.31.2",
  "arcAgentProfile": {
    "agentAutoUpgrade": "Disabled",
    "agentErrors": [],
    "agentState": "Succeeded",
    "desiredAgentVersion": null,
    "systemComponents": null
  },
  "arcAgentryConfigurations": null,
  "azureHybridBenefit": "NotApplicable",
  "connectivityStatus": "Connected",
  "distribution": "k3s",
  "distributionVersion": null,
  "gateway": null,
  "id": "/subscriptions/2ae5ef90-259a-4fd0-9b35-8611ffb26417/resourceGroups/p-we1iot/providers/Microsoft.Kubernetes/connectedClusters/p-we1iot-n013edge",
  "identity": {
    "principalId": "18d27d6f-e5d9-4b7c-aeae-71b84128eb0a",
    "tenantId": "7d918dfe-bd63-478b-b8b3-2f252b011527",
    "type": "SystemAssigned"
  },
  "infrastructure": "K3s",
  "kind": null,
  "kubernetesVersion": "1.33.5+k3s1",
  "lastConnectivityTime": "2025-11-20T08:33:39.921000+00:00",
  "location": "westeurope",
  "managedIdentityCertificateExpirationTime": "2026-02-18T08:28:00+00:00",
  "miscellaneousProperties": null,
  "name": "p-we1iot-n013edge",
  "offering": null,
  "oidcIssuerProfile": {
    "enabled": true,
    "issuerUrl": "https://europe.oic.prod-arc.azure.com/7d918dfe-bd63-478b-b8b3-2f252b011527/ea7687d8-2a1e-4fa1-bcf3-67bf7500d122/",
    "selfHostedIssuerUrl": null
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "iot-ops",
  "securityProfile": {
    "workloadIdentity": {
      "enabled": true
    }
  },
  "totalCoreCount": 4,
  "totalNodeCount": 1,
  "type": "microsoft.kubernetes/connectedclusters"
}
```

**Key Fields:**

* connectivityStatus: Connected → Cluster is reporting to Azure
* oidcIssuerProfile.enabled: true → Workload identity configured
* securityProfile.workloadIdentity.enabled: true → Required for Azure IoT Operations
* kubernetesVersion → K3s version reported to Azure
### Step 6: Verify Arc Agents

Arc agents run as pods in the `azure-arc` namespace. These components maintain connectivity, sync configuration, and enable extensions.

```bash
# Verify Arc K8s connection
az connectedk8s show \
  --name "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --query "{name:name, status:connectivityStatus, oidc:oidcIssuerProfile.enabled, workloadId:securityProfile.workloadIdentity.enabled, k8sVersion:kubernetesVersion}" \
  -o table
  
Name               Status     Oidc    WorkloadId    K8sVersion
-----------------  ---------  ------  ------------  ------------
p-we1iot-n013edge  Connected  True    True          1.33.5+k3s1


# Check the ARC Agents are running on the Server
kubectl get pods -n azure-arc

NAME                                          READY   STATUS    RESTARTS   AGE
cluster-metadata-operator-7488dcd5df-v5tmd    2/2     Running   0          170m
clusterconnect-agent-6bbff7588-8rcpg          3/3     Running   0          170m
clusteridentityoperator-675565bc5d-wdvft      2/2     Running   0          170m
config-agent-c998c5974-hlcc2                  2/2     Running   0          170m
controller-manager-b69b96dc8-9b9lh            2/2     Running   0          170m
extension-events-collector-65696bb498-tvs4t   2/2     Running   0          170m
extension-manager-5f569db9bb-nm6m7            3/3     Running   0          170m
flux-logs-agent-6646778fcf-8rtn8              1/1     Running   0          170m
kube-aad-proxy-75cbbc86c7-g86gd               2/2     Running   0          170m
logcollector-c8676f558-97vk5                  1/1     Running   0          170m
metrics-agent-747857c755-xxqpm                2/2     Running   0          170m
resource-sync-agent-77b657df44-q8t9b          2/2     Running   0          170m
```

**Agent Roles:**

* clusterconnect-agent: Enables Arc proxy for kubectl access
* config-agent: GitOps configuration management
* extension-manager: Manages Arc extensions
* metrics-agent: Forwards metrics to Azure Monitor
* kube-aad-proxy: EntraID authentication integration
**All pods should show **`**Running**`** status with no restarts** (occasional restarts during initial startup are normal).

### Step 7: Verify in Azure Portal

Navigate to **Azure Portal → Resource Groups → p-we1iot**

You should see two Arc resources:

1. otedge001 (Type: Azure Arc Machine) - Your Ubuntu server
1. p-we1iot-n013edge (Type: Arc-enabled Kubernetes) - Your K3s cluster
**Cluster Overview Provides:**

* Kubernetes version and distribution (K3s)
* Node count and total cores
* Connectivity status and last heartbeat
* OIDC issuer URL (for workload identity)
* Installed extensions
## Enable Custom Locations

Custom Locations allow Azure services (like Azure IoT Operations) to install into your Arc-connected cluster. This feature maps Azure regions to Kubernetes namespaces.

### Step 1: Configure OIDC Issuer

Azure IoT Operations uses workload identity for authentication. The OIDC issuer URL must be added to the K3s API server configuration.

**Retrieve Issuer URL:**

```bash
# Get the OIDC issuer URL from your Arc cluster
az connectedk8s show \
  --resource-group "$RESOURCE_GROUP" \
  --name "$CLUSTER_NAME" \
  --query oidcIssuerProfile.issuerUrl \
  --output tsv
```

**Example Output:**

```javascript
https://europe.oic.prod-arc.azure.com/7d918dfe-bd63-478b-b8b3-2f252b011527/ea7687d8-2a1e-4fa1-bcf3-67bf7500d122/
```

**Add Issuer to K3s Configuration:**

Create a new K3s configuration file:

```bash
# Create configuration file (requires sudo)
sudo vi /etc/rancher/k3s/config.yaml
laboratory,kant,ARC
```

**Add the following content (replace with your issuer URL):**

```yaml
kube-apiserver-arg:
 - service-account-issuer=<YOUR_ISSUER_URL>
 - service-account-max-token-expiration=24h
```

**Restart K3s to Apply Configuration:**

```bash
sudo systemctl restart k3s

# Wait for cluster to stabilize (30-60 seconds)
sleep 60

# Verify cluster is healthy
kubectl get nodes
```

### Step 2: Get Custom Location Object ID

Custom Locations requires a specific service principal with a fixed GUID. This command retrieves its object ID.

```bash
# Retrieve and store the custom location service principal ID
export OBJECT_ID=$(az ad sp show --id bc313c14-388c-4e7d-a58e-70017303ee3b --query id -o tsv)

# Verify the ID was retrieved
echo $OBJECT_ID
```

**Expected Output:**

A GUID like `1234abcd-5678-ef90-1234-567890abcdef`

**Note:** This GUID is constant across all Azure tenants. If the command fails, ensure you're authenticated with a user account (not service principal).

### Step 3: Enable Custom Locations Feature

This command configures your Arc cluster to support custom locations and cluster connect features.

```bash
az connectedk8s enable-features \
  --resource-group "$RESOURCE_GROUP" \
  --name "$CLUSTER_NAME" \
  --custom-locations-oid $OBJECT_ID \
  --features cluster-connect custom-locations
```

**Expected Output:**

```javascript
This command is in preview and under development.
This operation might take a while...

Step: 2025-11-20T10-50-11Z: Validating custom access token
Step: 2025-11-20T10-50-13Z: Checking Custom Location(Microsoft.ExtendedLocation) RP Registration state for this Subscription, and attempt to get the Custom Location Object ID (OID),if registered
Step: 2025-11-20T10-50-14Z: Setting KubeConfig
Step: 2025-11-20T10-50-14Z: Checking Connectivity to Cluster
Step: 2025-11-20T10-50-14Z: Install Helm client if it does not exist
Step: 2025-11-20T10-50-14Z: Get namespace of release: azure-arc
Step: 2025-11-20T10-50-14Z: Getting HelmPackagePath from Arc DataPlane
Step: 2025-11-20T10-50-15Z: Determine Helmchart Export Path
Step: 2025-11-20T10-50-15Z: Pulling HelmChart: mcr.microsoft.com/azurearck8s/batch1/stable/v2/azure-arc-k8sagents, Version: 1.31.2

Important! Custom Location feature enablement can't be validated when using a manually provided OID. If the custom location feature is not enabled, you may encounter an error when creating the custom location.
After creating the custom location, run `az customlocation show` and check that ProvisioningState is Succeeded. If ProvisoningState is Failed, then re-try this command with a  valid custom location OID to enable the feature.
For guidance, refer to: https://aka.ms/enable-customlocation
"Successfully enabled features: ['cluster-connect', 'custom-locations'] for the Connected Cluster otedge001"
```

**What This Enables:**

* cluster-connect: Remote kubectl access via Arc proxy
* custom-locations: Azure services can target this cluster
**Verification:**

In Azure Portal, navigate to your Arc Kubernetes cluster → **Settings → Extensions + Applications**. You should see options to install Arc extensions.

## Configure Cluster Access from Azure Portal

Azure Portal can display Kubernetes resources (namespaces, pods, services) directly in the UI. This requires a service account token with appropriate permissions.

### Step 1: Create Service Account

Service accounts provide identity for processes running in pods—or in this case, for Azure Portal to authenticate.

```bash
# Create service account in default namespace
kubectl create serviceaccount arc-user -n default
```

**Why default namespace?**

The service account can be in any namespace, but `default` is convenient and doesn't require additional configuration.

### Step 2: Grant Cluster-Admin Permissions

For full visibility in Azure Portal, grant cluster-admin role. For production, use more restrictive roles based on the principle of least privilege.

```bash
# Bind cluster-admin role to service account
kubectl create clusterrolebinding arc-user-binding \
  --clusterrole cluster-admin \
  --serviceaccount default:arc-user
```

**Security Consideration:**

`cluster-admin` provides full cluster access. For production:

* Create custom roles with specific permissions
* Use namespace-scoped roles where possible
* Rotate tokens regularly
* Audit token usage via Azure Activity Log
### Step 3: Create Service Account Token

Kubernetes 1.24+ requires explicit token creation (no longer automatic).

```bash
# Create long-lived token secret
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: arc-user-secret
  namespace: default
  annotations:
    kubernetes.io/service-account.name: arc-user
type: kubernetes.io/service-account-token
EOF
```

**Verify Secret Creation:**

```bash
kubectl get secret arc-user-secret -n default
```

### Step 4: Retrieve and Save Token

The token is base64-encoded in the secret. Decode and display it.

```bash
# Extract and decode token
TOKEN=$(kubectl get secret arc-user-secret -n default -o jsonpath='{.data.token}' | base64 -d)

# Display token
echo $TOKEN
```

**Expected Output:**

A long JWT token starting with `eyJhbGci...` 

```bash
eyJhbGciOiJSUzI1NiIsImtpZCI6IldBSHpyNG9BNXhUaklMMlFxcGlUMWlhMGlVcjJJMmxhZHVFOHBJUjFvRDQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImFyYy11c2VyLXNlY3JldCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJhcmMtdXNlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImI4NTE4ODU0LWMxOTMtNGU1ZS1iODk1LWNmNmI4YjI5YWEwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmFyYy11c2VyIn0.dDS9bN09OX52NchzpKzWiGXMDf1L2XU3IpGvk_pyn6DjJdLN00pQ5pnouFZcKfYv4CNKzmmLuDlNCIPDklwxD9dGtCXnnMfrzyP5t0uDhApzfgz-AfkoupID-DVTukAu4qGXXYbJNCA8bIZDXJlhuZU2a1T1bWB1axxQHgo0gyVonUUz-UUXxvdzrvQD_nXdqKcap0bnz9ewmdvNdGC8QDOc7HKYnSSlmhJ_4qNZO_CagV81o91I56KGb8hzleYmEdpdn1NuK3dG4sW6RhKELasY1WboSydJZ53a_PQPE9MECIfj1sftyhRhihtMciJ48bNyTWbcGN7oarybhkq3mw
```

**⚠️ Security Warning:**

This token provides full cluster access. Handle it as a secret:

* Store in password manager or Azure Key Vault
* Don't commit to Git repositories
* Rotate regularly (recreate the secret)
* Monitor usage via Azure audit logs
### Step 5: Add Token to Azure Portal

1. Navigate to Azure Portal → Arc Kubernetes Cluster → Kubernetes Resources
1. Click Namespaces (or any Kubernetes resource view)
1. Portal prompts for authentication token
1. Paste the token from Step 4
1. Click Sign In
  ![Image](img-728ba96c-image.png)
  
  **After authentication**, you'll see all Kubernetes namespaces:

* azure-arc - Arc agent components
* kube-system - K3s system components
* default - Default application namespace
  ![Image](img-728ba96c-image.png)
  
  **Portal Capabilities:**

* View namespaces, pods, services, deployments
* Inspect pod logs and events
* View resource utilization
* Deploy quickstart applications
## Remote Cluster Management via Arc Proxy

Azure Arc enables `kubectl` access from any machine with Azure CLI, without VPN or direct network connectivity to the edge cluster.

### How Arc Proxy Works

```mermaid
sequenceDiagram
    participant Dev as Developer Laptop
    participant AzCLI as Azure CLI<br/>(az connectedk8s proxy)
    participant AzureArc as Azure Arc Service
    participant K3s as K3s Cluster<br/>(clusterconnect-agent)
    
    Dev->>AzCLI: Start proxy session
    AzCLI->>AzureArc: Authenticate & request tunnel
    AzureArc->>K3s: Establish WebSocket tunnel
    K3s-->>AzureArc: Tunnel ready
    AzureArc-->>AzCLI: Proxy listening on localhost:47011
    AzCLI-->>Dev: Kubeconfig updated, context set
    
    loop kubectl commands
        Dev->>AzCLI: kubectl get pods
        AzCLI->>AzureArc: Forward API request
        AzureArc->>K3s: Deliver to API server
        K3s-->>AzureArc: API response
        AzureArc-->>AzCLI: Return response
        AzCLI-->>Dev: Display results
    end
```

### Start Proxy Session

From your laptop (Windows, macOS, or Linux), connect to the Arc cluster:

```bash
# Start Arc proxy (replace with your token from earlier)
az connectedk8s proxy \
  --resource-group iot-ops \
  --name otedge001 \
  --token <YOUR_ARC_USER_TOKEN>
```

**Expected Output:**

```javascript
Proxy is listening on port 47011
Merged "otedge001" as current context in /home/user/.kube/config
Start sending kubectl requests on 'otedge001' context using kubeconfig at /home/user/.kube/config
Press Ctrl+C to close proxy.
```

**What Happened:**

1. CLI downloaded cluster kubeconfig via Arc
1. Merged it into your local ~/.kube/config
1. Started local proxy listening on random port
1. Set current context to otedge001
**In a new terminal**, use `kubectl` as if directly connected:

```bash
# View nodes
kubectl get nodes

# View all pods across all namespaces
kubectl get pods --all-namespaces

# Deploy applications
kubectl apply -f deployment.yaml

# View logs
kubectl logs <pod-name> -n <namespace>
```

**Proxy Session:**

* Remains active until you press Ctrl+C
* Automatically reauthenticates if token expires
* Uses Azure RBAC for authorization (token determines permissions)
**Use Cases:**

* Remote troubleshooting from any location
* CI/CD pipelines deploying to edge clusters
* GitOps controllers (Flux, ArgoCD) managing edge
* Multi-cluster management from single workstation
## Test Deployment: Azure Vote Application

Validate cluster functionality by deploying a sample application from Azure Portal.

### Deploy Quickstart Application

1. Navigate to Portal: Arc Kubernetes Cluster → Workloads
1. Click Add: Select "Add" button
1. Choose Quickstart: Click "Deploy a quickstart application"
1. Select Template: Choose "Create a basic web application"
1. Review Manifest: Portal shows the deployment YAML
1. Deploy: Click "Add" to create resources
**What Gets Deployed:**

* Namespace: azure-store
* Pods: RabbitMQ, order-service, product-service, store-front
* Services: ClusterIP services for internal communication
* Service (store-front): LoadBalancer type (will remain pending on K3s)
### Verify Deployment

```bash
# Check pods in azure-store namespace
kubectl get pods -n azure-store

NAME                               READY   STATUS    RESTARTS   AGE
order-service-65cc8855c-rmwrr      1/1     Running   0          68s
product-service-77ff9f6fd6-vp5p5   1/1     Running   0          68s
rabbitmq-5dcdf9484-9lhmf           1/1     Running   0          68s
store-front-698cc8c565-b4cxc       1/1     Running   0          68s
```

**All pods should show **`**Running**`** status.**

### Expose Store-Front Service

K3s doesn't include a LoadBalancer implementation, so the `store-front` service remains in `Pending` state. Change it to NodePort for access.

```bash
# Change service type to NodePort
kubectl patch svc store-front -n azure-store -p '{"spec":{"type":"NodePort"}}'

# Verify service
kubectl get svc store-front -n azure-store

NAME          TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
store-front   NodePort   10.43.132.215   <none>        80:30663/TCP   86m
```

**Access Application:**

The application is now accessible on the node's IP address at the NodePort (in this case, `30663`).

```bash
# From edge server
curl http://localhost:30663

# From your laptop (via Arc proxy SSH tunnel)
ssh -L 8080:localhost:30663 sysadmin@otedge001
# Then browse to http://localhost:8080
```

**Clean Up Test Application:**

```bash
# Delete the test namespace and all resources
kubectl delete namespace azure-store
```

## Azure Monitor Integration for Kubernetes

Extend the server-level monitoring from Part 1 to include Kubernetes-specific metrics and logs.

### Architecture Overview

>  The new integrated approach uses:
  * Azure Monitor managed service for Prometheus (replacing Azure Monitor Workspace)
  * Azure Managed Grafana with built-in Arc Kubernetes integration
  * Container Insights for logs and performance data
  ### Architecture (Updated 2025)
  
  K3s Cluster (n013edge)
  ├── Azure Monitor Agent (ama-metrics) → Managed Prometheus
  ├── Container Insights (ama-logs) → Log Analytics
  └── Kube-State-Metrics → Cluster state metrics
  
  Azure Services
  ├── Azure Monitor Workspace (Prometheus storage)
  ├── Log Analytics Workspace (logs storage)
  └── Azure Managed Grafana (visualization with Arc integration)
  
  ```mermaid
graph TB
    subgraph "K3s Cluster"
        Pods[Application Pods]
        AMA[Azure Monitor Agent<br/>DaemonSet]
        KSM[Kube-State-Metrics<br/>Deployment]
    end
    
    subgraph "Azure Services"
        AMW[Azure Monitor Workspace<br/>Prometheus Metrics]
        LA[Log Analytics Workspace<br/>Container Logs]
        Grafana[Azure Managed Grafana<br/>Dashboards]
    end
    
    Pods -->|Metrics| AMA
    KSM -->|Cluster Metrics| AMA
    AMA -->|Prometheus Format| AMW
    AMA -->|Container Logs| LA
    
    AMW --> Grafana
    LA --> Grafana
    
    style AMW fill:#50E6FF,color:#000
    style Grafana fill:#FFA500,color:#000
```

### Prerequisites

**Enable CLI Extensions:**

```bash
# Add Azure Managed Grafana extension
az extension add --name amg --upgrade -y
```

**Register Resource Providers:**

```bash
# Azure Monitor providers
az provider register --namespace Microsoft.AlertsManagement
az provider register --namespace Microsoft.Monitor
az provider register --namespace Microsoft.Dashboard
az provider register --namespace Microsoft.Insights
az provider register --namespace Microsoft.OperationalInsights
```

Registration takes 2-3 minutes. Verify with:

```bash
az provider show --namespace Microsoft.Monitor --query "registrationState"
```

### Step 1: Create Azure Monitor Workspace

Azure Monitor Workspace stores Prometheus metrics scraped from the cluster.

```bash
# Set variables
RESOURCE_GROUP="p-we1iot"
LOCATION="westeurope"
MONITOR_WORKSPACE="p-we1iot-monitor"
GRAFANA_NAME="p-we1iot-grafana"
LOG_ANALYTICS="p-we1iot-logs"

# Create Azure Monitor Workspace (for Prometheus metrics)
az monitor account create \
  --name "$MONITOR_WORKSPACE" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --tags 'Purpose=Observability,Shared=true'
  
# Save Monitor Workspace ID
MONITOR_WORKSPACE_ID=$(az monitor account show \
  --name "$MONITOR_WORKSPACE" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv)

echo "Monitor Workspace ID: $MONITOR_WORKSPACE_ID"
```

**Save the workspace ID from output** (format: `/subscriptions/.../providers/microsoft.monitor/accounts/otedge001-monitor`)

### Step 2: Create Azure Managed Grafana

Grafana provides visualization for Prometheus and Log Analytics data.

```bash
# Create Azure Managed Grafana
az grafana create \
  --name "$GRAFANA_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --tags 'Purpose=Observability,Shared=true'

# Save Grafana ID
GRAFANA_ID=$(az grafana show \
  --name "$GRAFANA_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv)

echo "Grafana ID: $GRAFANA_ID"

# Get Grafana endpoint URL
GRAFANA_ENDPOINT=$(az grafana show \
  --name "$GRAFANA_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --query properties.endpoint -o tsv)

echo "Grafana Endpoint: $GRAFANA_ENDPOINT"
```

**Save the Grafana ID from output** (format: `/subscriptions/.../providers/Microsoft.Dashboard/grafana/otedge001-grafana`)

**Grafana URL:**

In Azure Portal, navigate to the Grafana resource to find the public endpoint URL (e.g., [`https://otedge001-grafana-xxxxx.northeurope.grafana.azure.com`](https://otedge001-grafana-xxxxx.northeurope.grafana.azure.com/)).

### Step 3: Create Log Analytics Workspace

Log Analytics stores container logs and Kubernetes events.

**Note:** If you created a Log Analytics workspace in Part 1 for server monitoring, you can reuse it. Otherwise, create a new one:

```bash
# Create or reuse Log Analytics Workspace
az monitor log-analytics workspace create \
  --workspace-name "$LOG_ANALYTICS" \
  --resource-group "$RESOURCE_GROUP" \
  --location "$LOCATION" \
  --tags 'Purpose=Observability,Shared=true'

# Save Log Analytics ID
LOG_ANALYTICS_ID=$(az monitor log-analytics workspace show \
  --workspace-name "$LOG_ANALYTICS" \
  --resource-group "$RESOURCE_GROUP" \
  --query id -o tsv)

echo "Log Analytics ID: $LOG_ANALYTICS_ID"
```

**Save the workspace ID from output** (format: `/subscriptions/.../providers/Microsoft.OperationalInsights/workspaces/otedge001-logs`)

### Step 4: Grant Grafana Access to the Monitoring Workspace

Get the Principal ID for Grafana, and then delegate this principal **Monitoring Data Reader **to the  Azure Monitor Workspace.

```bash
# Get Grafana's managed identity principal ID
GRAFANA_PRINCIPAL_ID=$(az grafana show \
  --name "$GRAFANA_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --query identity.principalId -o tsv)

# Assign Monitoring Data Reader role to Grafana
az role assignment create \
  --role "Monitoring Data Reader" \
  --assignee "$GRAFANA_PRINCIPAL_ID" \
  --scope "$MONITOR_WORKSPACE_ID"
```

### Step 5: Enable Metrics Collection

Deploy Azure Monitor Agent extension to collect Prometheus metrics from the cluster.

**This command:**

* Deploys Azure Monitor Agent as DaemonSet
* Deploys Kube-State-Metrics for cluster-level metrics
* Configures Prometheus scraping
* Links to Azure Monitor Workspace and Grafana
```bash
# Set cluster variables
CLUSTER_NAME="p-we1iot-n013edge"

# Enable managed Prometheus metrics collection
az k8s-extension create \
  --name azuremonitor-metrics \
  --cluster-name "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --cluster-type connectedClusters \
  --extension-type Microsoft.AzureMonitor.Containers.Metrics \
  --release-train stable \
  --configuration-settings \
    azure-monitor-workspace-resource-id="$MONITOR_WORKSPACE_ID" \
    grafana-resource-id="$GRAFANA_ID"

# This takes 5-10 minutes to deploy
```

**Installation takes 5-10 minutes.** The command creates:

* ama-metrics DaemonSet (scrapes metrics from all nodes)
* ama-metrics-ksm Deployment (kube-state-metrics)
* ama-metrics-node-exporter DaemonSet (node metrics)
**Verify Installation:**

```bash
# Verify installation
kubectl get pods -n kube-system | grep ama-metrics

ama-metrics-5bc5748849-6f9d4                          2/2     Running     0              2m29s
ama-metrics-5bc5748849-zv5hw                          2/2     Running     0              2m29s
ama-metrics-ksm-6c9746f9bb-hmfxc                      1/1     Running     0              2m29s
ama-metrics-node-xgpmf                                2/2     Running     0              2m29s
ama-metrics-operator-targets-799887c4cc-jzjs2         2/2     Running     2 (112s ago)   2m29s
```

### Step 6: Enable Container Logs Collection

Deploy Container Insights extension to collect logs and Kubernetes events.

```bash
# Enable Container Insights
az k8s-extension create \
  --name azuremonitor-containers \
  --cluster-name "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --cluster-type connectedClusters \
  --extension-type Microsoft.AzureMonitor.Containers \
  --release-train stable \
  --configuration-settings \
    logAnalyticsWorkspaceResourceID="$LOG_ANALYTICS_ID" \
    amalogs.useAADAuth=true
```

**Installation takes 10-15 minutes** (this is the slowest operation).

The command creates:

* ama-logs-* (DaemonSet - log collection)
* ama-logs-rs-* (ReplicaSet - aggregation)
**Verify Installation:**

```bash
# Verify installation
kubectl get pods -n azuremonitor-containers
kubectl get pods -n kube-system | grep ama-logs

NAME                                                  READY   STATUS    RESTARTS   AGE
ama-logs-mf8jk                                        3/3     Running   0          10m
ama-logs-rs-77ffcf5969-vfzfd                          2/2     Running   0          10m
```

### Step 7: Apply Azure Monitor Logging Preset

The logging preset optimizes log collection to reduce costs. The **cost-optimized** preset is recommended for edge/IoT scenarios.

* Excludes logs from system namespaces (kube-system, azure-arc, etc.)
* Collects only INFO level logs and above (skips DEBUG/TRACE)
* Reduces CPU/memory limits for log agents
* Focuses on application namespaces (like azure-iot-operations)
In the portal we can edit collection settings in Azure Portal, by navigating to: **Arc Cluster → Monitoring → Monitor Settings → Container logs → Edit collection settings **then select: "Cost-optimized" preset, and exclude namespaces: **kube-system, gatekeeper-system, azure-arc** Set the Log level to **Info**


Or, via the shell we can apply as follows:

```bash
# Apply cost-optimized logging preset via Azure CLI
az k8s-extension update \
  --name azuremonitor-containers \
  --cluster-name "$CLUSTER_NAME" \
  --resource-group "$RESOURCE_GROUP" \
  --cluster-type connectedClusters \
  --configuration-settings \
    logAnalyticsWorkspaceResourceID="$LOG_ANALYTICS_ID" \
    amalogs.useAADAuth=true \
    amalogs.logsettings.logLevel=info \
    amalogs.logsettings.excludeNamespaces="kube-system,gatekeeper-system,azure-arc" \
    amalogs.resources.daemonset.limits.cpu=150m \
    amalogs.resources.daemonset.limits.memory=750Mi


```

Now, we should have configured the data flows, we just need to ensure that as the administrator

WAIT 10-15 Mins

 

```bash
administrator@n013edge:~$ # Check for ContainerLogV2 data (the new format)
az monitor log-analytics query \
  --workspace "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.OperationalInsights/workspaces/p-we1iot-logs" \
  --analytics-query "ContainerLogV2 | where TimeGenerated > ago(1h) | take 10" \
  --timespan "PT1H"

# Alternatively, check if ANY data is arriving
az monitor log-analytics query \
  --workspace "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.OperationalInsights/workspaces/p-we1iot-logs" \
  --analytics-query "Heartbeat | where TimeGenerated > ago(1h) | take 10" \
  --timespan "PT1H"
(PathNotFoundError) The requested path does not exist
Code: PathNotFoundError
Message: The requested path does not exist
(PathNotFoundError) The requested path does not exist
Code: PathNotFoundError
Message: The requested path does not exist
```

### Step 7: Grant Grafana Admin Access

Now, we should have configured the data flows, we just need to ensure that as the administrator we have privilages in Grafana to administrator the environment.

```bash
# Grant yourself Grafana Admin access
USER_OBJECT_ID=$(az ad signed-in-user show --query id -o tsv)

az role assignment create \
  --role "Grafana Admin" \
  --assignee "$USER_OBJECT_ID" \
  --scope "$GRAFANA_ID"

# Open Grafana
echo "Grafana URL: $GRAFANA_ENDPOINT"
```

Navigate to **Azure Portal → Arc Kubernetes Cluster → Monitoring → Insights**


### Step 8: View Metrics in Azure Portal

Navigate to **Azure Portal → Arc Kubernetes Cluster → Monitoring → Insights**

**Available Views:**

1. Cluster: Overall health, node count, pod count
1. Nodes: Per-node CPU, memory, disk usage
1. Controllers: Deployment and StatefulSet status
1. Containers: Per-container resource usage and logs
**Example Metrics:**

* CPU usage percentage by node
* Memory working set by namespace
* Pod restart count
* Network bytes sent/received
**Time Ranges:**

Adjust from last hour to last 30 days via dropdown.


—

**Navigate to Grafana → Dashboards → Browse**

**NEW: Pre-configured Arc Kubernetes dashboards:**

1. Kubernetes / Compute Resources / Cluster - Overall cluster view
1. Kubernetes / Compute Resources / Namespace (Workloads) - Per-namespace breakdown
1. Kubernetes / Compute Resources / Pod - Per-pod metrics
1. Kubernetes / Networking / Cluster - Network traffic
1. Node Exporter / Nodes - System-level node metrics
**The dashboard you saw in the portal is automatically configured!**

### Step 7: View Container Insights in Azure Portal

Navigate to:

**Azure Portal → p-we1iot-n013edge Arc cluster → Monitoring → Insights**

Views available:

* Cluster: Health, node count, pod count, resource usage
* Nodes: Per-node CPU, memory, disk
* Controllers: Deployments, StatefulSets, DaemonSets
* Containers: Per-container logs and metrics
* Logs: KQL queries against Log Analytics
### Verification Commands

—


### Step 9: Access Grafana Dashboards

**Open Grafana:**

1. Navigate to Resource Group → Grafana instance
1. Click Endpoint link to open Grafana
1. Authenticate with your Azure credentials
**Pre-Built Dashboards:**

Navigate to **Dashboards → Browse** to find:

* Kubernetes / Compute Resources / Cluster: Overall cluster metrics
* Kubernetes / Compute Resources / Namespace: Per-namespace breakdown
* Kubernetes / Compute Resources / Pod: Per-pod detailed metrics
* Node Exporter / Nodes: System-level metrics
**Sample Dashboard View:**

The Cluster dashboard shows:

* CPU utilization across all pods
* Memory usage with requests/limits
* Network I/O rates
* Pod count by phase (Running/Pending/Failed)
**Customization:**

* Clone dashboards and modify queries
* Create custom panels with PromQL queries
* Set up alerting rules
* Share dashboards with team members
### Troubleshooting: No Data in Grafana

**If dashboards show "No Data":**

1. Wait for Initial Ingestion: First data appears 10-15 minutes after extension installation
1. Verify Metrics Pods Are Running:
  ```bash
  kubectl get pods -n kube-system | grep ama-metrics
  ```
  
  All pods should show `Running` status.
  
  1. Check Metrics Agent Logs:
  ```bash
  kubectl logs -n kube-system <ama-metrics-pod-name>
  ```
  
  Look for scraping errors or authentication failures.
  
  1. Verify Grafana Data Source:
  In Grafana → Configuration → Data Sources, confirm:
  
  * Azure Monitor Workspace is configured
  * Connection test succeeds
  1. Check Prometheus Endpoints:
  ```bash
  kubectl port-forward -n kube-system svc/ama-metrics 9090:9090
  curl http://localhost:9090/metrics
  ```
  
  Should return Prometheus metrics.
  
  ## Production Readiness Checklist

Before deploying Azure IoT Operations (Part 3), validate your K3s cluster meets all requirements:

### Infrastructure Validation

✅ **K3s Cluster**

- [ ] K3s installed and running (check: kubectl get nodes)
- [ ] Single or multi-node configuration operational
- [ ] User kubeconfig configured (~/.kube/config)
- [ ] Performance optimizations applied (inotify, file descriptors)
✅ **Azure Arc Connection**

- [ ] Cluster registered in Azure (connectivityStatus: Connected)
- [ ] OIDC issuer enabled and configured
- [ ] Workload identity enabled
- [ ] Custom locations feature enabled
- [ ] All Arc agents running in azure-arc namespace
✅ **Access & Security**

- [ ] Service account created with appropriate permissions
- [ ] Token saved securely
- [ ] Azure Portal Kubernetes resources accessible
- [ ] Arc proxy tested from developer workstation
✅ **Monitoring & Observability**

- [ ] Azure Monitor Workspace created
- [ ] Azure Managed Grafana deployed
- [ ] Log Analytics Workspace configured
- [ ] Metrics collection enabled (verify pods running)
- [ ] Container Insights enabled
- [ ] Data flowing to Grafana dashboards (wait 15min after installation)
### Capacity Validation

**Check Available Resources:**

```bash
# View node capacity and allocatable resources
kubectl describe nodes

# Look for:
# - Allocatable CPU (should have >2 cores free)
# - Allocatable Memory (should have >4Gi free)
# - Disk pressure (should be False)
```

**Azure IoT Operations Requirements:**

* Minimum 4 cores total (2 cores for system, 2 for workloads)
* Minimum 8GB RAM total (2-3GB for system, 5GB+ for workloads)
* Minimum 40GB disk with >20% free space
### Pre-Flight Test

**Deploy a test workload to validate cluster functionality:**

```bash
# Create test namespace
kubectl create namespace validation

# Deploy nginx pod
kubectl run nginx --image=nginx --port=80 -n validation

# Verify pod is running
kubectl get pods -n validation

# Create service
kubectl expose pod nginx --port=80 --type=NodePort -n validation

# Test connectivity
kubectl port-forward -n validation pod/nginx 8080:80
curl http://localhost:8080

# Clean up
kubectl delete namespace validation
```

**Expected Results:**

* Pod reaches Running status within 2 minutes
* Service exposes NodePort
* Port forward succeeds
* Curl returns nginx welcome page
## Next Steps: Preparing for Azure IoT Operations

With Arc-enabled K3s configured and monitored, you're ready for Azure IoT Operations deployment. Part 3 will cover:

**Part 3: Deploying Azure IoT Operations**

* Azure IoT Operations architecture and components
* Prerequisites validation and schema registry setup
* MQTT broker deployment and configuration
* Data processor pipelines for telemetry transformation
* Azure Key Vault integration for secrets management
* Workload identity configuration
**Part 4: Deploy Node-RED for IoT Edge**

* Node-RED installation on K3s
* Persistent storage configuration
* Modbus and MQTT integration
* Azure IoT Operations connectivity
* Production flow patterns
**Key Prerequisites Before Continuing to Part 3:**

✅ K3s cluster running and Arc-enabled

✅ Workload identity and OIDC issuer configured

✅ Custom locations enabled

✅ Azure Monitor and Grafana operational

✅ At least 4GB RAM and 2 cores available for IoT Operations

✅ Port 8883 available for MQTT broker (check: `netstat -tuln | grep 8883`)

## Additional Resources

**K3s Documentation:**

* K3s Installation
* K3s Architecture
* K3s Storage Options
* K3s Networking
**Azure Arc Kubernetes:**

* Arc-Enabled Kubernetes Overview
* Connect Cluster to Arc
* Custom Locations
* Cluster Connect
**Azure Monitor for Kubernetes:**

* Container Insights Overview
* Azure Monitor Workspace
* Azure Managed Grafana
**Azure IoT Operations:**

* Azure IoT Operations Preview
* Deploy IoT Operations
* MQTT Broker Configuration
**Questions or feedback?** Reach out via [LinkedIn](https://linkedin.com/in/damian-flynn). If this guide helped you build Arc-enabled edge infrastructure, share it with your team!

**Up next**: We'll deploy Azure IoT Operations to enable MQTT brokering, data processing pipelines, and secure device connectivity at the edge.

