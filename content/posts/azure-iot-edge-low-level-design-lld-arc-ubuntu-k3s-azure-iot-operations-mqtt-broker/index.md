---
title: "Azure IoT Edge Low-Level Design (LLD): Arc Ubuntu → K3s → Azure IoT Operations → MQTT Broker"
date: "2025-10-20T13:42:00.000Z"
lastmod: "2025-10-20T13:43:00.000Z"
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
summary: "End-to-end low-level design for building an Azure IoT Operations edge: Azure Arc-enable Ubuntu, deploy and Arc-connect K3s, install and secure Azure IoT Operations, and expose the MQTT broker with NodePort/LoadBalancer and TLS (including x509 auth). Includes commands, reference outputs, network endpoints, cert flows, and observability."
Tags:
  [
    "IoT",
    "Linux",
    "Docker",
    "IaC",
    "DevOps",
    "Bicep"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "0ba905b9-0db9-45ae-93bb-32eca4118f43",
    "created_time": "2025-10-20T13:42:00.000Z",
    "last_edited_time": "2025-10-20T13:43:00.000Z",
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
      "emoji": "🧩"
    },
    "parent": {
      "type": "data_source_id",
      "data_source_id": "235a5f88-c313-46d9-84b5-9f168a1633b7",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Azure-IoT-Edge-Low-Level-Design-LLD-Arc-Ubuntu-K3s-Azure-IoT-Operations-MQTT-Broker-0ba905b90db945ae93bb32eca4118f43",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:46:16.621Z"
last_edited_time: "2025-10-20T13:43:00.000Z"
---

> Azure IoT Edge LLD for a single or small multi-node Ubuntu environment, built progressively from Azure Arc-enabled Linux host to Arc-connected K3s, then Azure IoT Operations, concluding with MQTT Broker exposure and security. The document prioritizes deterministic, reproducible steps with exact commands, expected outputs, and operational guardrails.

## Scope and Audience

* Purpose: Provide a production-lean, technically rigorous low-level design (LLD) for deploying Azure IoT Operations (AIO) on an Arc-enabled Ubuntu host with K3s, then exposing and securing the built-in MQTT Broker.
* Audience: Experienced Linux, Kubernetes, and Azure engineers. Assumes comfort with CLI, YAML, Kubernetes manifests, Azure CLI, PKI basics, and firewalling.
* Out of scope: Full HA multi-node patterns, GitOps, regional DR, end-to-end app lifecycle, cost governance.
## High-Level Sequence

1. Arc-enable Ubuntu server (Azure Arc-enabled Server, web-socket connectivity, remote SSH via Arc)
1. Install K3s and connect Kubernetes to Azure Arc (enable OIDC + Workload Identity)
1. Deploy Azure IoT Operations via Azure CLI and Portal (optionally with observability stack)
1. Configure and expose the MQTT Broker (ClusterIP default, plus NodePort and LoadBalancer with TLS and x509)
# 1) Azure Arc—Enable Ubuntu Server

### 1.1 Baseline host

* Ubuntu 22.04 or 24.04 LTS, SSH accessible locally
* Example VM: 2 vCPU, 4 GB RAM, KVM/QEMU
### 1.2 Generate Arc onboarding script (Portal)

* Azure Portal → Azure Arc → Machines → Add/Create → Add a single server
* Select Subscription, Resource Group, Region, OS=Linux, Public Endpoint (initially)
* Review tags strategy (Datacenter, City, Country, environment, etc.)
### 1.3 Install Arc agent and connect

Run the portal-generated script (example below) on the edge host:

```bash
export subscriptionId="<SUBSCRIPTION_ID>"
export resourceGroup="iot-operations"
export tenantId="<TENANT_ID>"
export location="northeurope"
export authType="token"
export correlationId="<GUID>"
export cloud="AzureCloud"

# Download and install agent
LINUX_INSTALL_SCRIPT="/tmp/install_linux_azcmagent.sh"
rm -f "$LINUX_INSTALL_SCRIPT" || true
wget https://gbl.his.arc.azure.com/eus2/SomePath/install_linux_azcmagent.sh -O "$LINUX_INSTALL_SCRIPT"
bash "$LINUX_INSTALL_SCRIPT"

# Connect (device code flow)
sudo azcmagent connect \
  --resource-group "$resourceGroup" \
  --tenant-id "$tenantId" \
  --location "$location" \
  --subscription-id "$subscriptionId" \
  --cloud "$cloud" \
  --tags 'Datacenter=North Europe,City=Ballina,StateOrDistrict=Mayo,CountryOrRegion=Ireland,environment=MVP,ArcSQLServerExtensionDeployment=Disabled' \
  --correlation-id "$correlationId"
```

* Follow device code instructions, MFA as required.
* Expected: resource created at Microsoft.HybridCompute/machines/<hostname>.
### 1.4 Remote SSH over Arc (no inbound hole-punching)

Two options:

* Browser-based via Cloud Shell
* Local terminal with Azure CLI
CLI example:

```bash
az ssh arc --subscription "<SUBSCRIPTION_ID>" \
  --resource-group "iot-operations" \
  --name "<hostname>" \
  --local-user "sysadmin"
```

Notes:

* First attempt may prompt to update allowed port (22) in service configuration; re-run after ~1–2 min if endpoint not yet listening.
* Under the hood uses Azure Relay over outbound 443 web-socket.
### 1.5 Update Management

* In the Arc machine blade → Operations → Updates → enable periodic assessment.
* Use Automation Update Management or your patching SRE pipeline.
# 2) Install K3s and Connect Kubernetes to Azure Arc

## 2.1 K3s Single-Node Install

```bash
curl -sfL https://get.k3s.io | sh -
# kubectl/k3s set up; kubeconfig at /etc/rancher/k3s/k3s.yaml
```

Post-install developer convenience (non-root kubeconfig):

```bash
mkdir -p ~/.kube
sudo KUBECONFIG=~/.kube/config:/etc/rancher/k3s/k3s.yaml kubectl config view --flatten > ~/.kube/merged
mv ~/.kube/merged ~/.kube/config
chmod 0600 ~/.kube/config
export KUBECONFIG=~/.kube/config
kubectl config use-context default
```

Verification:

```bash
kubectl version --client
kubectl get nodes -o wide
```

### 2.2 Kernel and FS tunings (recommended)

```bash
echo fs.inotify.max_user_instances=8192 | sudo tee -a /etc/sysctl.conf
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
echo fs.file-max=100000 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## 2.3 Azure CLI and Provider Registration

```bash
sudo apt update && sudo apt install -y ca-certificates curl apt-transport-https lsb-release gnupg
curl -sL https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/microsoft.gpg
AZ_REPO=$(lsb_release -cs)
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
sudo apt update && sudo apt install -y azure-cli

az login
az provider register --namespace Microsoft.Kubernetes
az provider register --namespace Microsoft.KubernetesConfiguration
az provider register --namespace Microsoft.ExtendedLocation
az provider register --namespace Microsoft.SecretSyncController

az extension add --name connectedk8s
az extension add --name k8s-extension
az extension add --name customlocation
```

## 2.4 Connect K3s to Azure Arc with OIDC + Workload Identity

```bash
az group create -g iot-edge01 -l NorthEurope

az connectedk8s connect \
  --name otedge001 \
  --location NorthEurope \
  --resource-group iot-edge01 \
  --enable-oidc-issuer \
  --enable-workload-identity \
  --disable-auto-upgrade
```

Verify agents:

```bash
kubectl get pods -n azure-arc -o wide
```

### 2.5 Enable Custom Locations

Add K3s API server flags and enable Arc features:

```bash
ISSUER_URL=$(az connectedk8s show -g iot-edge01 -n otedge001 --query oidcIssuerProfile.issuerUrl -o tsv)

sudo tee /etc/rancher/k3s/config.yaml >/dev/null <<EOF
kube-apiserver-arg:
  - service-account-issuer=${ISSUER_URL}
  - service-account-max-token-expiration=24h
EOF

sudo systemctl restart k3s

# Enable features (requires specific CL OID)
export OBJECT_ID=$(az ad sp show --id bc313c14-388c-4e7d-a58e-70017303ee3b --query id -o tsv)
az connectedk8s enable-features \
  --resource-group iot-edge01 \
  --name otedge001 \
  --custom-locations-oid $OBJECT_ID \
  --features cluster-connect custom-locations
```

### 2.6 Optional: Arc Kubernetes Portal Access Token

```bash
kubectl create serviceaccount arc-user -n default
kubectl create clusterrolebinding arc-user-binding --clusterrole cluster-admin --serviceaccount default:arc-user
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: arc-user-secret
  annotations:
    kubernetes.io/service-account.name: arc-user
type: kubernetes.io/service-account-token
EOF
TOKEN=$(kubectl get secret arc-user-secret -o jsonpath='{$.data.token}' | base64 -d)
```

Use token in Azure Portal → Kubernetes Resources → Namespaces sign-in.

# 3) Azure IoT Operations (AIO) Deployment

## 3.1 Prereqs and CLI Extension

```bash
az extension add --upgrade --allow-preview --name azure-iot-ops
az upgrade
```

Register providers for AIO services as needed during flow.

## 3.2 Data services for AIO

* Schema Registry + Namespace
* Storage account with hierarchical namespace (for schema registry)
Example:

```bash
# Create Schema Registry
az iot ops schema registry create \
  --subscription <SUB> -g t-we1iot -n t-we1iot-schema \
  --registry-namespace t-we1iot \
  --sa-resource-id \
   /subscriptions/<SUB>/resourceGroups/t-we1iot/providers/Microsoft.Storage/storageAccounts/twe1iotstore \
  --sa-container n013edge
```

## 3.3 Initialize cluster for AIO (foundation)

```bash
az iot ops init -g t-we1iot --subscription <SUB> --cluster t-we1iot-n013edge
# Installs: platform, secretStore, containerStorage
```

## 3.4 Observability (Optional but recommended)

Deploy OTel collector in namespace `azure-iot-operations` and wire Azure Monitor + Grafana.

* Create Azure Monitor workspace (Prometheus)
* Create Azure Managed Grafana
* Install Arc k8s extensions for metrics and container insights
```bash
az monitor account create --name <A_MON_WS> --resource-group iot-edge01 -l NorthEurope
az grafana create --name <GRAF> --resource-group iot-edge01 -l NorthEurope

az k8s-extension create \
  --name azuremonitor-metrics \
  --cluster-name otedge001 \
  --resource-group iot-edge01 \
  --cluster-type connectedClusters \
  --extension-type Microsoft.AzureMonitor.Containers.Metrics \
  --configuration-settings \
    azure-monitor-workspace-resource-id=<A_MON_WS_ID> \
    grafana-resource-id=<GRAF_ID>

az k8s-extension create \
  --name azuremonitor-containers \
  --cluster-name otedge001 \
  --resource-group iot-edge01 \
  --cluster-type connectedClusters \
  --extension-type Microsoft.AzureMonitor.Containers \
  --configuration-settings \
    logAnalyticsWorkspaceResourceID=<LOGANALYTICS_WS_ID>
```

Optional OTel Collector (Helm):

```yaml
# otel-collector-values.yaml (key excerpts)
mode: deployment
fullnameOverride: aio-otel-collector
config:
  receivers:
    otlp:
      protocols: { grpc: { endpoint: ":4317" }, http: { endpoint: ":4318" } }
  exporters:
    prometheus: { endpoint: ":8889", resource_to_telemetry_conversion: { enabled: true } }
```

Deploy:

```bash
kubectl get namespace azure-iot-operations || kubectl create namespace azure-iot-operations
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update
helm upgrade --install aio-observability open-telemetry/opentelemetry-collector \
  -f otel-collector-values.yaml -n azure-iot-operations
```

## 3.5 Create AIO instance

Two sizing examples:

* Tiny profile (resource constrained)
* Low profile with OTel metrics piping
```bash
# Tiny profile
az iot ops create \
  --subscription <SUB> -g t-we1iot \
  --cluster t-we1iot-n013edge \
  --custom-location <CUSTOM_LOCATION_NAME> \
  -n <AIO_INSTANCE_NAME> \
  --sr-resource-id <SCHEMA_REGISTRY_ID> \
  --ns-resource-id <NAMESPACE_ID> \
  --broker-frontend-replicas 1 \
  --broker-frontend-workers 1 \
  --broker-backend-part 1 \
  --broker-backend-workers 1 \
  --broker-backend-rf 2 \
  --broker-mem-profile Tiny

# With OTel
az iot ops create \
  --subscription <SUB> -g t-we1iot \
  --cluster t-we1iot-n013edge \
  --custom-location <CUSTOM_LOCATION_NAME> \
  -n <AIO_INSTANCE_NAME> \
  --sr-resource-id <SCHEMA_REGISTRY_ID> \
  --ns-resource-id <NAMESPACE_ID> \
  --broker-frontend-replicas 2 \
  --broker-frontend-workers 2 \
  --broker-backend-part 2 \
  --broker-backend-workers 2 \
  --broker-backend-rf 2 \
  --broker-mem-profile Low \
  --ops-config observability.metrics.openTelemetryCollectorAddress=aio-otel-collector.azure-iot-operations.svc.cluster.local:4317 \
  --ops-config observability.metrics.exportInternalSeconds=60
```

## 3.6 Secret sync and managed identities (Secure option)

```bash
# Enable secret sync from Key Vault
az iot ops secretsync enable \
  -g <RG> -n <AIO_INSTANCE_NAME> \
  --kv-resource-id <KEYVAULT_ID> \
  --mi-user-assigned <UAMI_SECRETS_ID>

# Assign MI for cloud connections
az iot ops identity assign \
  -g <RG> -n <AIO_INSTANCE_NAME> \
  --mi-user-assigned <UAMI_COMPONENTS_ID>
```

## 3.7 Post-deploy health

```bash
az iot ops check --svc broker --detail-level 1
kubectl -n azure-iot-operations get pods
kubectl -n azure-iot-operations get svc | grep broker
```

# 4) MQTT Broker Exposure and Security

## 4.1 Defaults

* Broker deploys a default listener (ClusterIP, port 18883) using Kubernetes Service Account Token (SAT) authentication. Scope: in-cluster only.
* Use this for in-cluster workloads.
## 4.2 Add NodePort listener (no auth, lab only)

* Use a dedicated port (e.g., 31883, NodePort ranges 30000–32767)
* Authentication: none
* Authorization: none (lab only; enforce in production)
## 4.3 Add LoadBalancer listener with TLS

* Create listener: name loadbalancer-listener, service loadbalancer-listener-service, port 8883
* TLS: Automatic or Manual
  * Automatic requires cert-manager issuer (ClusterIssuer recommended)
  * SANs should include external IPs and any DNS (e.g., n013edge.local, 10.13.100.11, 100.81.128.71)
  Patch example to use working ClusterIssuer:

```bash
kubectl patch brokerlistener loadbalancer-listener -n azure-iot-operations --type='merge' -p='{
  "spec": {
    "ports": [
      {
        "port": 8883,
        "protocol": "Mqtt",
        "tls": {
          "mode": "automatic",
          "certManagerCertificateSpec": {
            "issuerRef": {
              "group": "cert-manager.io",
              "kind": "ClusterIssuer",
              "name": "azure-iot-operations-aio-certificate-issuer"
            },
            "privateKey": { "algorithm": "Ec256", "rotationPolicy": "Always" },
            "san": { "dns": ["n013edge.local"], "ip": ["10.13.100.11", "100.81.128.71"] }
          }
        }
      }
    ]
  }
}'
```

Validate:

```bash
kubectl get service -n azure-iot-operations | grep LoadBalancer
kubectl get certificates -n azure-iot-operations
kubectl get secrets -n azure-iot-operations | grep tls
```

## 4.4 Manual TLS and x509 client auth (alternative)

If using manual TLS and custom client CAs:

```bash
# Server-side TLS secret
kubectl create secret tls broker-server-cert -n azure-iot-operations \
  --cert=mqtts-endpoint.crt \
  --key=mqtts-endpoint.key

