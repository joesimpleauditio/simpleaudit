# SimpleAudit Open Source — SOC 2 in Plain English

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A free, MIT-licensed library of SOC 2 resources: policy templates, a readiness checklist, an evidence-collection guide, and control-to-service mappings for the major clouds. Maintained by [SimpleAudit](https://simpleaudit.io), the AI-guided compliance platform built for founders and small teams who don't have a dedicated security hire.

If you've landed here because an enterprise prospect, an investor, or a customer security questionnaire put the words "SOC 2" in front of you for the first time, take a breath. SOC 2 is far more approachable than the consulting industry wants you to believe. This README explains what it actually is, what an audit really involves, and how to get started without spending five figures before you've written a single policy.

## What SOC 2 actually is

SOC 2 is a report — not a certificate, not a license, and not a government regulation. It's prepared by an independent CPA firm (the auditor) and it attests that your organization handles customer data according to a defined set of standards called the Trust Services Criteria, published by the American Institute of CPAs. There are five criteria: Security, Availability, Processing Integrity, Confidentiality, and Privacy. Only Security (often called the "Common Criteria") is mandatory. The other four are optional and you include them based on what your product actually does and what your customers care about. Most early-stage companies start with Security alone, and that is a completely legitimate, defensible scope.

The single most important thing to understand is this: **there is no "pass" or "fail" in SOC 2.** The auditor examines whether you actually do the things your policies say you do, and they document any place where reality and policy diverge as a *finding*. You then write a management response to each finding. The final report — your controls, the auditor's observations, the findings, and your responses — is what you hand to a prospect after they sign an NDA. Nobody expects a flawless report. They expect an honest one, from an organization that operates its controls consistently and responds thoughtfully when gaps surface. Once that clicks, the whole exercise stops feeling like an exam and starts feeling like what it is: a structured way to prove you take security seriously.

## Type 1 vs. Type 2

There are two flavors of SOC 2 report, and the difference is about time.

A **Type 1** report is a point-in-time snapshot. The auditor confirms that, on a specific date, your controls are *designed* appropriately. It's faster and cheaper to obtain, and it's a reasonable first step to show momentum to a prospect who is waiting on you.

A **Type 2** report covers a *period* — typically 3, 6, or 12 months — and confirms that your controls not only were designed correctly but *operated effectively* across that whole window. Type 2 is what most enterprise buyers ultimately want, because it proves consistency over time, not just a good day. Many companies do a Type 1 first to unblock a deal, then roll straight into a Type 2 observation period. If you're not sure which one your situation calls for, SimpleAudit has a free [Type 1 or Type 2 decision tool](https://simpleaudit.io/tools) that walks you through it in a couple of minutes.

## What an audit actually involves

Strip away the jargon and a SOC 2 effort has four moving parts.

**1. Scope.** Decide which Trust Services Criteria apply and which systems are in scope. Keep this honest and tight — over-scoping is the most common way small teams turn a manageable project into an expensive one.

**2. Policies.** SOC 2 expects a written, approved, version-controlled set of policies describing how you handle security-relevant activities: an information security policy, an access control policy, an incident response plan, a change management process, a vendor risk management policy, and a handful of others depending on scope. These documents need to *exist*, be *approved by leadership*, and *reflect what you actually do*. They do not need to be perfect on day one. The `policy-templates/` directory in this repository gives you a complete, adaptable starting set so you're not staring at a blank page.

**3. Controls.** Controls are the actual safeguards behind the policies: multi-factor authentication, encryption at rest and in transit, access reviews, logging and monitoring, backups, and so on. Some are technical (your engineers' domain) and some are administrative (training, acknowledgments, review cadences). The compliance owner's job is mostly to confirm controls are in place and documented — not to configure every firewall personally.

**4. Evidence.** This is where most teams underestimate the work. Auditors don't take your word for it; they ask for proof that controls operated throughout the period — access logs, configuration exports, vulnerability scan results, training completion records, meeting notes from access reviews. The hard part of evidence isn't pulling it from a system once; it's the discipline of capturing it *consistently over twelve months*. The `evidence-collection-guide/` directory explains what auditors expect in each category and how to build that cadence early instead of scrambling at the end.

## The myth that SOC 2 requires a big budget

The compliance tooling market has trained founders to believe that getting SOC 2 means signing a $10,000–$50,000-per-year contract for an automated evidence-collection platform built around dozens of integrations. For a 300-person company with multi-cloud infrastructure and a full-time compliance engineer, those platforms genuinely earn their price. For a ten-person startup with a handful of critical tools, they're a solution to a problem you don't have — you'd be paying enterprise prices for a security team you don't employ.

The genuinely hard parts of SOC 2 for a small team aren't the parts those platforms automate. They are: figuring out your scope, writing policies that reflect reality, defining things like your recovery time objectives, and staying organized over a year. Those are people problems, not integration problems. This is the bet SimpleAudit is built on — AI does the heavy drafting and gap analysis, a human reviews and approves, and the evidence is captured as a natural byproduct of doing the work. If you want a concrete sense of where you stand today, the free [SOC 2 readiness assessment](https://simpleaudit.io/assessment) takes a few minutes and gives you a personalized picture of your gaps with no sales call attached.

## A realistic 30-day starting plan

You will not be audit-ready in 30 days, but you can build real momentum:

- **Week 1 — Scope and gap analysis.** Decide your criteria (Security, almost certainly), list the systems in scope, and honestly catalog which controls you already have versus which you need to build. The `readiness-checklist/` directory gives you a step-by-step list to work through.
- **Week 2 — Start the policy library.** Use the `policy-templates/` here as your base. Adapt each to describe what your organization actually does, then get leadership approval. Draft, review, approve — don't aim for perfection.
- **Week 3 — Map controls to your stack.** If you run on AWS, GCP, Azure, or Cloudflare, the `cloud-mapping/` directory shows which native services satisfy which SOC 2 controls, so you're not guessing about how your infrastructure lines up.
- **Week 4 — Build the evidence habit.** Set up the folders, calendars, and review cadences that will capture evidence consistently. The hardest control to operate is the one you forget to document.

At the end of the month you'll have a gap analysis, a draft policy library, a control map, and a clear view of what's left — enough to have an honest conversation with a prospect about your timeline and enough to start a formal audit engagement.

## The five mistakes that cost small teams the most

A handful of avoidable errors account for most of the pain founders report after their first SOC 2 cycle. Knowing them in advance is worth more than any single tool.

**Over-scoping.** Including Availability, Confidentiality, or Privacy criteria you don't need multiplies the controls you have to operate and the evidence you have to collect. Start with Security. Add criteria later, when a specific customer requirement justifies the cost.

**Treating policies as shelfware.** A polished policy that nobody follows is worse than a rough one that everybody does, because the audit tests the gap between the two. Write policies that describe your real behavior, then tighten the behavior over time — not the other way around.

**Letting evidence evaporate.** The most common self-inflicted wound is relying on tools that auto-delete. Meeting transcripts, chat logs, and recordings frequently purge themselves after 30 or 90 days, and auditors looking back twelve months won't accept "it used to be there." Export evidence to a durable, access-controlled location the moment it's created. For access reviews specifically, keep the per-application user list from each session so you have a granular snapshot of what changed.

**Confusing "we bought a platform" with "we're compliant."** Tooling automates the easy part — pulling evidence from integrated systems. It cannot decide your scope, write policies that reflect your reality, or run your access reviews for you. The judgment work is still yours. Buy tools for leverage, not as a substitute for ownership.

**Waiting too long to start.** Because Type 2 covers a *period*, every month you delay is a month that won't count toward your observation window. The cheapest possible move is to stand up basic policies and start capturing evidence now, even before you've picked an auditor. Momentum compounds; procrastination accrues interest.

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

## AICPA Attribution

This repository references the SOC 2 Trust Services Criteria framework, owned by the AICPA. Criterion identifiers (CC1.1, CC6.1, etc.) are used for reference purposes. The full TSC document is available from the AICPA. This material is SimpleAudit's interpretation and is not affiliated with, endorsed by, or reviewed by the AICPA.

## Contributing

Contributions are welcome. Please open an issue describing the proposed change before sending a pull request, especially for policy or checklist content — small wording differences carry real audit implications, and we want to discuss them in the open.

## More from SimpleAudit

- **Free readiness assessment** — see where you stand: https://simpleaudit.io/assessment
- **Free SOC 2 tools** — cost calculator, Type 1 vs. Type 2 decision tool, timeline estimator: https://simpleaudit.io/tools

## License

[MIT](LICENSE) — use freely, attribution appreciated but not required.

---

*Maintained by [SimpleAudit](https://simpleaudit.io) — AI-driven SOC 2 compliance for teams without dedicated compliance staff. Contributions and corrections welcome via pull request.*
