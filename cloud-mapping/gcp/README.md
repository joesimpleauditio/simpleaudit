# GCP-to-SOC 2 Mapping

How Google Cloud services support the SOC 2 Trust Services Criteria, and which configurations satisfy specific criteria. This document is the pillar reference; the detailed control-by-service table lives in [controls.md](controls.md).

## Why specific service mapping matters

Telling your auditor "we use Google Cloud, which is SOC 2 compliant" does not help you. Google Cloud's SOC 2 attests to Google's controls — physical security, infrastructure protections, foundational service capabilities — and not to *your* use of Google Cloud. Your SOC 2 must demonstrate the controls *you* operate on top of GCP.

The granularity matters because auditors test specifics, not categories. They will not ask "do you have access controls?" They will ask "show me how production console access is authenticated, who has Organization Admin in your GCP organization, and how console-access activity is logged and reviewed." The answer is a series of specific service configurations — Cloud Identity user lifecycle, Organization Policy constraints, Cloud Audit Logs sinks, Security Command Center findings — not the phrase "we use Google Cloud securely."

This document and the linked table map each SOC 2 Common Criterion to the GCP services and configurations that satisfy it. Use them as a checklist when implementing controls and as a script when discussing your environment with the auditor.

## Shared responsibility — what Google Cloud does, what you must do

