# SOC 2 Readiness Checklist

A practical checklist of items most organizations need in place before SOC 2 Type 2 audit fieldwork. Use it to discover gaps, sequence remediation, and pace your readiness work.

## What "readiness" means

A SOC 2 readiness assessment is the structured comparison between **what an auditor will look for** and **what your organization currently has in place**. The output is a list of gaps with owners and target dates. When the gap list is closed (or has documented compensating controls and accepted risks), you are ready for the formal audit.

Readiness assessments are not graded. There is no minimum score. Auditors look for control *coverage* — every Trust Services Criterion that applies to your scope has at least one documented, operating control — and *effectiveness* — the controls actually do what they claim.

There are two SOC 2 report types:

- **SOC 2 Type 1** — a snapshot. The auditor confirms that your controls are designed and in place on a specific date.
- **SOC 2 Type 2** — operating effectiveness over a period (typically three to twelve months). The auditor samples evidence from across the period to verify the controls operated as designed.

This checklist targets Type 2, which is what most enterprise customers ask for. If you are starting fresh, Type 1 is a reasonable first step — it forces you to document and implement controls without the additional pressure of producing months of evidence at audit time. Many organizations issue Type 1 in their first SOC 2 cycle and Type 2 the following year.

## How to use this checklist

### 1. Start with the Common Criteria

The SOC 2 Trust Services Criteria are organized into five categories:

- **Security (Common Criteria, "CC")** — required in every SOC 2 report. The CC sections (CC1 through CC9) cover the entire foundation.
- **Availability ("A")** — uptime, capacity, environmental protection, recovery
- **Confidentiality ("C")** — protection of information designated as confidential
- **Processing Integrity ("PI")** — completeness, accuracy, validity of system processing
- **Privacy ("P")** — collection, use, retention, disclosure of personal information

Every SOC 2 report covers Security. The other categories ("additional categories") are added based on your business and customer commitments. A typical SaaS company starts with Security only, adds Availability and Confidentiality once basic operations are stable, and considers Privacy when handling materially sensitive personal data.

This checklist groups items by category. Work through the Common Criteria first. Items in the additional-category sections only apply if your report scope includes that category.

### 2. Decide who owns each item

Each item has a default owner role in brackets (e.g., `[Engineering]`, `[HR]`, `[Legal]`). At a small company these may all be one or two people, but explicitly recording ownership prevents drift. Owners are responsible for the *control*, not necessarily for executing every artifact.

### 3. Work in waves

Most readiness efforts fall into four waves:

1. **Foundation** — Policies, identity, MFA, baseline access reviews, change management discipline. Without these, nothing else works.
2. **Operations** — Vendor management, vulnerability management, incident response, monitoring, backups.
3. **Evidence cadences** — Set up the recurring reviews and exercises that will produce the artifacts the auditor samples (access reviews, vendor reviews, tabletop exercises, restoration tests, etc.).
4. **Pre-audit cleanup** — Walk every control with a critical eye. Where does practice not match policy? Where is evidence thin? Fix gaps before the auditor finds them.

### 4. Build the evidence flywheel before the observation window opens

For a Type 2 audit, the auditor will sample evidence from the observation window. If the window is six months and you spin up access reviews in month five, you have one data point — not a track record. Start your cadences *before* the window opens so that by audit time you have the months of evidence the auditor needs.

### 5. Track gaps, not status

Avoid a "RAG status" tracker (red / amber / green) — it tells you nothing about what to do next. Track each gap with: owner, current state, target state, action items, target date. When the action items are closed and the target state is reached, the gap is resolved.

## How long does this take

A small SaaS company starting from scratch (no formal compliance program, no SOC 2 experience on the team) typically spends several months on readiness work before the formal audit can begin. The timeline depends on:

- **Starting maturity** — companies with existing engineering rigor (code reviews, CI/CD, IaC, working incident response) move faster than companies starting from informal practices.
- **Scope** — Security only is faster than adding all four additional categories.
- **Team capacity** — readiness work competes with feature work. Dedicate capacity or expect slow progress.
- **Tooling** — automated evidence collection (compliance platforms, cloud security posture tools) speeds the recurring work substantially, but tools alone do not create policies or run access reviews.

For a Type 2 report, add the observation window (typically three months minimum for an initial bridge, six to twelve months for subsequent reports). The full first cycle — readiness through report issuance — is commonly a six- to twelve-month effort.

## How auditors think

A few patterns worth understanding before you begin:

- **Documentation, design, and operation.** For each control, the auditor checks: Is it documented (in a policy)? Is the design adequate to meet the criterion? Did it operate during the period (evidence of operation)? Gaps at any of the three stages produce findings.
- **Sample sizes.** Type 2 auditors sample. For a quarterly control they may select two of the four quarters' evidence; for a daily control they may select several days. They look at the population of evidence, then sample.
- **Consistency between policy and practice.** If your policy says "access reviews are conducted quarterly" but evidence shows only one review in the period, that is a finding — even though one review is better than none. Either align practice to policy or align policy to practice.
- **Carve-outs and complementary user entity controls (CUECs).** The auditor will identify subservice organizations (e.g., AWS) that are carved out of your report — and CUECs that your *customers* must perform for your controls to be effective (e.g., "Customer is responsible for managing user accounts within their tenant"). These are normal; you do not need to compensate for them, but they should be accurate.

## How SimpleAudit fits in

This repository gives you the structured starting point. [SimpleAudit](https://simpleaudit.io) layers AI guidance, evidence automation, and a guided remediation workflow on top — useful when you want to compress the timeline and reduce the cognitive load on a small team. Either way, the artifacts you produce are yours, the controls are yours, and the audit is yours.

---

When you are ready, dive into [`checklist.md`](checklist.md).

Generated and maintained by SimpleAudit — https://simpleaudit.io
