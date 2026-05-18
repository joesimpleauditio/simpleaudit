# SOC 2 Common Criteria → Azure Service Mapping

A detailed mapping from each SOC 2 Common Criterion to Microsoft Azure services and the configurations that satisfy the criterion. The "Configuration Notes" column captures the specific settings auditors will want to see evidence of.

This table is opinionated — it assumes the architectural patterns described in the [section README](README.md): CAF enterprise-scale management-group hierarchy, Microsoft Entra ID with Conditional Access + PIM, centralized logging in a dedicated Log Analytics workspace, infrastructure-as-code, Azure Policy enforcement of the Microsoft cloud security benchmark. If your architecture differs, adapt the mappings accordingly.

> **Service-name note:** Microsoft has renamed several core Azure services in the past few years. The current names — used throughout this table — and their previous names:
> - **Microsoft Entra ID** (formerly Azure Active Directory / Azure AD; renamed June 2023)
> - **Microsoft Defender for Cloud** (formerly Azure Security Center + Azure Defender)
> - **Microsoft Sentinel** (formerly Azure Sentinel)
> - **Microsoft Entra External ID** (the modern CIAM successor to Azure Active Directory B2C; B2C continues to operate for existing tenants but new work should target External ID)
> - **Microsoft Purview** (unifies what were previously Azure Information Protection, Azure Purview data governance, and the Microsoft 365 compliance solutions under a single brand)
> - **Azure Update Manager** (formerly Update Management in Azure Automation)
>
> **Azure Blueprints is deprecated** as of mid-2026 — migrate to Template Specs + Azure Policy + Deployment Stacks (or Bicep + Azure Verified Modules). Do not adopt Blueprints for new work.

---

## CC1 — Control Environment

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC1.1** | Service Trust Portal, Microsoft Purview Compliance Manager | Not implemented in Azure per se — documented in policies. Purview Compliance Manager tracks tenant-wide compliance posture against frameworks including SOC 2. |
| **CC1.2** | Azure Cost Management, Microsoft Entra ID, Defender for Cloud Secure Score | Tenant root and billing scopes restricted; leadership receives security findings via Defender for Cloud workbooks or Power BI dashboards. |
| **CC1.3** | Microsoft Entra ID, Management Groups | Entra ID groups and management-group structure reflect organizational structure; subscriptions reflect environment or business boundaries. Document the mapping. |
| **CC1.4** | Microsoft Entra ID, Microsoft Learn (training records) | Personnel with privileged Azure access have documented training records. Microsoft Learn / Applied Skills records can be evidence. |
| **CC1.5** | Azure Activity Log, Microsoft Entra ID Audit Logs, PIM audit history | Every privileged action is attributable to a specific principal via Activity Log and Entra ID Audit Logs. PIM activation history evidences just-in-time elevation. Access reviews tied to identity-provider records. |

## CC2 — Communication and Information

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC2.1** | Azure Resource Graph, Defender for Cloud, Activity Log + Log Analytics | Resource Graph queries resource state at any point in time; Defender for Cloud aggregates security findings; Activity Log is the canonical audit log. Together they provide quality-controlled operational data. |
| **CC2.2** | Azure Communication Services, Action Groups, Microsoft Teams integration | Channels for internal alerts and notifications (paging, security email distribution lists) are documented and managed. Action Groups route Azure Monitor alerts to email, SMS, webhook, ITSM. |
| **CC2.3** | Azure Communication Services, Azure Front Door (for status pages) | Customer-facing communication channels for security disclosures, incident notifications. Status page often runs on Azure Static Web Apps + Front Door or third-party (Statuspage, Better Stack). |

## CC3 — Risk Assessment

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC3.1** | Azure Well-Architected Framework, Microsoft cloud security benchmark (MCSB) | Well-Architected Reviews documented for material workloads. MCSB initiative assigned at tenant root. |
| **CC3.2** | Microsoft Defender for Cloud, Defender for Endpoint, Defender for Servers, Microsoft Sentinel | Continuous findings from Defender plans feed the risk register. Sentinel analytics rules detect threats across the tenant. |
| **CC3.3** | Microsoft Entra ID Identity Protection, Microsoft Sentinel UEBA, Defender for Cloud Apps | Identity Protection detects risky sign-ins and risky users. Sentinel UEBA profiles user/entity behavior to detect anomalies. |
| **CC3.4** | Azure Policy compliance, Activity Log, Azure Resource Graph change-history queries | Azure Policy evaluates drift from desired state. Resource Graph + Activity Log answer "what changed and when." |

