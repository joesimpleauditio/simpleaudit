# SOC 2 Common Criteria → GCP Service Mapping

A detailed mapping from each SOC 2 Common Criterion to Google Cloud services and the configurations that satisfy the criterion. The "Configuration Notes" column captures the specific settings auditors will want to see evidence of.

This table is opinionated — it assumes the architectural patterns described in the [section README](README.md): Organization → Folder → Project hierarchy, Cloud Identity / Workforce Identity Federation, centralized logging via aggregated sinks, infrastructure-as-code, VPC Service Controls perimeters around sensitive data. If your architecture differs, adapt the mappings accordingly.

> **Service-name note:** Google occasionally renames or restructures services. Cloud Data Loss Prevention is now **Sensitive Data Protection**. G Suite is now **Google Workspace**. Stackdriver was split into Cloud Logging, Cloud Monitoring, and Cloud Trace several years ago — current architecture should reference the split services, not Stackdriver. Where a rename has occurred, the current name is used with a parenthetical reference to the prior name on first mention.

---

## CC1 — Control Environment

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC1.1** | Commitment to integrity and ethical values | Compliance Reports Manager, Active Assist (Recommender) | Not implemented in GCP per se — documented in policies. Recommender's IAM and security insights can support evidence of operational discipline. |
| **CC1.2** | Board / leadership oversight | Resource Manager (Organization node), Cloud Billing | Organization node and billing accounts restricted; leadership receives security findings via Security Command Center notifications or Looker Studio dashboards. |
| **CC1.3** | Organizational structure & reporting lines | Cloud Identity / Workforce Identity Federation, Resource Manager Folders | IAM bindings and group structure reflect organizational structure; Folders reflect business or environment boundaries. Document the mapping. |
| **CC1.4** | Demonstrates commitment to competence | Cloud Identity, Google Cloud Skills Boost | Personnel with privileged GCP access have documented training records. Google Cloud Skills Boost certificates and learning paths can be evidence. |
| **CC1.5** | Holds individuals accountable | Cloud Audit Logs, Policy Analyzer | Every privileged action is attributable to a specific principal via Cloud Audit Logs Admin Activity logs. Access reviews tied to identity-provider records. |

## CC2 — Communication and Information

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC2.1** | Quality of information | Cloud Asset Inventory, Security Command Center, Cloud Audit Logs | Cloud Asset Inventory records resource state with change history; SCC aggregates findings across detectors; Cloud Audit Logs is the canonical audit log. Together they provide quality-controlled operational data. |
| **CC2.2** | Internal communication of objectives | Cloud Pub/Sub, Google Workspace, Cloud Monitoring alerting channels | Channels for internal alerts and notifications (paging, security email distribution lists) are documented and managed. |
| **CC2.3** | External communication | Cloud Pub/Sub, third-party email (SendGrid, Mailgun on GCP), Cloud DNS for status pages | Customer-facing communication channels for security disclosures, incident notifications. Status page often runs on Cloud Storage + Cloud CDN or third-party (Statuspage, Better Stack). |

## CC3 — Risk Assessment

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC3.1** | Specifies suitable objectives | Google Cloud Architecture Framework, Security Command Center (Compliance Posture), Risk Manager | Architecture Framework reviews documented for material workloads. SCC Compliance Posture is GA and maps controls to SOC 2 / ISO / CIS frameworks. Risk Manager (Private Preview, US-only, prioritized for SCC Premium customers — verify availability for your account) provides risk reports for compliance attestations. |
| **CC3.2** | Identifies risks | Security Command Center, Sensitive Data Protection (formerly Cloud DLP), Web Security Scanner | Continuous findings from these services feed the risk register. SCC Premium includes Event Threat Detection and Virtual Machine Threat Detection. |
| **CC3.3** | Considers fraud risk | Cloud Audit Logs, Event Threat Detection (SCC Premium), Policy Analyzer | Detection rules for anomalous IAM actions, unusual API patterns, service account key abuse. |
| **CC3.4** | Identifies and assesses changes | Organization Policy Service, Cloud Asset Inventory change history, Recommender | Organization Policy constraints prevent drift from desired state; Asset Inventory queries answer "what changed and when"; Recommender surfaces risk-relevant configuration drift. |

## CC4 — Monitoring Activities

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC4.1** | Selects and develops ongoing evaluations | Security Command Center, Cloud Asset Inventory, Assured Workloads (for regulated tenants) | SCC Posture Management continuously evaluates compliance against built-in or custom benchmarks. Assured Workloads enforces regulated-tenant constraints (e.g., FedRAMP, HIPAA workloads). |
| **CC4.2** | Evaluates and communicates deficiencies | Security Command Center, Cloud Pub/Sub, Cloud Monitoring alerting | Findings route to Pub/Sub topics, SIEM, or ticketing via SCC notifications. Aging findings escalated through alerting policies. |