# ConfigMap containing client root CA
kubectl create configmap clients42-ca -n azure-iot-operations \
  --from-file=client_ca.pem=clients_root_ca.crt
```

* In AIO portal, set listener TLS mode to Manual, secret name broker-server-cert.
* In Broker Authentication x509 policy details:
```json
{ "trustedClientCaCert": "clients42-ca" }
```

## 4.5 Authentication policies

* SAT for in-cluster
* None (lab only)
* x509 (recommended for edge clients)
* Custom (third-party endpoint)
Example test sequence (TLS only, no auth):

```bash
# Extract broker internal CA for testing if applicable
kubectl get secret aio-broker-internal-ca -n azure-iot-operations -o jsonpath='{.data.ca\.crt}' | base64 -d > broker-ca.crt

# Test publish
mosquitto_pub -h 10.13.100.11 -p 8883 -t "test/topic" -m "hello" --cafile broker-ca.crt -d
```

Example x509 client (manual PKI):

```bash
mosquitto_pub -t "testtopic/test" -m "client42a measurement" -i client42a \
  -q 1 -V mqttv5 -d -h 10.13.100.11 -p 8883 \
  --key client42a.key --cert client42a.crt --cafile server_root_ca.crt
```

## 4.6 Multi-listener behavior

* Messages traverse between listeners; subscribers on NodePort receive publications sent via LoadBalancer and vice versa, subject to authorization policies.
* Expect self-test topics from MQTT service on wildcard subscriptions: azedge/dmqtt/selftest/*.
## 4.7 Client examples

* MQTTX: TLS enabled, self-signed, optional CA file import
* Node-RED: configure TLS with client cert and CA
* Mosquitto CLI: as above
# Networking and Security

* Egress 443 required for Arc agents, Arc SSH, AIO control plane
* For LoadBalancer, confirm network assigns stable external IP; otherwise use static address or MetalLB (on-prem)
* For NodePort, use ports 30000–32767; ensure host firewall permits inbound
* Lock down auth: prefer x509 or SAT; avoid "None" beyond lab
* Ensure SANs match endpoint IP/DNS or clients will fail hostname verification
* Do not terminate TLS on intermediate hops if using client cert auth without mTLS pass-through
# Operations and Troubleshooting

* Arc SSH transient failures right after service configuration changes are normal; retry after 60–120 seconds
* az iot ops check --svc broker provides structured health report
* Inspect pods:
```bash
kubectl -n azure-iot-operations get pods
kubectl -n azure-iot-operations logs <pod> -c broker
```

* Certificate issues: check Certificate/Issuer objects and Secret presence
* Observability: verify ama-metrics* pods and OTel exporter endpoints; Grafana dashboards may take 5–15 min to populate
# Deliverables and Artifacts

* Arc-enabled Linux host with remote SSH via Arc
* Arc-connected K3s cluster with OIDC and Workload Identity
* Azure IoT Operations instance (Tiny/Low profile)
* MQTT Broker exposed on NodePort 31883 (lab) and LoadBalancer 8883 (TLS)
* Optional x509 client auth wired via ConfigMap CA and Manual TLS Secret
* Observability: Azure Monitor (Prometheus) and optional OTel collector, Grafana dashboards
# Appendix A — Example OpenSSL PKI (Manual)

* Root and Intermediate CAs for server and clients
* Endpoint cert with SAN=IP(s) and/or DNS
```bash
# Server root & intermediate
openssl genrsa -out aio_root_ca.key 4096
openssl req -x509 -new -key aio_root_ca.key -sha256 -days 3000 -subj "/CN=AIO Root CA" -out aio_root_ca.crt

