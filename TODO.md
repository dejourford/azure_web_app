☁️ Azure Cloud Security Engineer Lab: Secure & Monitor a Web App with Defender for Cloud & Sentinel
🧠 Objective:
Deploy a vulnerable web app in Azure, lock it down with security best practices, and configure Defender for Cloud + Sentinel to detect and respond to attacks.

🔧 Lab Outline
🔹 Phase 1: Infrastructure Deployment (Baseline Setup)
Deploy a simple Web App or VM

Use Azure App Service or a Linux VM running a vulnerable app (e.g., DVWA or OWASP Juice Shop)

Expose it publicly to simulate real-world attack surface

Create a Resource Group, VNet, NSG, and Storage Account

Set up network segmentation: Subnets for web and data layers

Apply basic NSG rules (e.g., allow only HTTP/HTTPS, block everything else)

Deploy Azure Firewall or Front Door (Optional but realistic)

Protect the app at the edge

🔹 Phase 2: Security Configuration (Cloud Hardening)
Enable Defender for Cloud (Free or Standard Tier)

Turn on threat protection for:

App Service or VM

Storage

Key Vault

SQL (if used)

Enable Security Center recommendations

Apply secure score fixes (e.g., disable RDP/SSH from internet, enforce MFA)

Enable Diagnostic Logs and send them to:

Log Analytics Workspace

Microsoft Sentinel

🔹 Phase 3: Simulate Attacks
Use Kali Linux or Burp Suite (from another VM or external IP) to:

Scan the app (nmap, nikto, gobuster)

Try a SQL injection or brute-force login (Juice Shop makes this easy)

Attempt to upload a malicious file

This will trigger Defender alerts and log data for Sentinel.

🔹 Phase 4: Threat Detection & Response
In Microsoft Sentinel:

Connect Defender for Cloud, AzureActivity, and NSG Flow Logs

Build queries like:

kql
Copy
Edit
SecurityAlert
| where AlertName contains "Brute Force" or "SQL Injection"
| project AlertName, CompromisedEntity, TimeGenerated, Description
Create Analytics Rules:

Alert on:

High-severity Defender alerts

NSG flow showing suspicious inbound traffic

Activity from unusual geolocations

(Optional) Use Logic App to automate response:

Disable VM NIC or stop the app

Notify via email/Teams

Create ServiceNow or Azure DevOps ticket

🛠️ Skills You’ll Demonstrate
Area	Tech
Infra as Code (optional)	ARM / Bicep / Terraform
Logging & Monitoring	Log Analytics, Azure Monitor, Sentinel
Detection & Response	Defender for Cloud, Analytics Rules, KQL
Secure Config	NSGs, Identity Protections, App hardening
Real-World Simulation	OWASP attacks, SIEM alerts, Playbooks

📘 Bonus Project Export
Document this lab into a portfolio-style case study:

Problem Statement

Architecture Diagram

Screenshots of alerts/logs

Sample KQL queries

Lessons learned


# TODO: Azure Cloud Security Lab

## Phase 1: Deployment
- [ ] Create resource group
- [ ] Deploy Juice Shop web app
- [ ] Configure NSG and VNet

## Phase 2: Security Setup
- [ ] Enable Microsoft Defender for Cloud
- [ ] Set up Log Analytics & Sentinel

## Phase 3: Simulated Attacks
- [ ] Run SQL injection attack
- [ ] Run brute-force login

## Phase 4: Detection & Response
- [ ] Query logs in Sentinel
- [ ] Build analytics rules
- [ ] Automate response with Logic Apps