## CC5 — Control Activities

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC5.1** | Selects and develops control activities | Organization Policy Service, Security Command Center Posture Management | Organization Policies encode control sets (e.g., `constraints/storage.uniformBucketLevelAccess`); Posture deployments enforce guardrails at the organization/folder/project level. |
| **CC5.2** | Selects and develops technology controls | All GCP services as appropriate | Specific controls mapped throughout this table. |
| **CC5.3** | Deploys controls through policies and procedures | Organization Policy Service, IAM Conditions, VPC Service Controls | Organization Policies define the boundary of what projects may do (restrict allowed regions, block default network creation, deny external IP on Compute Engine, require CMEK). IAM Conditions add attribute-based constraints. |

## CC6 — Logical and Physical Access Controls

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC6.1** | Logical and physical access controls | Cloud Identity, Workforce Identity Federation, BeyondCorp Enterprise, IAM, Cloud KMS, VPC, Firewall Rules, VPC Service Controls | Cloud Identity (or federated IdP) used for all human access, MFA enforced via context-aware access. Service accounts replace default identities and have key creation disabled where possible. Super Admin credentials restricted to break-glass with hardware MFA. Network access controlled via VPC firewall rules, Cloud NGFW, and VPC Service Controls perimeters. Encryption keys managed in Cloud KMS or Cloud HSM. |
| **CC6.2** | User authorization and accountability | Cloud Identity, IAM, Cloud Audit Logs | New access via documented request → approval → IAM role binding (preferably via group membership). Every action attributable via Cloud Audit Logs Admin Activity logs. |
| **CC6.3** | Management of access through role changes and termination | Cloud Identity, SCIM provisioning from corporate IdP, Workforce Identity Federation | SCIM from corporate IdP automatically disables GCP access when the IdP account is disabled. Quarterly reviews of IAM bindings and group memberships using Policy Analyzer queries. |
| **CC6.4** | Physical access restrictions | Google data centers (subservice) | Carve-out: Google operates physical security. Your CUEC: do not transport or store data on physical media outside Google-managed facilities. |
| **CC6.5** | Information disposal | Cloud Storage Object Lifecycle, Cloud KMS key destruction, BigQuery table expiration | Lifecycle rules delete data per retention policy; Cloud KMS key destruction provides cryptographic erasure for CMEK-protected data; BigQuery table and partition expiration configured per dataset. |
| **CC6.6** | Restriction of access from outside the system boundary | VPC firewall rules, Cloud Armor, BeyondCorp Enterprise, Identity-Aware Proxy (IAP), VPC Service Controls, Private Service Connect | Production administrative interfaces fronted by IAP or BeyondCorp Enterprise — no direct internet exposure. Cloud Armor rules on public-facing HTTP(S) Load Balancers. Private Service Connect / VPC peering for service-to-service traffic where appropriate. |
| **CC6.7** | Restriction of information transmission | TLS, Cloud KMS, Cloud Storage bucket policies, VPC Service Controls egress rules | All transit encrypted (TLS 1.2+). Cloud Storage bucket policies enforce HTTPS. VPC-SC egress rules restrict exfiltration to approved destinations. Data exports controlled and logged. |
| **CC6.8** | Prevention of unauthorized or malicious software | Container Threat Detection (SCC Premium), Artifact Registry vulnerability scanning, Binary Authorization, OS patch management | Compute Engine and container images scanned. Artifact Registry scans on push and continuously. Binary Authorization enforces signed images on GKE. VM Manager handles patch management for Compute Engine fleets. |

## CC7 — System Operations

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC7.1** | Detection of vulnerabilities | Artifact Registry scanning, Web Security Scanner, Security Command Center, VM Manager OS patch insights | Artifact Registry covers container images on push and ongoing. Web Security Scanner crawls App Engine, Compute Engine, and GKE-hosted web apps. SCC aggregates. |
| **CC7.2** | Monitoring for anomalies | Event Threat Detection (SCC Premium), Cloud Audit Logs, VPC Flow Logs, Cloud Monitoring | Event Threat Detection identifies threats from Cloud Audit Logs and VPC Flow. Cloud Monitoring alerting policies on baseline operational metrics. |
| **CC7.3** | Evaluation of security events | Security Command Center → Cloud Pub/Sub → SIEM or ticketing | Routing rules from Pub/Sub filter findings into appropriate response queues. SCC integrates natively with Chronicle, Splunk, and others. |
| **CC7.4** | Incident response | Cloud Monitoring alerting, Cloud Pub/Sub, third-party paging (PagerDuty) | Findings page on-call rotation; incident channel opened; runbook executed. |
| **CC7.5** | Recovery from incidents | Cloud Storage versioning, Persistent Disk snapshots, Cloud SQL automated backups, Cloud Spanner backups, Backup and DR Service | Recovery procedures documented per system; Backup and DR Service enables consistent restore across GCE, Cloud SQL, and on-prem workloads; Terraform enables environment recreation. |

