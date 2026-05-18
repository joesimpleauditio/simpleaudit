# AWS-to-SOC 2 Mapping

How AWS services support the SOC 2 Trust Services Criteria, and which configurations satisfy specific criteria. This document is the pillar reference; the detailed control-by-service table lives in [controls.md](controls.md).

## Why specific service mapping matters

Telling your auditor "we use AWS, which is SOC 2 compliant" does not help you. AWS's SOC 2 attests to AWS's controls — physical security, infrastructure protections, foundational service capabilities — and not to *your* use of AWS. Your SOC 2 must demonstrate the controls *you* operate on top of AWS.

The granularity matters because auditors test specifics, not categories. They will not ask "do you have access controls?" They will ask "show me how production AWS console access is authenticated, who has root access to the management account, and how console-access activity is logged and reviewed." The answer is a series of specific service configurations — IAM Identity Center policies, Organizations service control policies, CloudTrail trails, GuardDuty findings — not the phrase "we use AWS securely."

This document and the linked table map each SOC 2 Common Criterion to the AWS services and configurations that satisfy it. Use them as a checklist when implementing controls and as a script when discussing your environment with the auditor.

## Shared responsibility — what AWS does, what you must do

AWS publishes a [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) that splits security responsibilities between AWS and the customer. The high-level division:

- **AWS is responsible for security *of* the cloud** — physical data centers, the host hypervisor, the network fabric, the foundational service implementations, and the operational controls that keep those running. AWS's SOC 2 attests to these.
- **You are responsible for security *in* the cloud** — IAM users and policies, network configuration (VPC, security groups, NACLs), data encryption configuration, OS-level patching (for EC2), logging configuration, monitoring, application-level controls. Your SOC 2 attests to these.

The split varies by service. For managed services (RDS, Lambda, DynamoDB) AWS covers more — they patch the runtimes, they manage the underlying compute. For IaaS-style services (EC2) you cover more — you patch the OS, you manage the kernel.

In your SOC 2 system description, AWS will appear as a **subservice organization**, "carved out" of your report. The auditor will note that AWS's SOC 2 is reviewed and that AWS's controls support your environment. The auditor will not test AWS's controls; the auditor will test yours.

## Inheriting AWS's controls

You inherit AWS's controls by:

1. **Reviewing AWS's SOC 2 report at least annually.** Available through AWS Artifact (`https://aws.amazon.com/artifact/`). The latest SOC 2 Type 2 covers a 12-month period; review it for exceptions relevant to your environment.
2. **Subscribing to AWS Security Hub findings, AWS Health alerts, and security bulletins** to be aware of operational issues that affect your controls.
3. **Documenting AWS as a subservice organization** in your system description, including the Trust Services Categories AWS's SOC 2 covers and the carve-out methodology.
4. **Defining Complementary User Entity Controls (CUECs)** — controls *you* must operate for AWS's controls to be effective. Examples: configuring encryption keys, restricting console access, enabling logging, applying patches to your EC2 instances.

## Where AWS Artifact fits

