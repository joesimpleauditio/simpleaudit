# SOC 2 Common Criteria → AWS Service Mapping

A detailed mapping from each SOC 2 Common Criterion to AWS services and the configurations that satisfy the criterion. The "Configuration Notes" column captures the specific settings auditors will want to see evidence of.

This table is opinionated — it assumes the architectural patterns described in the [section README](README.md): multi-account structure under Organizations, IAM Identity Center federation, centralized logging in an audit account, infrastructure-as-code. If your architecture differs, adapt the mappings accordingly.

> **Service-name note:** AWS occasionally renames services (AWS SSO → IAM Identity Center). Where a rename has occurred, the current name is used with a parenthetical reference to the old name on first mention.

---

## CC1 — Control Environment

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC1.1** | AWS Artifact, AWS Trusted Advisor | Not implemented in AWS per se — documented in policies. AWS Trusted Advisor's security checks can support evidence of operational discipline. |
| **CC1.2** | AWS Organizations, Cost Explorer | Management account is restricted; leadership receives security findings via Security Hub email reports or aggregated dashboards. |
| **CC1.3** | IAM Identity Center, Organizations OUs | Permission sets reflect organizational structure; OUs reflect business or environment boundaries. Document the mapping. |
| **CC1.4** | IAM Identity Center, AWS Training and Certification | Personnel with privileged AWS access have documented training records. AWS Skill Builder records can be evidence. |
| **CC1.5** | CloudTrail, IAM Access Analyzer | Every privileged action is attributable to a specific principal via CloudTrail. Access reviews tied to identity-provider records. |

## CC2 — Communication and Information

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC2.1** | AWS Config, Security Hub, CloudTrail | Config records resource state; Security Hub aggregates findings; CloudTrail is the canonical audit log. Together they provide quality-controlled operational data. |
| **CC2.2** | Amazon SES, Amazon SNS | Channels for internal alerts and notifications (paging, security email distribution lists) are documented and managed. |
| **CC2.3** | Amazon SES, Amazon Pinpoint | Customer-facing communication channels for security disclosures, incident notifications. Status page often runs on CloudFront/S3 or third-party. |

## CC3 — Risk Assessment

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC3.1** | AWS Well-Architected Framework, AWS Trusted Advisor | Well-Architected Reviews documented for material workloads. |
| **CC3.2** | Security Hub, GuardDuty, Inspector, IAM Access Analyzer | Continuous findings from these services feed the risk register. |
| **CC3.3** | CloudTrail, IAM Access Analyzer, GuardDuty | Detection rules for anomalous IAM actions, unusual API patterns, external access. |
| **CC3.4** | AWS Config Rules, AWS Trusted Advisor | Config evaluates drift from desired state; rules trigger when a resource changes in a way that crosses risk thresholds. |

## CC4 — Monitoring Activities

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC4.1** | AWS Config, AWS Audit Manager, Security Hub | Config Rules continuously evaluate compliance. Audit Manager produces evidence packages mapped to SOC 2. |
| **CC4.2** | Security Hub, Amazon EventBridge, Amazon SNS | Findings route to SNS topics, SIEM, or ticketing. Aging findings escalated. |

## CC5 — Control Activities

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC5.1** | AWS Config Conformance Packs, Service Control Policies (SCPs) | Conformance Packs encode control sets; SCPs enforce guardrails at the OU/account level. |
| **CC5.2** | All AWS services as appropriate | Specific controls mapped throughout this table. |
| **CC5.3** | AWS Organizations, Service Control Policies | SCPs define the boundary of what accounts may do (deny destructive actions, deny disabling logs, deny non-approved regions). |