## CC4 — Monitoring Activities

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC4.1** | Azure Policy, Microsoft Purview Compliance Manager, Defender for Cloud regulatory compliance dashboard | Azure Policy continuously evaluates compliance against MCSB and custom initiatives. Compliance Manager dashboards track SOC 2 control coverage. Defender for Cloud regulatory compliance dashboard maps findings to SOC 2 controls directly. |
| **CC4.2** | Defender for Cloud, Azure Monitor Action Groups, Microsoft Sentinel automation rules | Findings route to Action Groups, SIEM, or ITSM via Sentinel playbooks. Aging recommendations escalated through Defender for Cloud workflow automation. |

## CC5 — Control Activities

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC5.1** | Azure Policy initiatives, Microsoft cloud security benchmark | Policy initiatives encode control sets; MCSB initiative covers a large fraction of SOC 2 technical controls. |
| **CC5.2** | All Azure services as appropriate | Specific controls mapped throughout this table. |
| **CC5.3** | Azure Policy (deny effect), Management Groups, RBAC | Policy assignments with `deny` effect define the boundary of what subscriptions may do (deny non-approved regions, deny public storage accounts, deny VMs without managed disks). |

## CC6 — Logical and Physical Access Controls

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC6.1** | Microsoft Entra ID, Conditional Access, Microsoft Entra ID PIM, Azure RBAC, Azure Key Vault, VNets, NSGs, Azure Firewall | Microsoft Entra ID with Conditional Access enforcing MFA on all sign-ins. PIM provides just-in-time elevation for privileged roles. Break-glass accounts limited to two, with hardware MFA and offline credentials. Azure RBAC assigned via groups, not directly to users. Network access controlled via VNets, NSGs, Azure Firewall. Encryption keys managed in Key Vault. |
| **CC6.2** | Microsoft Entra ID, Conditional Access, Azure RBAC, Activity Log | New access via documented request → approval → group membership → role assignment (preferably PIM-eligible, not active). Every action attributable via Activity Log and Entra ID Audit Logs. |
| **CC6.3** | Microsoft Entra ID, SCIM provisioning, Entra ID Access Reviews | SCIM provisioning from corporate IdP (or Entra ID as primary IdP) automatically disables Azure access when the user is offboarded. Quarterly Entra ID Access Reviews for privileged roles and group memberships. |
| **CC6.4** | Microsoft data centers (subservice) | Carve-out: Microsoft operates physical security. Your CUEC: do not transport or store data on physical media outside Microsoft-managed facilities. |
| **CC6.5** | Azure Storage lifecycle management, Azure Key Vault key destruction, Azure Backup retention | Lifecycle rules delete blobs per retention policy. Key Vault key destruction provides cryptographic erasure (with soft-delete + purge protection awareness). Backup retention policies configured per workload. |
| **CC6.6** | NSGs, Azure Firewall, Azure DDoS Protection, Azure Front Door + WAF, Azure Bastion, Private Endpoints, Service Endpoints | Production administrative interfaces fronted by Azure Bastion or Entra Application Proxy — no direct internet exposure on VMs. WAF policies on Azure Front Door / Application Gateway for public-facing endpoints. Private Endpoints for PaaS data services to keep traffic on the Microsoft backbone. |
| **CC6.7** | TLS, Azure Key Vault, Storage `Secure transfer required` setting, Private Endpoints | All transit encrypted (TLS 1.2+). Storage accounts enforce `Secure transfer required`. Private Endpoints restrict data-plane traffic to the VNet. Data exports controlled and logged via diagnostic settings. |
| **CC6.8** | Microsoft Defender for Servers (with Defender for Endpoint), Defender for Containers, Azure Update Manager, Microsoft Defender for Cloud Apps | VMs scanned by Defender for Endpoint (deployed via Defender for Servers Plan 2). Container images scanned by Defender for Containers (registry + runtime). Azure Update Manager handles OS patch deployment for IaaS VMs. |

## CC7 — System Operations

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC7.1** | Microsoft Defender Vulnerability Management (integrated with Defender for Servers / Defender for Containers), Defender for SQL, Defender for App Service | MDVM covers VMs, containers, and software inventory. Defender for SQL covers vulnerability assessment of Azure SQL. Defender for App Service detects suspicious behavior on App Service plans. |
| **CC7.2** | Microsoft Sentinel, Defender for Cloud, Microsoft Entra ID Identity Protection, Azure Monitor alerts | Sentinel analytics rules detect threats from Activity Log, Entra ID logs, Defender alerts. Identity Protection flags risky sign-ins. Azure Monitor alerts on baseline operational metrics. |
| **CC7.3** | Microsoft Sentinel, Defender for Cloud → Sentinel/ITSM integration | Sentinel incidents triage and assign to response queues. Defender for Cloud workflow automation routes recommendations to ITSM. |
| **CC7.4** | Azure Monitor Action Groups, Microsoft Sentinel automation rules, third-party paging (PagerDuty) | Findings page on-call rotation via Action Groups or Sentinel playbooks; incident channel opened; runbook executed. |
| **CC7.5** | Azure Backup, Azure Site Recovery, Azure SQL automated backups, Cosmos DB continuous backup, Bicep / ARM templates | Recovery procedures documented per system; Azure Backup vault enables consistent restore with immutable vaults; Azure Site Recovery enables warm-DR for IaaS; Bicep enables environment recreation. |