## CC8 — Change Management

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC8.1** | Authorized changes to infrastructure, data, software | Cloud Build, Secure Source Manager, Artifact Registry, Binary Authorization, IAM | Branch protection in source-control (GitHub, GitLab, or Secure Source Manager) requires reviews. *Cloud Source Repositories reached End of Sale on June 17, 2024 — existing customers only; new environments should use Secure Source Manager, GitHub, or GitLab.* Cloud Build pipelines require commit references. Binary Authorization enforces that only signed builds deploy to production GKE. IAM restricts production deploy permissions to pipeline service accounts and break-glass roles. Manual Cloud Console changes logged via Cloud Audit Logs and reconciled to IaC. |

## CC9 — Risk Mitigation

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **CC9.1** | Risk mitigation activities for disruption | Backup and DR Service, multi-zone and multi-region deployments, Cloud Load Balancing health checks, Cloud DNS routing policies | Critical workloads run across at least two zones (or two regions for the most critical). Backup and DR Service cross-region copy enabled. Cloud DNS failover routing documented. |
| **CC9.2** | Vendor and business partner risk | Compliance Reports Manager, IAM (for vendor access), Workforce Identity Federation | Vendor access scoped through IAM roles with time-bound credentials and conditional bindings. Compliance Reports Manager subscription provides Google's own SOC 2 attestation. |

## Availability (A1)

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **A1.1** | Capacity management | Cloud Monitoring, Active Assist Recommender, quota dashboards in Cloud Console | Capacity monitored continuously; Managed Instance Groups and GKE Autoscaler configured for elastic workloads. Quota dashboards reviewed; quota increases requested in advance of foreseen growth. |
| **A1.2** | Backup and recovery infrastructure | Backup and DR Service, Cloud SQL automated backups, Persistent Disk snapshots, Cloud Storage versioning + lifecycle, Cloud Spanner backups | Backup schedules align with documented RPO. Cross-region copy enabled for critical data. Bucket Lock for immutability where required. |
| **A1.3** | Tests recovery plan procedures | Backup and DR Service restore jobs, custom restore runbooks, GKE backup restore drills | Annual restoration test documented with evidence (restore job ID, validated row counts, application smoke test). DR exercise covers at least one critical system per year end-to-end. |

## Confidentiality (C1)

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **C1.1** | Protection of confidential information | Cloud KMS, Cloud HSM, Cloud Storage default encryption + bucket policies, Cloud SQL CMEK, Persistent Disk CMEK, Secret Manager, Sensitive Data Protection, IAM Recommender | Encryption at rest using Cloud KMS CMEK for all production data stores; Cloud HSM for FIPS 140-2 Level 3 workloads. Bucket policies enforce uniform bucket-level access and HTTPS. Secrets stored in Secret Manager with rotation. Sensitive Data Protection scans data stores for personal data classifications. |
| **C1.2** | Disposal of confidential information | Cloud KMS key destruction, Cloud Storage lifecycle + versioning, BigQuery table/partition expiration, Backup and DR retention | Cryptographic erasure via Cloud KMS key destruction. Lifecycle policies delete data per retention. Disposal events logged via Cloud Audit Logs. |

## Processing Integrity (PI1)

PI controls are highly application-specific. GCP provides the building blocks; the application implements the logic.

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **PI1.1–PI1.5** | Definitions, input/processing/output integrity, completeness | Cloud Endpoints / API Gateway (input validation), Cloud Pub/Sub (idempotent processing patterns), Workflows / Cloud Composer (workflow tracing), Cloud Monitoring custom metrics, Firestore / Spanner transactions, Cloud Storage object versioning | API Gateway enforces request schemas (OpenAPI/gRPC). Pub/Sub message deduplication + idempotent handlers prevent duplicates. Workflows provides end-to-end execution traceability. |

## Privacy (P1–P8)

Privacy controls cross-cut application logic, legal terms, and operational practice. GCP supports privacy controls; it does not implement them on your behalf.