## CC6 — Logical and Physical Access Controls

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC6.1** | IAM Identity Center, IAM, MFA, KMS, VPC, Security Groups | IAM Identity Center federated to corporate IdP, MFA enforced for all access. Standalone IAM users limited to break-glass; root credentials offline with hardware MFA. Network access controlled via VPCs, security groups, NACLs, AWS Network Firewall as needed. Encryption keys managed in KMS. |
| **CC6.2** | IAM Identity Center, IAM, CloudTrail | New access via documented request → approval → permission set assignment. Every action attributable via CloudTrail. |
| **CC6.3** | IAM Identity Center, SCIM provisioning | SCIM from corporate IdP automatically disables AWS access when the IdP account is disabled. Quarterly reviews of permission set assignments and group memberships. |
| **CC6.4** | AWS data centers (subservice) | Carve-out: AWS operates physical security. Your CUEC: do not transport or store data on physical media outside AWS-managed facilities. |
| **CC6.5** | S3 Lifecycle, KMS key destruction, AWS Backup retention | Lifecycle rules delete data per retention policy; KMS key destruction provides cryptographic erasure where supported; backup vault retention configured. |
| **CC6.6** | Security Groups, NACLs, AWS WAF, AWS Shield, VPC endpoints, IAM policies | Production administrative interfaces not internet-exposed (Session Manager, VPN, or zero-trust access). WAF rules on public-facing endpoints. VPC endpoints for AWS service traffic where appropriate. |
| **CC6.7** | TLS, KMS, S3 SSL-required bucket policies, VPC endpoints | All transit encrypted (TLS 1.2+). Bucket policies require `aws:SecureTransport=true`. Data exports controlled and logged. |
| **CC6.8** | AWS Inspector, GuardDuty, AWS Systems Manager Patch Manager | EC2 and container images scanned. Patch Manager schedules patching. ECR image scanning enabled. |

## CC7 — System Operations

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC7.1** | Amazon Inspector, ECR scanning, Security Hub, AWS Trusted Advisor | Inspector covers EC2, Lambda, and container images. ECR scans images on push and continuously. Security Hub aggregates. |
| **CC7.2** | GuardDuty, CloudTrail Insights, AWS Config, VPC Flow Logs, CloudWatch Anomaly Detection | GuardDuty detects threats from CloudTrail, VPC Flow, DNS. CloudWatch alarms on baseline operational metrics. |
| **CC7.3** | Security Hub, EventBridge → SIEM or ticketing | Routing rules from EventBridge filter findings into appropriate response queues. |
| **CC7.4** | AWS Health, EventBridge, SNS, third-party paging (PagerDuty) | Findings page on-call rotation; incident channel opened; runbook executed. |
| **CC7.5** | AWS Backup, AWS Elastic Disaster Recovery, RDS snapshots, AWS CloudFormation | Recovery procedures documented per system; AWS Backup vault enables consistent restore; CloudFormation enables environment recreation. |

## CC8 — Change Management

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC8.1** | AWS CodePipeline / CodeBuild / CodeDeploy, Infrastructure-as-code (CloudFormation, CDK, Terraform), IAM | Branch protection in source-control system (GitHub, CodeCommit) requires reviews. Pipeline executions require commit references. IAM policies restrict production deploy permissions to pipelines and break-glass roles. Manual cloud-console changes prevented or logged via CloudTrail and reconciled to IaC. |

## CC9 — Risk Mitigation

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **CC9.1** | AWS Backup, multi-AZ / multi-region architectures, AWS Elastic Disaster Recovery, Route 53 health checks | Critical workloads run across at least two availability zones (or two regions for the most critical). Backup vault cross-region copy enabled. Route 53 failover routing documented. |
| **CC9.2** | AWS Artifact, IAM (for vendor access) | Vendor access scoped through IAM roles with `external-id` and time-bound credentials. AWS Artifact subscription provides AWS's own SOC 2 attestation. |

## Availability (A1)

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **A1.1** | CloudWatch, Compute Optimizer, AWS Cost Explorer, AWS Trusted Advisor service limits | Capacity monitored continuously; auto-scaling configured for elastic workloads. Service-quota dashboards reviewed. |
| **A1.2** | AWS Backup, RDS automated backups, EBS snapshots, S3 versioning + lifecycle, AWS Elastic Disaster Recovery | Backup schedules align with documented RPO. Cross-region copy enabled for critical data. Vault Lock for immutability where required. |
| **A1.3** | AWS Backup restore jobs, AWS Elastic Disaster Recovery drills, custom restore runbooks | Annual restoration test documented with evidence (restore job ID, validated row counts, application smoke test). DR exercise covers at least one critical system per year end-to-end. |