## CC8 — Change Management

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC8.1** | Azure DevOps Pipelines / GitHub Actions, Azure Container Registry, Bicep / ARM Templates / Terraform, Azure RBAC, Deployment Stacks | Branch protection in source-control (Azure Repos, GitHub) requires reviews. Pipeline executions require commit references and approver gates. Azure RBAC restricts production deploy permissions to service principals and break-glass roles. Deployment Stacks (or Azure Verified Modules) provide governed deployment with deny-settings. Manual portal changes logged via Activity Log and reconciled to IaC. |

## CC9 — Risk Mitigation

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **CC9.1** | Azure Backup, Azure Site Recovery, Availability Zones, Paired Regions, Azure Front Door / Traffic Manager | Critical workloads run across at least two Availability Zones (or two regions for the most critical). Azure Backup geo-redundant storage enabled. Traffic Manager / Front Door failover routing documented. |
| **CC9.2** | Service Trust Portal, Microsoft Entra ID B2B (for vendor access), Microsoft Entra ID Entitlement Management | Vendor access scoped through Entra ID B2B guest accounts with Conditional Access and time-bound access packages via Entitlement Management. Service Trust Portal subscription provides Microsoft's own SOC 2 attestation. |

## Availability (A1)

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **A1.1** | Azure Monitor, Azure Advisor, Azure Subscription quotas dashboard | Capacity monitored continuously; VMSS / App Service / AKS autoscaling configured for elastic workloads. Subscription quota dashboards reviewed; quota increases requested in advance. |
| **A1.2** | Azure Backup, Azure SQL automated backups, Cosmos DB continuous backup, Storage geo-redundant replication, Azure Site Recovery | Backup schedules align with documented RPO. Geo-redundant storage and cross-region backup copy enabled for critical data. Immutable vaults with multi-user authorization for ransomware protection. |
| **A1.3** | Azure Backup restore jobs, Azure Site Recovery DR drills, custom restore runbooks | Annual restoration test documented with evidence (restore job ID, validated row counts, application smoke test). ASR test failover exercises documented. DR exercise covers at least one critical system per year end-to-end. |

## Confidentiality (C1)

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **C1.1** | Azure Key Vault, Managed HSM, Storage encryption + CMK, Azure SQL TDE with CMK, Disk encryption with CMK, Azure Confidential Computing, Microsoft Purview Information Protection | Encryption at rest using customer-managed keys (CMK) in Key Vault for all production data stores; Managed HSM for FIPS 140-2 Level 3 workloads. Storage accounts enforce `Secure transfer required` and minimum TLS 1.2. Confidential Computing for memory-encrypted workloads where required. Purview Information Protection labels sensitive data. |
| **C1.2** | Azure Key Vault key destruction, Storage lifecycle management, Azure Backup retention | Cryptographic erasure via Key Vault key destruction (with awareness of soft-delete + purge protection windows). Lifecycle policies delete blobs per retention. Disposal events logged via diagnostic settings. |

## Processing Integrity (PI1)

PI controls are highly application-specific. Azure provides the building blocks; the application implements the logic.

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **PI1.1–PI1.5** | Azure API Management (input validation), Azure Service Bus (idempotent processing patterns), Azure Logic Apps / Durable Functions (workflow tracing), Application Insights, Cosmos DB conditional writes, Storage blob versioning | API Management enforces request schemas (OpenAPI). Service Bus sessions + idempotent handlers prevent duplicates. Durable Functions provides end-to-end execution traceability. Application Insights tracks request success rates and end-to-end transactions. |

## Privacy (P1–P8)

Privacy controls cross-cut application logic, legal terms, and operational practice. Azure supports privacy controls; it does not implement them on your behalf.