[AWS Artifact](https://aws.amazon.com/artifact/) is AWS's portal for compliance reports. From it, you can download:

- AWS SOC 1, SOC 2, and SOC 3 reports
- ISO certificates
- PCI DSS Attestation of Compliance
- Industry-specific attestations

For SOC 2 readiness, the SOC 2 Type 2 report is the primary document. The SOC 3 report is a public-facing summary of the SOC 2 — useful for customer-facing trust pages, not for audit work.

## Architectural patterns that support SOC 2

Several AWS architectural patterns substantially simplify SOC 2 compliance. They are not strictly required, but they are common in SOC 2-compliant SaaS environments:

### Multi-account structure with AWS Organizations

A single AWS account is rarely sufficient for SOC 2. The accepted pattern is a multi-account structure:

- **Management account** — billing and organization-level configuration only; no production workloads
- **Production accounts** — one or more, hosting customer-facing workloads
- **Non-production accounts** — development, staging, sandbox; separate from production
- **Audit / log archive account** — central destination for CloudTrail, Config, and other audit data; write-once where feasible
- **Shared services account** — identity, networking hubs, security tooling

Multi-account isolation provides natural blast-radius limits, simplifies access controls, and produces cleaner audit trails.

### Centralized identity through IAM Identity Center (formerly AWS SSO)

Federate AWS access from your corporate identity provider (Okta, Microsoft Entra ID, Google Workspace) into IAM Identity Center, then assign permission sets to users and groups. This pattern:

- Eliminates standalone IAM users for human access
- Brings AWS access into the same account-lifecycle as the rest of your identity ecosystem (offboarding from Okta removes AWS access automatically)
- Centralizes access logging
- Simplifies access reviews

Standalone IAM users should be limited to break-glass accounts and a small number of service-account purposes.

### Encryption with KMS

Use AWS KMS for encryption at rest with customer-managed keys for sensitive workloads. KMS key policies, automatic rotation, and grant-based access provide auditable encryption-control evidence.

### Logging via CloudTrail to a separate audit account

CloudTrail captures management-plane activity (every API call) and, with data events enabled selectively, data-plane activity (S3 object access, DynamoDB calls). Ship logs to an immutable destination in a separate audit account, with retention aligned to your retention policy.

### Monitoring via GuardDuty, CloudTrail insights, and Security Hub

GuardDuty provides threat detection across CloudTrail, VPC Flow Logs, and DNS logs. Security Hub aggregates findings from GuardDuty, AWS Config, Macie, Inspector, and third-party tools into a single view. CloudTrail Insights detects unusual activity patterns. Together they provide the detection backbone for the CC7 control family.

### Configuration management via AWS Config

AWS Config records resource configurations over time and evaluates them against rules. This provides:

- Continuous CSPM-style monitoring
- Evidence of configuration state at any point in time (useful for audits)
- Drift detection when a resource changes outside infrastructure-as-code

### Infrastructure as code

Define all production infrastructure in CloudFormation, Terraform, Pulumi, or CDK. Store in source control. Deploy through CI/CD. This pattern provides:

- Code review trail for infrastructure changes (CC8)
- Reproducible environments (continuity and DR)
- Diff-able evidence of "what changed and when"
- Reduced reliance on ClickOps, which is hard to evidence

### Backups via AWS Backup

AWS Backup centralizes backup management across RDS, DynamoDB, EBS, EFS, S3 (via lifecycle policies), and more. Backup vault policies, cross-region copy, and immutability settings (Vault Lock) provide auditable backup-program evidence.

### Web protection via AWS WAF, Shield, and CloudFront

For public-facing applications, WAF rules, Shield Advanced (where applicable), and CloudFront provide DDoS mitigation, geoblocking, and managed rule sets that satisfy several of the resilience and protection controls in CC6 and the Availability category.

## A realistic minimum AWS configuration for SOC 2 readiness

If you are starting from a minimal AWS footprint, the following provides a reasonable foundation:

1. AWS Organizations with at least production, non-production, and audit accounts
2. IAM Identity Center federated to your corporate IdP, MFA enforced
3. Root account credentials stored offline, hardware MFA on the management account root, root usage alerted
4. CloudTrail enabled across all accounts, logs centralized in the audit account with object-lock retention
5. AWS Config enabled in all accounts, recording all resource types, findings aggregated
6. GuardDuty enabled in all regions of all accounts
7. Security Hub enabled with AWS Foundational Security Best Practices standard
8. KMS customer-managed keys for production data stores with rotation enabled
9. S3 bucket public-access block at the account level for all accounts
10. VPC Flow Logs enabled for production VPCs
11. AWS Backup with cross-region copy for critical RDS, DynamoDB, and EBS
12. IAM Access Analyzer enabled to detect external access
13. CloudWatch alarms on root account usage, IAM policy changes, security group changes, and other sensitive operations
14. Infrastructure-as-code (Terraform, CloudFormation) for all production resources

This is the baseline. The detailed table in [controls.md](controls.md) maps each item to specific SOC 2 criteria and notes additional configurations for specific control families.

## On AWS as a subservice organization

In your SOC 2 system description, you will explicitly identify AWS as a subservice organization. The standard pattern is:

> *"[Company Name] uses Amazon Web Services (AWS) as a subservice organization for hosting infrastructure. AWS is responsible for the controls supporting physical and environmental security of its data centers, the host hypervisor, the foundational network infrastructure, and the foundational service implementations. [Company Name] is responsible for the controls supporting logical access to and configuration of AWS services in its accounts."*

The carve-out is then listed alongside the Complementary User Entity Controls (CUECs) you implement.

## Practical first steps

If you are early in readiness:

1. **Enable AWS Artifact access** so you can download the AWS SOC 2 report and review it. This is also where you formally accept terms with AWS for receiving the reports.
2. **Set up IAM Identity Center** federation if you have not already. The before-and-after on access reviews is dramatic.
3. **Turn on CloudTrail, Config, and GuardDuty** in every region of every account. The cost is minimal; the audit benefit is large.
4. **Adopt the multi-account structure** if you are still on a single account. The migration is meaningful work but the resulting auditability is worth it.
5. **Walk the [controls.md](controls.md) table** with your engineering team. Mark which rows you already satisfy, which need work, and which do not apply.

---

When you have implemented the controls described here, you will have addressed a large fraction of the AWS-specific work that SOC 2 requires. The remaining work is the *human* controls — access reviews, incident response, training, vendor management — which are governed by the policies in [/policy-templates/](../../policy-templates/).

Generated and maintained by SimpleAudit — https://simpleaudit.io
