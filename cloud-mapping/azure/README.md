# Azure-to-SOC 2 Mapping

How Microsoft Azure services support the SOC 2 Trust Services Criteria, and which configurations satisfy specific criteria. This document is the pillar reference; the detailed control-by-service table lives in [controls.md](controls.md).

## Why specific service mapping matters

Telling your auditor "we use Azure, which is SOC 2 compliant" does not help you. Microsoft's SOC 2 attests to Microsoft's controls — physical security, infrastructure protections, foundational service capabilities — and not to *your* use of Azure. Your SOC 2 must demonstrate the controls *you* operate on top of Azure.

The granularity matters because auditors test specifics, not categories. They will not ask "do you have access controls?" They will ask "show me how production portal access is authenticated, who holds Global Administrator in your Microsoft Entra ID tenant, and how administrative activity is logged and reviewed." The answer is a series of specific service configurations — Microsoft Entra ID Conditional Access policies, Privileged Identity Management (PIM) assignments, Azure Monitor diagnostic settings shipping to Log Analytics, Microsoft Defender for Cloud recommendations — not the phrase "we use Azure securely."

This document and the linked table map each SOC 2 Common Criterion to the Azure services and configurations that satisfy it. Use them as a checklist when implementing controls and as a script when discussing your environment with the auditor.

## Shared responsibility — what Microsoft does, what you must do

Microsoft publishes a [Shared Responsibility in the Cloud](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility) model that splits security responsibilities by service category — IaaS, PaaS, and SaaS each shift the boundary differently. The practical division:

- **Microsoft is responsible for security *of* the cloud** — physical data centers, the host hypervisor, the host OS, the foundational network fabric, the foundational service implementations, and, for PaaS, the platform runtime and storage abstractions. Microsoft's SOC 2 attests to these.
- **You are responsible for security *in* the cloud** — Microsoft Entra ID identity governance, Azure RBAC role assignments, Azure Policy enforcement, network configuration (VNets, NSGs, Azure Firewall), encryption-key management (Azure Key Vault, customer-managed keys), guest OS patching for IaaS VMs, diagnostic settings and log routing, monitoring, application-level controls.

The split varies by service. For SaaS (Microsoft 365) Microsoft covers almost everything except identity, data, and devices. For PaaS (App Service, Azure SQL, Azure Functions, Container Apps, AKS managed-plane) Microsoft covers the runtime and storage substrate; you cover identity, data, and configuration. For IaaS (Virtual Machines, AKS node pools, self-managed databases) you cover the guest OS, runtime, and everything above.

In your SOC 2 system description, Azure will appear as a **subservice organization**, "carved out" of your report. The auditor will note that Microsoft's SOC 2 is reviewed and that Microsoft's controls support your environment. The auditor will not test Microsoft's controls; the auditor will test yours.

## Inheriting Microsoft's controls

You inherit Microsoft's controls by:

1. **Reviewing Microsoft's SOC 2 report at least annually.** Available through the Service Trust Portal (`https://servicetrust.microsoft.com/`). The latest SOC 2 Type 2 covers a 12-month period; review it for exceptions relevant to your in-scope services. Different SOC 2 reports cover different service families (Azure, Microsoft 365, Dynamics 365) — match the report to the services you use.
2. **Subscribing to Azure Service Health, Microsoft Security Response Center (MSRC) advisories, and Microsoft Defender for Cloud alerts** to be aware of operational issues that affect your controls.
3. **Documenting Microsoft as a subservice organization** in your system description, including the Trust Services Categories Microsoft's SOC 2 covers and the carve-out methodology.
4. **Defining Complementary User Entity Controls (CUECs)** — controls *you* must operate for Microsoft's controls to be effective. Examples specific to Azure: configuring Conditional Access for MFA enforcement, enabling Defender for Cloud across all subscriptions, configuring diagnostic settings to ship logs to Log Analytics, applying Azure Policy initiatives at the management-group level, patching Virtual Machines you manage.

## Where the Service Trust Portal fits

