# Cloud Mapping

Translations from SOC 2 Common Criteria to concrete cloud-provider configurations.

## Why this section exists

Policies say things like "logical access controls shall be enforced." Auditors expect to see those controls *operating*, which means specific service configurations, specific IAM policies, specific KMS keys, specific logging destinations. The gap between "we have an access control policy" and "show me the configuration that enforces it" is where many readiness assessments stall.

This section translates the policy language into provider-specific configurations. Use it to:

1. Implement the controls (the policy says "MFA enforced for production access"; the cloud mapping shows you which AWS Organizations / IAM Identity Center / IAM settings make that true).
2. Document the implementation in your SOC 2 system description.
3. Produce evidence the auditor will sample (the configuration screenshots or terraform state showing the setting is in place).

## What's covered

- [**aws/**](aws/) — AWS-to-SOC 2 mapping. A pillar-style reference plus a detailed mapping table covering Common Criteria CC1 through CC9, Availability, Confidentiality, Processing Integrity, and Privacy.
- [**gcp/**](gcp/) — Google Cloud-to-SOC 2 mapping. Covers Cloud Identity, IAM, Cloud KMS, Cloud Audit Logs, Security Command Center, VPC Service Controls, and the rest of the GCP service set across all Common Criteria and Additional Categories.
- [**azure/**](azure/) — Microsoft Azure-to-SOC 2 mapping. Covers Microsoft Entra ID, Conditional Access, PIM, Azure Policy, Microsoft Defender for Cloud, Microsoft Sentinel, Key Vault, and the rest of the Azure service set across all Common Criteria and Additional Categories.
- [**cloudflare/**](cloudflare/) — Cloudflare-to-SOC 2 mapping. Covers Cloudflare Zero Trust (Access, Gateway, Tunnel), WAF, DDoS Protection, API Shield, Workers, and Logpush. Cloudflare is an edge/network/Zero Trust platform — many SOC 2 criteria are operated in the IdP, application, or primary IaaS rather than in Cloudflare; the table makes this explicit.

Mappings for additional providers may be added over time. Contributions welcome.

## How to use this section

1. **Read the section README** for the provider you use. It explains the shared-responsibility model and where the cloud provider's SOC 2 picks up vs. where yours does.
2. **Walk the controls.md table** alongside your readiness checklist. Each row maps a SOC 2 criterion to specific services and configurations.
3. **Implement what is missing.** Where your environment lacks a control, treat that as a gap on the readiness checklist.
4. **Document the implementation** — typically in your system description and as configuration captured in infrastructure-as-code.

## A note on the shared responsibility model

Major cloud providers (AWS, Azure, Google Cloud) operate their own SOC 2-certified infrastructure. As a customer, you inherit infrastructure-tier controls — but you do not inherit the controls you must implement on top of that infrastructure.

For example: AWS's SOC 2 demonstrates that AWS protects its data centers and EBS volumes with strong access controls and encryption capabilities. Your SOC 2 must demonstrate that *you used* those capabilities (you turned on encryption at rest; you used IAM correctly; you separated production from non-production). The cloud provider gives you the controls; you operate them.

In SOC 2 terminology, the cloud provider is a **subservice organization**, and the controls you must operate are **Complementary User Entity Controls (CUECs)**. Your auditor will identify the carve-out explicitly in your system description.

The mappings in this section focus exclusively on the controls *you* are responsible for operating.

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