| SOC 2 Criterion | Description | GCP Services | Configuration Notes |
|---|---|---|---|
| **P1 — Notice and communication of objectives** | Privacy notice provided to data subjects | Documented in policies; Cloud DNS / Cloud CDN serves privacy notice on customer-facing properties | Not directly applicable — operated at the policy and application layer. |
| **P2 — Choice and consent** | Consent captured and honored | Application-layer; Firestore / Cloud SQL stores consent records | Not directly applicable — operated at the application layer. |
| **P3 — Collection** | Only necessary personal data collected | Sensitive Data Protection discovers and classifies inadvertently collected data; IAM restricts who can query personal data | Sensitive Data Protection scans Cloud Storage and BigQuery for personal data. |
| **P4 — Use, retention, and disposal** | Personal data used only as authorized, retained only as needed | Cloud Storage lifecycle policies, BigQuery table expiration, Cloud KMS key destruction, Cloud Audit Logs Data Access | Lifecycle and expiration policies enforce retention limits. Data Access logs evidence who accessed personal data. |
| **P5 — Access** | Data subjects can access, correct, or delete their data | Application-layer; Cloud Audit Logs Data Access supports request fulfillment evidence | Documented in policies; GCP provides supporting evidence via Data Access logs. |
| **P6 — Disclosure to third parties** | Disclosures controlled and tracked | VPC Service Controls (egress rules), Cloud Audit Logs, Sensitive Data Protection de-identification | VPC-SC egress rules restrict exfiltration to approved destinations. SDP de-identification (tokenization, masking) for third-party data sharing. |
| **P7 — Quality** | Personal data is accurate and complete | Application-layer; BigQuery data quality jobs | Not directly applicable — operated at the application layer. |
| **P8 — Monitoring and enforcement** | Privacy program monitored and enforced | Security Command Center, Cloud Audit Logs Data Access, Sensitive Data Protection continuous discovery | SCC findings surface privacy-relevant misconfigurations (e.g., publicly accessible buckets containing personal data). |

---

## Service-by-service quick reference

For teams setting up from scratch, the following minimum service set covers a large fraction of the table above:

| GCP Service | Primary purpose for SOC 2 |
|---|---|
| **Resource Manager (Organization + Folders)** | Multi-project structure under a unified policy boundary |
| **Cloud Identity / Workforce Identity Federation** | Federated human access with MFA |
| **IAM + IAM Conditions** | Role bindings, attribute-based constraints |
| **Cloud KMS / Cloud HSM** | Encryption keys with rotation and IAM-based access |
| **Cloud Audit Logs** | Admin Activity, Data Access, System Event logs (CC1.5, CC6.x, CC7.x, CC8.1) |
| **Cloud Asset Inventory** | Configuration recording with change history (CC4, CC5) |
| **Security Command Center (Premium)** | Threat detection + posture management (CC3, CC4, CC7) |
| **Sensitive Data Protection (formerly Cloud DLP)** | Personal data discovery and de-identification (P3, P6) |
| **Backup and DR Service** | Centralized backup with retention and immutability (A1.2, A1.3) |
| **VM Manager** | Patching and OS inventory (CC6.x, CC7.5) |
| **Cloud Armor + Cloud Load Balancing** | Public-facing protection (CC6.6, A1.1) |
| **VPC + Firewall Rules + VPC Service Controls** | Network and data-plane boundaries (CC6.6, P6) |
| **Secret Manager** | Secret storage with rotation (CC6.1, CC6.7) |
| **Binary Authorization** | Signed-image enforcement on GKE (CC8.1) |
| **Cloud Monitoring + Cloud Logging** | Operational metrics, alerting, log routing (CC7.x) |
| **Compliance Reports Manager** | Receive Google attestation reports (CC9.2) |
| **Risk Manager** | Risk reports for compliance attestations — *Private Preview, US-only, SCC Premium prioritized* (CC3.1) |

---

## How to use this table during readiness

1. **Walk top to bottom**, criterion by criterion, against your environment.
2. **For each row, capture three things:**
   - Is the service enabled?
   - Is it configured per the notes?
   - Where is the evidence?
3. **Track gaps** in your readiness checklist with target dates.
4. **Capture configurations in IaC** (Terraform is the GCP community standard) so the evidence is reviewable, versioned, and reproducible.
5. **Confirm with your auditor** which carve-outs (Google managed-service operations) and CUECs (your responsibilities) will appear in the system description.

If a row does not apply to your environment, mark it not applicable with a written justification. "We do not use GKE" is acceptable; "We did not implement this" is not.

---

*Mapping current as of the date of this document; Google Cloud releases new services and renames services regularly. Validate service names and capabilities with Google Cloud documentation at audit time.*

*Generated and maintained by SimpleAudit — https://simpleaudit.io*
