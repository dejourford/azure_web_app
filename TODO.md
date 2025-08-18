# TODO: Azure Cloud Security Lab

## Phase 1: Deployment
- [X] Create resource group
- [X] Deploy Juice Shop web app
- [X] Configure NSG and VNet

## Phase 2: Security Setup
- [X] Set up Log Analytics Workspace 
- [X] Enable Microsoft Defender for Endpoint
- [X] Set up Microsoft Sentinel


## Phase 3: Simulated Attacks
- [ ] Run SQL injection attack (staging/prod only)
- [ ] Run brute-force login (staging/prod only)

## Phase 4: Detection & Response
- [ ] Query logs in Sentinel (staging and prod only)
- [ ] Build analytics rules (staging and prod only)
- [ ] Automate response with Logic Apps (staging and prod only)


# Recommended Fresh Build (Enterprise-aligned)


### Step 1: Subscriptions & Resource Groups

* **Create 2 subscriptions** (if your Azure account allows):

  * One for **Prod**
  * One for **Non-Prod** (Dev + Stage inside)
* At minimum, use **separate RGs**: `rg-hub`, `rg-prod`, `rg-dev`, `rg-stage`.

### Step 2: Networking (Hub-and-Spoke)

* Create **Hub VNet** (e.g. `10.0.0.0/16`)

  * Subnets: `AzureBastionSubnet`, `AzureFirewallSubnet`, maybe a shared services subnet
* Create **Spoke VNets** for each env (e.g. `10.1.0.0/16`, `10.2.0.0/16`, `10.3.0.0/16`)

  * Subnets per env: `web`, `app`, `data`
* **Peer hub ↔ spokes** (allow hub→spoke + spoke→hub)

### Step 3: Shared Services (Hub)

* Deploy **Azure Bastion (Standard)** in `AzureBastionSubnet`
* Deploy **Azure Firewall** in `AzureFirewallSubnet`
* Configure **UDRs** in spokes so *all outbound traffic* goes → Firewall

### Step 4: Ingress

* Deploy **App Gateway WAF v2** (start with Prod)

  * Public IP only on the gateway
  * WAF policy in Prevention (OWASP rules)
  * Backend = Prod VM (private IP)
* Later: add AppGW for Dev/Stage or route them through one AppGW using hostnames

### Step 5: Workloads

* Create **VMs in each spoke** (Dev/Stage/Prod)

  * No public IPs
  * NSGs tight (only allow AppGW → web port, Bastion → SSH)
  * Run Juice Shop container inside

### Step 6: Identity & Secrets

* Enable **Managed Identity** on VMs (so apps can grab Key Vault secrets)
* Use **Key Vault** for TLS certs + app secrets

### Step 7: Logging & Security

* One **central Log Analytics workspace** (in hub or a security RG)
* Send:

  * AppGW logs (access + firewall)
  * Azure Firewall logs
  * NSG Flow logs (Traffic Analytics)
  * VM syslog/events
  * Azure Activity
* Connect **Microsoft Sentinel** for detection
* Turn on **Defender for Cloud Plan 2**

### Step 8: Governance (Optional polish)

* Apply **Azure Policies**: deny public IPs, require diagnostics, enforce tagging, enforce TLS
* Enable **DDoS Protection Standard** on hub VNet