Google publishes a [Shared Responsibility and Shared Fate model](https://cloud.google.com/architecture/framework/security/shared-responsibility-shared-fate) that splits security responsibilities between Google and the customer. Google frames the split slightly differently from AWS — Google emphasizes "shared fate," meaning Google provides opinionated secure defaults, blueprints, and security posture services rather than purely handing off responsibility at a boundary. The practical division still applies:

- **Google is responsible for security *of* the cloud** — physical data centers, the hardware, the host kernel, the foundational network fabric (including the global private backbone), the underlying service implementations. Google's SOC 2 attests to these.
- **You are responsible for security *in* the cloud** — Cloud Identity user lifecycle, IAM bindings, Organization Policy constraints, network configuration (VPC, firewall rules, VPC Service Controls perimeters), encryption-key management (Cloud KMS, Cloud HSM, or external KMS), OS-level patching for Compute Engine, logging configuration, monitoring, application-level controls.

The split varies by service. For serverless and managed services (Cloud Run, Cloud Functions, BigQuery, Spanner, GKE Autopilot) Google covers more — runtime patching, the underlying compute, the storage layer. For IaaS-style services (Compute Engine, self-managed GKE Standard) you cover more — you patch the OS, you manage node pools.

In your SOC 2 system description, Google Cloud will appear as a **subservice organization**, "carved out" of your report. The auditor will note that Google's SOC 2 is reviewed and that Google's controls support your environment. The auditor will not test Google's controls; the auditor will test yours.

## Inheriting Google Cloud's controls

You inherit Google's controls by:

1. **Reviewing Google Cloud's SOC 2 report at least annually.** Available through the [Compliance Reports Manager](https://cloud.google.com/security/compliance/compliance-reports-manager). The latest SOC 2 Type 2 covers a 12-month period; review it for exceptions relevant to your in-scope services.
2. **Subscribing to Google Cloud security bulletins, Security Command Center notifications, and Status Dashboard alerts** to be aware of operational issues that affect your controls.
3. **Documenting Google Cloud as a subservice organization** in your system description, including the Trust Services Categories Google's SOC 2 covers and the carve-out methodology.
4. **Defining Complementary User Entity Controls (CUECs)** — controls *you* must operate for Google's controls to be effective. Examples specific to GCP: enabling Cloud Audit Logs (Admin Activity logs are on by default, but Data Access logs are not), configuring Organization Policy constraints, enabling customer-managed encryption keys (CMEK) where required, restricting use of default service accounts, applying patches to Compute Engine instances you operate.

## Where the Compliance Reports Manager fits

The [Compliance Reports Manager](https://cloud.google.com/security/compliance/compliance-reports-manager) is Google Cloud's portal for compliance reports. From it, you can download:

- Google Cloud SOC 1, SOC 2, and SOC 3 reports
- ISO 27001, 27017, 27018, and 27701 certificates
- PCI DSS Attestation of Compliance
- HIPAA implementation guidance and BAA-related materials
- FedRAMP, CSA STAR, and other regional/industry-specific attestations

For SOC 2 readiness, the SOC 2 Type 2 report is the primary document. The SOC 3 report is a public-facing summary — useful for customer-facing trust pages, not for audit work.

Access requires signing in with a Google account associated with your organization's billing or organization node, and accepting Google's confidentiality terms before downloading restricted reports.

## Architectural patterns that support SOC 2

Several GCP architectural patterns substantially simplify SOC 2 compliance. They are not strictly required, but they are common in SOC 2-compliant SaaS environments running on Google Cloud:

### Resource hierarchy with Organization, Folders, and Projects

A single GCP project is rarely sufficient for SOC 2. The accepted pattern is a layered hierarchy:

- **Organization** — top-level node tied to your Cloud Identity or Google Workspace domain; holds organization-wide IAM and Organization Policies
- **Folders** — environment or business-unit boundaries (e.g., `production`, `non-production`, `shared-services`, `audit`)
- **Projects** — workload-level isolation; one project rarely hosts more than one logically separate application

Hierarchy isolation provides natural blast-radius limits, enables environment-scoped Organization Policies, and produces cleaner audit trails. Permissions inherit downward — bind broadly at folder level for environment-wide controls and narrowly at project level for least-privilege.

### Centralized identity through Cloud Identity + Workforce Identity Federation

Federate human access from your corporate identity provider (Okta, Microsoft Entra ID, Google Workspace itself) into Cloud Identity or Workforce Identity Federation, then assign IAM roles to groups. This pattern:

- Eliminates standalone Google accounts for human GCP access
- Brings GCP access into the same account-lifecycle as the rest of your identity ecosystem (offboarding removes GCP access automatically via SCIM)
- Centralizes access logging in Cloud Audit Logs and the IdP
- Simplifies access reviews

Service accounts are the appropriate identity for workloads; default Compute Engine and App Engine service accounts should be disabled and replaced with purpose-specific service accounts wherever possible.

### Encryption with Cloud KMS and Cloud HSM

GCP encrypts customer data at rest by default with Google-managed keys. For SOC 2 evidence of customer-controlled cryptography, use Cloud KMS customer-managed encryption keys (CMEK) on sensitive data stores (Cloud Storage, BigQuery, Cloud SQL, Persistent Disk). Cloud HSM provides FIPS 140-2 Level 3 keys for regulated workloads. Key rotation, IAM-based access policies, and detailed audit logs on key use provide auditable encryption-control evidence.

### Logging via Cloud Audit Logs + aggregated sinks

Cloud Audit Logs captures Admin Activity (every administrative API call, on by default), Data Access (read/write access to user data, requires explicit enabling), System Events (Google-initiated), and Policy Denied logs. Configure aggregated log sinks at the organization level to ship audit logs to a dedicated logging project — typically into a Cloud Storage bucket with retention lock and/or a BigQuery dataset for query-based investigations.

### Monitoring via Security Command Center, Cloud Monitoring, and Cloud Logging

Security Command Center (SCC) is GCP's CSPM and threat-detection platform. SCC Premium adds Event Threat Detection, Container Threat Detection, and Virtual Machine Threat Detection — these cover the detection backbone for the CC7 control family. Cloud Monitoring handles operational metrics and alerting; Cloud Logging handles log ingestion, retention, and routing. Stackdriver was split into these three services years ago — do not reference Stackdriver in current architecture.

### Configuration management via Organization Policy Service and Cloud Asset Inventory

Organization Policy Service enforces constraints across the resource hierarchy (e.g., restrict which regions can host resources, require uniform bucket-level access, block default service account creation). Cloud Asset Inventory records resource configurations and supports query and change-history APIs — useful for audits and drift detection.

### Infrastructure as code

Define all production infrastructure in Terraform, Pulumi, or Cloud Deployment Manager (legacy). Store in source control. Deploy through Cloud Build, GitHub Actions, or another CI/CD platform. This pattern provides:

- Code review trail for infrastructure changes (CC8)
- Reproducible environments (continuity and DR)
- Diff-able evidence of "what changed and when"
- Reduced reliance on Cloud Console clicks, which are hard to evidence

### Perimeter protection via VPC Service Controls

VPC Service Controls (VPC-SC) defines security perimeters around GCP API surfaces (Cloud Storage, BigQuery, Pub/Sub, and many others). Inside a perimeter, services can call each other freely; calls crossing the perimeter require explicit Ingress/Egress rules. This is the closest equivalent to AWS's account-boundary model and is the recommended pattern for protecting sensitive data stores from data exfiltration.

### Edge protection via Cloud Armor and Cloud Load Balancing

For public-facing applications, Cloud Armor provides WAF policies, OWASP rule sets, and DDoS protection (Cloud Armor Standard is included with HTTP(S) Load Balancing; Cloud Armor Managed Protection Plus adds advanced features). Combined with global Cloud Load Balancing, this satisfies several of the resilience and protection controls in CC6 and the Availability category.

## A realistic minimum GCP configuration for SOC 2 readiness

If you are starting from a minimal GCP footprint, the following provides a reasonable foundation:

1. Organization node tied to Cloud Identity with at least production, non-production, audit, and shared-services folders
2. Cloud Identity (or federation from your corporate IdP) with MFA enforced via context-aware access policies
3. Super Admin accounts limited to two named individuals with hardware MFA and offline recovery codes
4. Cloud Audit Logs enabled across the organization, Data Access logs enabled for sensitive services, logs centralized via aggregated sink to a dedicated logging project
5. Security Command Center Premium enabled at the organization level with Event Threat Detection and Container Threat Detection active
6. Cloud KMS customer-managed keys (CMEK) for production data stores with automatic rotation enabled
7. Organization Policy constraints applied: restrict resource locations, require OS Login, disable default service account key creation, enforce uniform bucket-level access
8. VPC Service Controls perimeter around production data services (BigQuery, Cloud Storage, etc.)
9. Cloud Asset Inventory exports configured for change-history evidence
10. Binary Authorization enabled for GKE workloads requiring signed container images
11. Cloud Armor policies on all public-facing HTTP(S) Load Balancers
12. Recommender + IAM Recommender reviewed quarterly for least-privilege adjustments
13. Infrastructure-as-code (Terraform) for all production resources
14. Cloud Build or external CI/CD with signed provenance for deployments

This is the baseline. The detailed table in [controls.md](controls.md) maps each item to specific SOC 2 criteria and notes additional configurations for specific control families.

## On Google Cloud as a subservice organization

In your SOC 2 system description, you will explicitly identify Google Cloud as a subservice organization. The standard pattern is:

> *"[Company Name] uses Google Cloud Platform (GCP) as a subservice organization for hosting infrastructure. Google Cloud is responsible for the controls supporting physical and environmental security of its data centers, the host hypervisor, the foundational network infrastructure, and the foundational service implementations. [Company Name] is responsible for the controls supporting logical access to and configuration of Google Cloud services in its organization, folders, and projects."*

The carve-out is then listed alongside the Complementary User Entity Controls (CUECs) you implement.

## Practical first steps

If you are early in readiness:

1. **Sign in to the [Compliance Reports Manager](https://cloud.google.com/security/compliance/compliance-reports-manager)** so you can download Google Cloud's SOC 2 report and review it. This is also where you formally accept terms with Google for receiving the reports.
2. **Set up Workforce Identity Federation or Cloud Identity federation** if you have not already. The before-and-after on access reviews is dramatic.
3. **Turn on Cloud Audit Logs (including Data Access) and Security Command Center Premium** at the organization level. The cost is meaningful for SCC Premium but the audit benefit is large.
4. **Adopt the Organization → Folder → Project hierarchy** if you are still on a single project. The migration is meaningful work but the resulting auditability is worth it.
5. **Walk the [controls.md](controls.md) table** with your engineering team. Mark which rows you already satisfy, which need work, and which do not apply.

See also: the [root cloud-mapping README](../README.md) for cross-provider guidance, and the [project README](../../README.md) for how this fits into a full SOC 2 readiness program.

---

When you have implemented the controls described here, you will have addressed a large fraction of the GCP-specific work that SOC 2 requires. The remaining work is the *human* controls — access reviews, incident response, training, vendor management — which are governed by the policies in [/policy-templates/](../../policy-templates/).

*Generated and maintained by SimpleAudit — https://simpleaudit.io*