## Confidentiality (C1)

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **C1.1** | KMS, S3 default encryption + bucket policies, RDS encryption, EBS encryption, Secrets Manager, IAM Access Analyzer | Encryption at rest using KMS CMKs for all production data stores. Bucket policies block unencrypted PutObject. Secrets stored in Secrets Manager (or Systems Manager Parameter Store with KMS). |
| **C1.2** | KMS key deletion, S3 lifecycle + versioning + deletion, AWS Backup vault deletion | Cryptographic erasure via KMS key destruction. Lifecycle policies delete data per retention. Disposal events logged via CloudTrail. |

## Processing Integrity (PI1)

PI controls are highly application-specific. AWS provides the building blocks; the application implements the logic.

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **PI1.1–PI1.5** | API Gateway (input validation), Amazon SQS / SNS (idempotent processing patterns), Step Functions (workflow tracing), CloudWatch metrics, DynamoDB conditional writes, S3 versioning | API Gateway enforces request schemas. SQS FIFO + idempotent handlers prevent duplicates. Step Functions provides end-to-end execution traceability. |

## Privacy (P1–P8)

Privacy controls cross-cut application logic, legal terms, and operational practice. AWS supports privacy controls; it does not implement them on your behalf.

| SOC 2 Criterion | AWS Services | Configuration Notes |
|---|---|---|
| **P1–P8** | Amazon Macie, KMS, Lake Formation, IAM, CloudTrail | Macie discovers and classifies sensitive data in S3. KMS encrypts personal data; access controls restrict to specific roles. CloudTrail logs access to facilitate data-subject request fulfillment. Lake Formation provides fine-grained access for analytics workloads. |

---

## Service-by-service quick reference

For teams setting up from scratch, the following minimum service set covers a large fraction of the table above:

| AWS Service | Primary purpose for SOC 2 |
|---|---|
| **AWS Organizations** | Multi-account structure, SCPs for guardrails |
| **IAM Identity Center** | Federated human access with MFA |
| **IAM** | Service roles, policies; minimal human users |
| **KMS** | Encryption keys with rotation and access policies |
| **CloudTrail** | Management-plane audit log (CC1.5, CC6.x, CC7.x, CC8.1) |
| **AWS Config** | Configuration recording and rule-based evaluation (CC4, CC5) |
| **GuardDuty** | Threat detection (CC7.1, CC7.2) |
| **Security Hub** | Findings aggregation (CC3.2, CC4) |
| **Amazon Inspector** | Vulnerability scanning of EC2, Lambda, containers (CC7.1) |
| **AWS Backup** | Centralized backup with retention and immutability (A1.2, A1.3) |
| **AWS Systems Manager** | Patching, session management, parameter store (CC6.x, CC7.5) |
| **AWS WAF + Shield + CloudFront** | Public-facing protection (CC6.6, A1.1) |
| **VPC + Security Groups + NACLs** | Network boundaries (CC6.6) |
| **Secrets Manager** | Secret storage with rotation (CC6.1, CC6.7) |
| **IAM Access Analyzer** | Detect external access (CC6.6) |
| **CloudWatch + EventBridge** | Operational metrics, alerting, event routing (CC7.x) |
| **AWS Artifact** | Receive AWS attestation reports (CC9.2) |
| **AWS Audit Manager** | Evidence collection mapped to frameworks (CC4) |

---

## How to use this table during readiness

1. **Walk top to bottom**, criterion by criterion, against your environment.
2. **For each row, capture three things:**
   - Is the service enabled?
   - Is it configured per the notes?
   - Where is the evidence?
3. **Track gaps** in your readiness checklist with target dates.
4. **Capture configurations in IaC** so the evidence is reviewable, versioned, and reproducible.
5. **Confirm with your auditor** which carve-outs (AWS managed-service operations) and CUECs (your responsibilities) will appear in the system description.

If a row does not apply to your environment, mark it not applicable with a written justification. "We do not use AWS Lambda" is acceptable; "We did not implement this" is not.

---

*Mapping current as of the date of this document; AWS releases new services and renames services regularly. Validate service names and capabilities with AWS documentation at audit time.*

Generated and maintained by SimpleAudit — https://simpleaudit.io
