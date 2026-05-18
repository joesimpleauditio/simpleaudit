# SimpleAudit Open Source — SOC 2 Resources

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A curated, opinionated set of SOC 2 compliance resources for startups and small engineering teams. Everything in this repository is free to use, modify, and redistribute under the [MIT License](LICENSE).

## Who this is for

If you are a founder, engineering lead, or operations owner at a small or mid-sized company facing your first SOC 2 audit — and you do not have a dedicated compliance team — this repository is for you.

SOC 2 is a controls-based attestation. The auditor does not test your code; they test whether your *organization* runs a coherent program that satisfies the Trust Services Criteria (TSC). That program needs written policies, repeatable processes, and evidence that the processes actually ran. Most teams approaching their first audit spend more time figuring out *what is expected* than actually doing the work. These resources collapse that gap.

The materials are written for the most common SOC 2 Type 2 starting point: a SaaS company running on a major cloud provider, with a small engineering team, no on-premises infrastructure, and no existing ISO 27001 program. If your environment looks different, the templates are a starting point — adapt them, do not rubber-stamp them. Auditors notice copy-paste policies that do not match how the company actually operates.

## What's in this repository

| Section | What it is | When to use it |
|---|---|---|
| [policy-templates/](policy-templates/) | Twelve generic SOC 2 policy templates in Markdown | When you need the *written* policy artifact your auditor will sample |
| [readiness-checklist/](readiness-checklist/) | A 100+ item checklist grouped by Trust Services Criteria | When you want to know *what work is left* before audit fieldwork |
| [evidence-collection-guide/](evidence-collection-guide/) | A guide to the evidence auditors actually sample, plus three concrete templates | When you are about to enter the observation window and need to start producing artifacts |
| [cloud-mapping/aws/](cloud-mapping/aws/) | A detailed mapping from SOC 2 Common Criteria to AWS services and configurations | When you need to translate "logical access controls" into specific IAM, SSO, and KMS settings |
| [cloud-mapping/gcp/](cloud-mapping/gcp/) | A detailed mapping from SOC 2 Common Criteria to Google Cloud services and configurations | When you need to translate SOC 2 controls into Cloud Identity, IAM, Cloud KMS, and Security Command Center settings |
| [cloud-mapping/azure/](cloud-mapping/azure/) | A detailed mapping from SOC 2 Common Criteria to Microsoft Azure services and configurations | When you need to translate SOC 2 controls into Microsoft Entra ID, Azure RBAC, Key Vault, and Defender for Cloud settings |
| [cloud-mapping/cloudflare/](cloud-mapping/cloudflare/) | A detailed mapping from SOC 2 Common Criteria to Cloudflare services and configurations | When Cloudflare fronts your edge/network/Zero Trust layer and you need to evidence WAF, Access, and Logpush configurations |

Each subdirectory has its own `README.md` with section-specific guidance. Start there.

## How to use this repository

1. **Read the [readiness checklist](readiness-checklist/README.md) first.** It is the fastest way to map your current state against what the audit will examine. You will almost certainly find gaps, and that is the point — the checklist tells you where to start.
2. **Adapt the policy templates** to your organization. Replace `[Company Name]` and similar placeholder fields, then take a careful read of every section. If a clause does not match how you operate, change it; do not leave it in to look thorough. An auditor who finds a policy claim that contradicts the evidence will flag a control deficiency.
3. **Implement the cloud configurations** for the provider(s) you use — [AWS](cloud-mapping/aws/), [GCP](cloud-mapping/gcp/), [Azure](cloud-mapping/azure/), or [Cloudflare](cloud-mapping/cloudflare/). The policy says "MFA is enforced for all production access"; the cloud configuration is how you make that statement true.
4. **Use the evidence guide** to set up the cadences (access reviews, vendor reviews, training, scans) that produce the artifacts your auditor will sample. The observation window for a Type 2 report is typically three to twelve months — start before the window opens, not after.

## Limitations

These templates are a starting point, not legal or audit advice. Every audit is scoped to a specific organization, environment, and set of Trust Services Criteria. Your auditor's interpretation of the criteria is the one that counts. When in doubt, ask them — most auditors are happy to clarify scope in advance of fieldwork.

These materials reflect the AICPA Trust Services Criteria framework. SOC 2 reports are issued by a licensed CPA firm; the resources here do not substitute for that engagement.

## Contributing

Contributions are welcome. Please open an issue describing the proposed change before sending a pull request, especially for policy or checklist content — small wording differences carry real audit implications, and we want to discuss them in the open.

## License

[MIT](LICENSE) — use freely, attribution appreciated but not required.

---

Built and maintained by [SimpleAudit](https://simpleaudit.io) — AI-driven SOC 2 compliance for teams without dedicated compliance staff.