openssl genrsa -out aio_int_ca.key 4096
openssl req -new -key aio_int_ca.key -subj "/CN=AIO Intermediate CA" -out aio_int_ca.csr
openssl x509 -req -in aio_int_ca.csr -CA aio_root_ca.crt -CAkey aio_root_ca.key -CAcreateserial -out aio_int_ca.crt -days 3000 -sha256

# Endpoint cert
openssl genrsa -out mqtts-endpoint.key 2048
openssl req -new -key mqtts-endpoint.key -subj "/CN=mqtts-endpoint" -out mqtts-endpoint.csr
cat > san.cnf <<EOF
[ req ]
distinguished_name=req
[ v3_req ]
subjectAltName=DNS:n013edge.local,IP:10.13.100.11,IP:100.81.128.71
EOF
openssl x509 -req -in mqtts-endpoint.csr -CA aio_int_ca.crt -CAkey aio_int_ca.key -CAcreateserial -out mqtts-endpoint.crt -days 3000 -sha256 -extfile san.cnf -extensions v3_req
```

# Appendix B — Quick Verification Matrix

* Arc server connected: Azure Portal blade shows Connected; SSH over Arc succeeds
* K3s ready: kubectl get nodes shows Ready; azure-arc namespace pods Running
* AIO instance: az iot ops check --svc broker all succeeded
* MQTT listeners:
  * ClusterIP 18883 (SAT) reachable from pods
  * NodePort 31883 reachable from LAN clients (no auth, lab)
  * LoadBalancer 8883 reachable via TLS; CA trusted by clients
  * Observability: Metrics present in Azure Monitor and Grafana
This LLD stitches the Arc-enabled Linux host, Arc-connected K3s, AIO deployment, and MQTT broker exposure into a consistent, reproducible path. Use the Tiny profile for constrained edge labs, then scale broker cardinality, replicas, and memory profile for production traffic, and move from ‘None’ to x509 or custom auth with proper authorization and mTLS.