| SOC 2 Criterion | Azure Services | Configuration Notes |
|---|---|---|
| **P1 — Notice and communication of objectives** | Documented in policies; Azure Front Door serves privacy notice on customer-facing properties | Not directly applicable — operated at the policy and application layer. |
| **P2 — Choice and consent** | Application-layer; Azure SQL / Cosmos DB stores consent records | Not directly applicable — operated at the application layer. |
| **P3 — Collection** | Microsoft Purview Data Map, Microsoft Purview Information Protection labels, Defender for Cloud Apps DLP | Purview discovers and classifies personal data across Azure data sources. Information Protection labels enforce handling rules. |
| **P4 — Use, retention, and disposal** | Azure Storage lifecycle management, Cosmos DB TTL, Azure SQL retention policies, Key Vault key destruction, Activity Log + Diagnostic Logs | Lifecycle and TTL policies enforce retention limits. Diagnostic logs evidence who accessed personal data. |
| **P5 — Access** | Application-layer; Microsoft Purview eDiscovery (for Microsoft 365 data); diagnostic logs evidence DSR fulfillment | Documented in policies; Azure provides supporting evidence via diagnostic logs. Purview eDiscovery supports DSAR fulfillment for Microsoft 365 content. |
| **P6 — Disclosure to third parties** | Private Endpoints, NSG egress rules, Azure Firewall egress filtering, Purview de-identification | Private Endpoints + Azure Firewall restrict egress to approved destinations. Purview de-identification (where applicable) for third-party data sharing. |
| **P7 — Quality** | Application-layer; Azure Data Factory / Synapse data quality jobs | Not directly applicable — operated at the application layer. |
| **P8 — Monitoring and enforcement** | Microsoft Purview Compliance Manager, Defender for Cloud Apps, Activity Log | Compliance Manager tracks privacy-control coverage. Defender for Cloud Apps surfaces SaaS data-handling risks. |

---

## Service-by-service quick reference

For teams setting up from scratch, the following minimum service set covers a large fraction of the table above:

| Azure Service | Primary purpose for SOC 2 |
|---|---|
| **Management Groups + Subscriptions** | Multi-subscription structure under a unified policy boundary |
| **Microsoft Entra ID + Conditional Access** | Federated human access with MFA |
| **Microsoft Entra ID PIM** | Just-in-time privileged elevation |
| **Azure RBAC** | Role assignments via groups, scoped to MG/Sub/RG |
| **Azure Key Vault / Managed HSM** | Encryption keys with rotation and RBAC-based access |
| **Activity Log + Diagnostic Settings → Log Analytics** | Centralized audit logging (CC1.5, CC6.x, CC7.x, CC8.1) |
| **Azure Policy + MCSB initiative** | Configuration enforcement and compliance evaluation (CC4, CC5) |
| **Microsoft Defender for Cloud** | CSPM + CWPP threat detection (CC3, CC4, CC7) |
| **Microsoft Sentinel** | SIEM + SOAR (CC7.2, CC7.3, CC7.4) |
| **Microsoft Defender Vulnerability Management** | Vulnerability scanning of VMs, containers, software inventory (CC7.1) |
| **Azure Backup + Azure Site Recovery** | Centralized backup with immutability + DR (A1.2, A1.3) |
| **Azure Update Manager** | Patching for IaaS VMs (CC6.8) |
| **Azure Front Door + WAF / Application Gateway + WAF** | Public-facing protection (CC6.6, A1.1) |
| **VNets + NSGs + Azure Firewall + Private Endpoints** | Network and data-plane boundaries (CC6.6) |
| **Azure Bastion** | VM administration without public IPs (CC6.1, CC6.6) |
| **Microsoft Purview** | Data discovery, classification, governance, privacy (C1, P3-P8) |
| **Service Trust Portal** | Receive Microsoft attestation reports (CC9.2) |
| **Azure Monitor + Action Groups** | Operational metrics, alerting, event routing (CC7.x) |

---

## How to use this table during readiness

1. **Walk top to bottom**, criterion by criterion, against your environment.
2. **For each row, capture three things:**
   - Is the service enabled?
   - Is it configured per the notes?
   - Where is the evidence?
3. **Track gaps** in your readiness checklist with target dates.
4. **Capture configurations in IaC** (Bicep or Terraform) so the evidence is reviewable, versioned, and reproducible.
5. **Confirm with your auditor** which carve-outs (Microsoft managed-service operations) and CUECs (your responsibilities) will appear in the system description.

If a row does not apply to your environment, mark it not applicable with a written justification. "We do not use Azure Kubernetes Service" is acceptable; "We did not implement this" is not.

---

*Mapping current as of the date of this document; Microsoft releases new Azure services and renames services regularly. Validate service names and capabilities with Microsoft Learn documentation at audit time.*

*Generated and maintained by SimpleAudit — https://simpleaudit.io*