The [Service Trust Portal](https://servicetrust.microsoft.com/) is Microsoft's portal for compliance documentation. From it, you can download:

- Microsoft Azure SOC 1, SOC 2, and SOC 3 reports
- Microsoft 365 SOC 1, SOC 2 reports
- ISO 27001, 27017, 27018, 27701 certificates
- PCI DSS Attestation of Compliance
- HIPAA / HITRUST documentation and the Microsoft Business Associate Agreement (BAA)
- FedRAMP, IRS 1075, CJIS, and other regional/industry-specific attestations

For SOC 2 readiness, the Azure SOC 2 Type 2 report is the primary document. The SOC 3 report is a public-facing summary — useful for customer-facing trust pages, not for audit work. Access requires sign-in with a Microsoft Entra ID account (work or school) and acceptance of the NDA-equivalent terms before downloading restricted reports.

## Architectural patterns that support SOC 2

Several Azure architectural patterns substantially simplify SOC 2 compliance. They are not strictly required, but they are common in SOC 2-compliant SaaS environments running on Azure:

### Management-group hierarchy with subscriptions

A single Azure subscription is rarely sufficient for SOC 2. The accepted pattern is a layered hierarchy under the tenant root:

- **Tenant Root Group** — Microsoft Entra ID tenant; holds tenant-wide identity configuration
- **Management Groups** — environment or business-unit boundaries (e.g., `Platform`, `Landing Zones`, `Sandbox`, `Decommissioned`) — typically following the Microsoft Cloud Adoption Framework (CAF) enterprise-scale landing-zone pattern
- **Subscriptions** — workload-level isolation; one subscription rarely hosts more than one logically separate application environment
- **Resource Groups** — lifecycle and access-control boundaries inside a subscription

Hierarchy isolation enables management-group-scoped Azure Policy assignments, RBAC inheritance, and cleaner audit trails. The CAF enterprise-scale pattern is the de facto reference architecture.

### Centralized identity through Microsoft Entra ID

> **Service-name note:** Microsoft Entra ID is the product formerly known as Azure Active Directory (Azure AD). The rename took effect in June 2023. Documentation, tooling, and PowerShell modules still reference "Azure AD" in many places — treat them as equivalent for SOC 2 evidence purposes.

Federate human access through Microsoft Entra ID with Conditional Access policies enforcing MFA, device compliance, and risk-based controls. For multi-IdP environments, integrate via SAML/OIDC. Use **Microsoft Entra ID Privileged Identity Management (PIM)** for time-bound, just-in-time elevation to privileged roles (Global Administrator, Subscription Owner, Contributor on production resources). This pattern:

- Eliminates standing privileged access
- Brings Azure access into the same account-lifecycle as the rest of your identity ecosystem (offboarding from Entra ID removes Azure access automatically)
- Centralizes access logging in Microsoft Entra ID sign-in and audit logs
- Simplifies access reviews (Entra ID Access Reviews automate quarterly review cycles)

Service principals and managed identities are the appropriate identity for workloads; avoid using user accounts for service-to-service authentication.

### Encryption with Azure Key Vault and Managed HSM

Azure encrypts customer data at rest by default with Microsoft-managed keys. For SOC 2 evidence of customer-controlled cryptography, use Azure Key Vault customer-managed keys (CMK) on sensitive data stores (Azure Storage, Azure SQL, Cosmos DB, Disk encryption). Azure Key Vault Managed HSM provides FIPS 140-2 Level 3 keys for regulated workloads. Soft-delete and purge protection prevent accidental key loss; key rotation, RBAC-based access, and detailed Key Vault diagnostic logs provide auditable encryption-control evidence.

### Logging via Azure Monitor diagnostic settings + Log Analytics workspace

Azure Monitor diagnostic settings capture resource-level activity (every administrative API call, every data-plane operation where enabled) and route them to a Log Analytics workspace, Storage account (for long-term archival), or Event Hub (for SIEM ingestion). Configure diagnostic settings at the management-group level via Azure Policy so every new resource ships logs by default. Pair with the **Microsoft Entra ID** sign-in and audit logs (exported via diagnostic settings to the same workspace).

### Monitoring via Microsoft Defender for Cloud and Microsoft Sentinel

**Microsoft Defender for Cloud** (formerly Azure Security Center / Azure Defender) is Azure's CSPM and CWPP platform. Defender for Cloud's enhanced security plans cover Servers, Storage, SQL, App Service, Containers, Key Vault, and Resource Manager — these provide the detection backbone for the CC7 control family. **Microsoft Sentinel** (formerly Azure Sentinel) is the cloud-native SIEM/SOAR; pair with Log Analytics for incident detection, hunting, and automated response.

### Configuration management via Azure Policy and Azure Resource Graph

Azure Policy enforces configurations across the management-group hierarchy (e.g., `Allowed locations`, `Require encryption at rest with CMK`, `Audit VMs that don't use Managed Disks`). Initiative definitions group related policies; Microsoft publishes a SOC 2-aligned initiative as part of the **Microsoft cloud security benchmark (MCSB)**. Azure Resource Graph queries answer "show me every resource in non-compliant state" for evidence purposes.

> **Service-name note:** Azure Blueprints is **deprecated** as of mid-2026. Microsoft recommends migrating to Template Specs + Azure Policy + Deployment Stacks (or Bicep + Azure Verified Modules). Do not adopt Azure Blueprints for new work.

### Infrastructure as code

Define all production infrastructure in Bicep, ARM templates, Terraform, or Pulumi. Store in source control. Deploy through Azure DevOps Pipelines, GitHub Actions, or another CI/CD platform. This pattern provides:

- Code review trail for infrastructure changes (CC8)
- Reproducible environments (continuity and DR)
- Diff-able evidence of "what changed and when"
- Reduced reliance on Azure Portal clicks, which are hard to evidence

### Network protection via Network Security Groups, Azure Firewall, and Azure Front Door

For network boundaries, **Network Security Groups (NSGs)** provide L4 stateful filtering at the subnet/NIC level. **Azure Firewall** provides centralized stateful network and application-layer filtering with threat intelligence feeds. **Azure DDoS Protection (Standard)** provides DDoS mitigation for protected public IPs. **Azure Front Door** (with WAF policies) handles global L7 load balancing and Web Application Firewall for public-facing applications. **Azure Bastion** provides browser-based SSH/RDP without exposing VMs to the internet.

### Backups via Azure Backup and Azure Site Recovery

**Azure Backup** centralizes backup management across VMs, Azure Files, SQL in VMs, SAP HANA, and PostgreSQL. Recovery Services vault policies, geo-redundant storage, and immutable vaults (with multi-user authorization) provide auditable backup-program evidence. **Azure Site Recovery** handles workload replication and DR orchestration for IaaS and on-premises workloads.

## A realistic minimum Azure configuration for SOC 2 readiness

If you are starting from a minimal Azure footprint, the following provides a reasonable foundation:

1. CAF enterprise-scale management-group hierarchy with at least Platform, Landing Zones (Prod/Non-Prod), and Sandbox tiers
2. Microsoft Entra ID with Conditional Access enforcing MFA on all sign-ins, plus a Block Legacy Authentication policy
3. Microsoft Entra ID PIM for all privileged roles, with approval workflow and 1-hour activation windows for Global Administrator
4. Global Administrator accounts limited to two named individuals with hardware MFA (FIDO2) and break-glass accounts excluded from CA policies, monitored separately
5. Diagnostic settings centrally configured via Azure Policy to ship Activity Logs, Entra ID logs, and resource logs to a Log Analytics workspace in a dedicated subscription
6. Microsoft Defender for Cloud enabled with enhanced security plans (Servers, Storage, Key Vault, SQL, Containers, App Service, Resource Manager) on all subscriptions
7. Microsoft Sentinel deployed on the central Log Analytics workspace with the Microsoft Entra ID, Azure Activity, and Defender for Cloud data connectors enabled
8. Azure Key Vault customer-managed keys for production data stores with soft-delete + purge protection + rotation
9. Azure Policy initiative for the **Microsoft cloud security benchmark (MCSB)** assigned at the tenant root
10. Network Security Groups on all subnets; Azure Firewall in the hub VNet; Azure Bastion for VM administration (no public IPs on VMs)
11. Azure Backup with geo-redundant storage and immutable vaults for production workloads; Azure Site Recovery for critical IaaS workloads
12. Microsoft Defender for Endpoint deployed to all Server VMs (via Defender for Servers integration)
13. Infrastructure-as-code (Bicep or Terraform) for all production resources
14. Azure DevOps Pipelines or GitHub Actions with branch protection and approval gates for deployments

This is the baseline. The detailed table in [controls.md](controls.md) maps each item to specific SOC 2 criteria and notes additional configurations for specific control families.

## On Azure as a subservice organization

In your SOC 2 system description, you will explicitly identify Microsoft Azure as a subservice organization. The standard pattern is:

> *"[Company Name] uses Microsoft Azure as a subservice organization for hosting infrastructure. Microsoft is responsible for the controls supporting physical and environmental security of its data centers, the host hypervisor, the foundational network infrastructure, and the foundational service implementations. [Company Name] is responsible for the controls supporting logical access to and configuration of Azure services in its tenant, management groups, and subscriptions."*

The carve-out is then listed alongside the Complementary User Entity Controls (CUECs) you implement.

## Practical first steps

If you are early in readiness:

1. **Sign in to the [Service Trust Portal](https://servicetrust.microsoft.com/)** so you can download Microsoft's SOC 2 report and review it. This is also where you formally accept terms with Microsoft for receiving the reports.
2. **Set up Microsoft Entra ID Conditional Access + PIM** if you have not already. The before-and-after on standing privileged access is dramatic.
3. **Turn on Microsoft Defender for Cloud and Sentinel** across all subscriptions. The cost is meaningful for Defender enhanced plans, but the audit benefit is large.
4. **Adopt the CAF enterprise-scale management-group hierarchy** if you are still on a flat subscription model. The migration is meaningful work but the resulting auditability is worth it.
5. **Walk the [controls.md](controls.md) table** with your engineering team. Mark which rows you already satisfy, which need work, and which do not apply.

See also: the [root cloud-mapping README](../README.md) for cross-provider guidance, and the [project README](../../README.md) for how this fits into a full SOC 2 readiness program.

---

When you have implemented the controls described here, you will have addressed a large fraction of the Azure-specific work that SOC 2 requires. The remaining work is the *human* controls — access reviews, incident response, training, vendor management — which are governed by the policies in [/policy-templates/](../../policy-templates/).

*Generated and maintained by SimpleAudit — https://simpleaudit.io*
