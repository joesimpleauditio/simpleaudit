# SOC 2 Common Criteria → Cloudflare Service Mapping

A detailed mapping from each SOC 2 Common Criterion to Cloudflare services and the configurations that satisfy the criterion. The "Configuration Notes" column captures the specific settings auditors will want to see evidence of.

This table is opinionated — it assumes the architectural patterns described in the [section README](README.md): Cloudflare sits in front of (or alongside) a primary IaaS, SSO + MFA federated from your corporate IdP, scoped API tokens, WAF Managed Ruleset + Cloudflare Access in production. If your architecture differs, adapt the mappings accordingly.

> **Important scope note:** Cloudflare is an **edge/network/Zero Trust platform**, not a full IaaS. Many SOC 2 criteria — especially in CC1 (control environment), parts of CC2/CC3, and most of P1–P8 — are operated in your IdP, application code, or primary IaaS (AWS / GCP / Azure), not in Cloudflare. Cells marked **"Not directly applicable — Cloudflare provides the substrate; this control is operated in your IdP / application layer"** are honest descriptions, not gaps. Where Cloudflare *is* the primary control plane (CC6 access, CC7 monitoring, CC8 edge change management, A1 availability/DDoS), the mappings are dense.

> **Service-name note:** Cloudflare has rebranded several products. The current names are used throughout this table:
> - **Cloudflare Tunnel** (formerly Argo Tunnel)
> - **Cloudflare Zero Trust** (formerly Cloudflare for Teams) — now part of the **Cloudflare One** SASE platform
> - **Cloudflare Email Security** (formerly Area 1 Security)
>
> Documentation, terraform modules, and older runbooks may still reference the previous names.

---

## CC1 — Control Environment

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC1.1** | Commitment to integrity and ethical values | Cloudflare Trust Hub | Not directly applicable — Cloudflare provides the substrate; this control is operated at the policy and HR layer. |
| **CC1.2** | Board / leadership oversight | Cloudflare Account → Audit Logs | Account-level structure restricted; leadership receives security findings via Logpush → SIEM dashboards. Cloudflare itself is not the leadership-reporting control plane. |
| **CC1.3** | Organizational structure & reporting lines | Cloudflare Account Members + Roles, SCIM provisioning | Account roles reflect organizational structure (Super Administrator restricted to two named individuals; functional roles for Engineering, Security, Audit). Document the role assignments. |
| **CC1.4** | Demonstrates commitment to competence | Cloudflare Learning Paths, Cloudflare certifications | Personnel with privileged Cloudflare access have documented training records. Cloudflare's learning resources can serve as evidence. |
| **CC1.5** | Holds individuals accountable | Cloudflare Audit Logs, Logpush of audit events | Every privileged action in the Cloudflare dashboard is attributable via Audit Logs. Logpush audit events to your SIEM. Access reviews tied to corporate IdP records. |

## CC2 — Communication and Information

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC2.1** | Quality of information | Cloudflare Audit Logs, Logpush, Analytics + Logs API | Audit Logs are the canonical record of dashboard administrative actions; Logpush + Analytics provide quality-controlled traffic and security telemetry. |
| **CC2.2** | Internal communication of objectives | Cloudflare Notifications | Cloudflare Notifications route account/zone events (security advisories, DDoS attack notifications, certificate expirations) to email, PagerDuty, Slack, or webhook. |
| **CC2.3** | External communication | Cloudflare Pages / Workers (for status pages), DNS records | Customer-facing communication channels for security disclosures, incident notifications. Status page often runs on Cloudflare Pages or third-party (Statuspage, Better Stack). Not directly applicable to Cloudflare's product surface itself — operated at the application layer. |

## CC3 — Risk Assessment

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC3.1** | Specifies suitable objectives | Cloudflare Security Center | Security Center surfaces misconfigurations, exposed assets, and security recommendations across zones and the account. Reviewed during periodic risk assessments. |
| **CC3.2** | Identifies risks | Security Center, WAF Analytics, Bot Analytics, DDoS Analytics, Cloudflare CASB | Continuous findings from these services feed the risk register. CASB surfaces SaaS risk for Zero Trust customers. |
| **CC3.3** | Considers fraud risk | Bot Management, API Shield Sequence Mitigation, Zero Trust Risk Score | Bot Management distinguishes automation from humans; API Shield Sequence Mitigation detects API abuse patterns; Zero Trust Risk Score elevates users with anomalous behavior. |
| **CC3.4** | Identifies and assesses changes | Cloudflare Audit Logs, Terraform state, Security Center configuration drift alerts | Audit Logs + Terraform state answer "what changed and when." Security Center surfaces configuration drift on tracked resources. |

## CC4 — Monitoring Activities

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC4.1** | Selects and develops ongoing evaluations | Cloudflare Security Center, Logpush continuous evidence | Security Center continuously evaluates the account for misconfigurations. Logpush provides streaming evidence to a SIEM where compliance dashboards can be built. |
| **CC4.2** | Evaluates and communicates deficiencies | Cloudflare Notifications, Security Center, Logpush → SIEM/ITSM | Findings route to email, PagerDuty, Slack, or webhook. Aging Security Center issues tracked through the same ticketing system as other findings. |

## CC5 — Control Activities

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC5.1** | Selects and develops control activities | WAF rulesets, Zero Trust policies, Page Rules / Rulesets Engine | WAF Managed Rulesets + custom rulesets encode L7 control sets. Zero Trust Access policies + Gateway policies encode identity/network control sets. Rulesets Engine provides a unified configuration model. |
| **CC5.2** | Selects and develops technology controls | All Cloudflare products as appropriate | Specific controls mapped throughout this table. |
| **CC5.3** | Deploys controls through policies and procedures | API tokens, Cloudflare Terraform Provider, Audit Logs | Cloudflare Terraform provider deploys policies declaratively from source control. API tokens scoped to specific zones + permissions enforce deployment-time guardrails. |

## CC6 — Logical and Physical Access Controls

Dense — Cloudflare is the primary control plane for several CC6 controls when used as the Zero Trust access layer.

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC6.1** | Logical and physical access controls | Cloudflare SSO + MFA enforcement, Cloudflare Access (Zero Trust), API tokens, Account Roles | SSO from corporate IdP for all dashboard access; account-level MFA enforcement on all members. Cloudflare Access fronts internal applications and enforces identity + MFA + device posture. Legacy Global API Keys disabled. API tokens scoped, expiring, and IP-restricted where possible. Standalone username/password accounts are break-glass only. |
| **CC6.2** | User authorization and accountability | Cloudflare Account Members + Roles, Zero Trust Access policies, Audit Logs | New dashboard access via documented request → IdP group → SCIM-provisioned membership → role assignment. Every dashboard action attributable via Audit Logs. Every Access-protected application request attributable via Access logs. |
| **CC6.3** | Management of access through role changes and termination | SCIM provisioning from corporate IdP, Cloudflare Access policy by group | SCIM from corporate IdP automatically removes Cloudflare dashboard access when the IdP account is disabled. Access policies bind to IdP groups, so role changes propagate to application access. Quarterly review of dashboard members and Zero Trust seats. |
| **CC6.4** | Physical access restrictions | Cloudflare data centers (subservice) | Carve-out: Cloudflare operates physical security of its global PoPs. Your CUEC: do not transport or store data on physical media outside Cloudflare-managed facilities for Cloudflare-hosted assets (Workers, R2, KV, D1). |
| **CC6.5** | Information disposal | R2 lifecycle rules, Workers KV TTL, D1 deletion + retention | R2 lifecycle rules delete objects per retention policy. Workers KV TTL evicts keys automatically. D1 backups retained per documented retention. For data on origin (not Cloudflare-hosted), disposal is governed by the primary IaaS. |
| **CC6.6** | Restriction of access from outside the system boundary | Cloudflare WAF, DDoS Protection, Bot Management, API Shield, Cloudflare Tunnel, Authenticated Origin Pulls, Cloudflare Access, Gateway | This is Cloudflare's strongest control area. Production administrative interfaces fronted by Cloudflare Access — no direct internet exposure. Cloudflare Tunnel eliminates inbound origin firewall rules entirely. WAF Managed Ruleset + custom rules protect public properties. API Shield enforces schema + JWT on APIs. Authenticated Origin Pulls + IP allowlist prevent direct-to-origin attacks. Gateway filters outbound managed-device traffic. |
| **CC6.7** | Restriction of information transmission | Universal SSL, Always-Use-HTTPS, minimum TLS 1.2, HSTS, Authenticated Origin Pulls, Cloudflare Tunnel encryption | All transit encrypted (TLS 1.2+, with TLS 1.3 preferred). Always-Use-HTTPS forces redirect; HSTS prevents downgrade. Authenticated Origin Pulls and Cloudflare Tunnel encrypt the Cloudflare-to-origin leg with mTLS / TLS. |
| **CC6.8** | Prevention of unauthorized or malicious software | Browser Isolation, Gateway HTTP/Network filtering, Email Security, Cloudflare CASB | Browser Isolation contains risky web sessions in a remote browser. Gateway blocks known-malicious destinations via continuously-updated threat intelligence. Email Security blocks phishing and BEC. CASB scans SaaS for risky configurations. Workloads on origin servers are scanned by your primary IaaS tooling. |

## CC7 — System Operations

Dense — Cloudflare is the primary edge detection and monitoring layer.

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC7.1** | Detection of vulnerabilities | Security Center, Cloudflare CASB, WAF Attack Score | Security Center surfaces misconfigurations + exposed assets + outdated configurations across zones. CASB scans connected SaaS for known issues. WAF Attack Score machine-learning-classifies request likelihood-of-attack. |
| **CC7.2** | Monitoring for anomalies | WAF Analytics, Bot Analytics, DDoS Analytics, Zero Trust Analytics, Logpush → SIEM | Continuous analytics across WAF events, bot traffic, DDoS events, Zero Trust events. Logpush streams events to SIEM (Splunk, Microsoft Sentinel, Datadog) for cross-product correlation. |
| **CC7.3** | Evaluation of security events | Logpush → SIEM/SOAR, Cloudflare Notifications | Routing rules in the SIEM filter Cloudflare events into appropriate response queues. Notifications page on-call for critical events directly. |
| **CC7.4** | Incident response | Cloudflare Notifications, third-party paging (PagerDuty), DDoS Attack Notifications | Findings page on-call rotation via Notifications integrations; incident channel opened; runbook executed. DDoS Attack Notifications fire automatically during attacks. |
| **CC7.5** | Recovery from incidents | Terraform state, Cloudflare Page Rules / Rulesets as code, R2 versioning, Workers Wrangler rollback | Edge configuration recreated from Terraform state. Workers deployments rolled back via `wrangler rollback` or by redeploying a prior version from source control. R2 versioning enables object recovery. Recovery of origin systems is governed by the primary IaaS. |

## CC8 — Change Management

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC8.1** | Authorized changes to infrastructure, data, software | Cloudflare Terraform Provider, Wrangler (Workers/Pages CLI), Cloudflare API tokens, Audit Logs, Pages branch deployments | Branch protection in source-control (GitHub, GitLab) requires reviews on Terraform changes and Workers source. Wrangler deploys through CI/CD with scoped API tokens; manual dashboard changes logged via Audit Logs and reconciled to Terraform state. Pages preview deployments per branch + production deployments gated on main. Scoped API tokens prevent production write access from non-deployment contexts. |

## CC9 — Risk Mitigation

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **CC9.1** | Risk mitigation activities for disruption | DDoS Protection, Load Balancing + health checks, Argo Smart Routing, Magic Transit, Workers (multi-region by default), R2 (multi-region) | Cloudflare's anycast network provides global failover at the edge by design. Load Balancing health checks failover between origin pools. Magic Transit absorbs L3 DDoS for entire IP ranges. Workers and R2 are globally replicated. Origin-level disruption mitigation is governed by the primary IaaS. |
| **CC9.2** | Vendor and business partner risk | Cloudflare Trust Hub, Account Members (for vendor access), Cloudflare Access (for vendor app access) | Vendor access to the Cloudflare account scoped through limited roles or via Cloudflare Access policies on shared applications. Trust Hub provides Cloudflare's own SOC 2 attestation. |

## Availability (A1)

Dense — Cloudflare's edge anycast network + DDoS Protection are core availability controls.

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **A1.1** | Capacity management | Cloudflare anycast network, Load Balancing, Argo Smart Routing, Cloudflare Analytics, Workers Analytics | Cloudflare's anycast network provides essentially unlimited edge capacity by design. Load Balancing distributes traffic across origin pools. Argo Smart Routing optimizes paths. Analytics dashboards reviewed for traffic and latency trends. |
| **A1.2** | Backup and recovery infrastructure | Terraform state for edge configuration, R2 versioning, D1 backups, Workers source in version control | Edge configuration backed by Terraform state in source control. R2 versioning protects against accidental deletes. D1 backups configured per database. For data on origin, backup is governed by the primary IaaS. |
| **A1.3** | Tests recovery plan procedures | Terraform plan/apply rehearsals, Workers rollback drills, DDoS Attack drills | Annual recovery test documented with evidence: Terraform plan/apply against a staging zone, Workers rollback executed and verified, DDoS attack simulation or drill (or evidence from real attack mitigation). DR exercises for origin systems are governed by the primary IaaS. |

## Confidentiality (C1)

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **C1.1** | Protection of confidential information | Universal SSL + minimum TLS 1.2, R2 encryption at rest, Workers Secrets, Workers KV / D1 encryption at rest, Cloudflare DLP, Customer Keys (Keyless SSL, Geo Key Manager — Enterprise) | All Cloudflare-hosted data encrypted at rest by default. Workers Secrets encrypted per Worker; never logged. Keyless SSL keeps private keys on customer-controlled HSM/key servers. Geo Key Manager restricts private key residency to specific regions (e.g., EU-only). Cloudflare DLP detects sensitive data in HTTP traffic flows. |
| **C1.2** | Disposal of confidential information | R2 lifecycle rules, Workers KV TTL, D1 deletion, Audit Logs | Lifecycle rules delete R2 objects per retention. Workers KV TTL evicts keys. D1 deletes recorded via Audit Logs. Disposal of data on origin governed by primary IaaS. |

## Processing Integrity (PI1)

Cloudflare provides edge primitives for processing integrity; the application implements the logic.

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **PI1.1–PI1.5** | Definitions, input/processing/output integrity, completeness | API Shield (schema validation, JWT validation, sequence mitigation), Workers (idempotent handlers, Durable Objects for strong consistency), Workers Analytics, R2 versioning | API Shield enforces OpenAPI / schema validation at the edge before requests reach origin. Durable Objects provide single-threaded consistent state for transactional patterns. Workers Analytics traces request success rates. End-to-end processing-integrity logic is implemented in the application and primary IaaS. |

## Privacy (P1–P8)

Cloudflare provides infrastructure controls supporting privacy; the application and your primary IaaS implement the privacy program.

| SOC 2 Criterion | Description | Cloudflare Services | Configuration Notes |
|---|---|---|---|
| **P1 — Notice and communication of objectives** | Privacy notice provided to data subjects | Pages / Workers (for serving privacy notice), DNS records | Not directly applicable — operated at the application layer. Pages can host the privacy notice. |
| **P2 — Choice and consent** | Consent captured and honored | Workers (edge logic for consent enforcement) | Not directly applicable — operated at the application layer. Workers can intercept and apply consent-aware routing or header injection at the edge. |
| **P3 — Collection** | Only necessary personal data collected | Cloudflare DLP, API Shield discovery | DLP can detect inadvertent collection of sensitive fields in HTTP traffic. API Shield discovery surfaces undocumented APIs that may collect personal data. |
| **P4 — Use, retention, and disposal** | Personal data used only as authorized, retained only as needed | R2 lifecycle rules, Workers KV TTL, Logpush retention configuration | Lifecycle and TTL enforce retention limits on Cloudflare-hosted data. Logpush retention configured in destination SIEM/store. Personal data retention on origin is governed by the primary IaaS. |
| **P5 — Access** | Data subjects can access, correct, or delete their data | Not directly applicable — operated at the application layer | DSAR fulfillment evidence comes from application logs and primary IaaS data stores. Cloudflare Audit Logs may evidence administrative-side access to Cloudflare-hosted data. |
| **P6 — Disclosure to third parties** | Disclosures controlled and tracked | Cloudflare DLP, Gateway egress filtering, API Shield | DLP detects sensitive data flowing through HTTP. Gateway filters outbound traffic from managed devices. API Shield + Audit Logs evidence API-based disclosures. Cross-border data flow is governed by Geo Key Manager (for keys) and Regional Services / Data Localization Suite (for processing residency). |
| **P7 — Quality** | Personal data is accurate and complete | Not directly applicable — operated at the application layer | Application-level data quality controls. |
| **P8 — Monitoring and enforcement** | Privacy program monitored and enforced | Cloudflare DLP, CASB, Logpush → SIEM | DLP and CASB surface privacy-relevant misconfigurations. Logpush feeds privacy-event analytics in the SIEM. Privacy program governance is operated outside Cloudflare. |

---

## Service-by-service quick reference

For teams setting up Cloudflare as part of a SOC 2-ready architecture, the following minimum service set covers a large fraction of the table above:

| Cloudflare Service | Primary purpose for SOC 2 |
|---|---|
| **SSO + Account MFA enforcement** | Federated dashboard access (CC6.1, CC6.2) |
| **Scoped API tokens** | Least-privilege automation access (CC6.1, CC8.1) |
| **Audit Logs + Logpush** | Centralized audit logging (CC1.5, CC6.x, CC7.x, CC8.1) |
| **Cloudflare Access (Zero Trust)** | Identity-aware proxy for internal apps (CC6.1, CC6.6) |
| **Cloudflare Tunnel** | Origin protection without inbound firewall rules (CC6.6) |
| **Cloudflare Gateway** | DNS/HTTP/Network filtering on managed devices (CC6.6, CC6.8) |
| **WAF Managed Rulesets + custom rules** | Edge L7 protection (CC6.6) |
| **DDoS Protection (always-on)** | Volumetric + protocol attack mitigation (A1.1, CC9.1) |
| **API Shield** | API schema + JWT validation + sequence mitigation (CC6.6, PI1.x) |
| **Bot Management** | Automation detection (CC3.3, CC6.6) |
| **Authenticated Origin Pulls** | Origin protection via mTLS (CC6.7) |
| **Universal SSL + min TLS 1.2 + HSTS** | Transport encryption (CC6.7) |
| **Security Center** | CSPM-style configuration evaluation (CC3, CC4) |
| **Cloudflare Notifications** | Operational + security alerting (CC2.2, CC7.x) |
| **Cloudflare Terraform Provider** | Edge configuration as code (CC5.3, CC8.1) |
| **Workers Secrets** | Secret storage for Workers (CC6.1, CC6.7) |
| **Trust Hub** | Receive Cloudflare attestation reports (CC9.2) |

---

## How to use this table during readiness

1. **Walk top to bottom**, criterion by criterion, against your environment.
2. **For each row, capture three things:**
   - Is the service enabled?
   - Is it configured per the notes?
   - Where is the evidence?
3. **For "Not directly applicable" rows**, cross-reference the system where the control *is* implemented (your IdP, your primary IaaS, your application). The SOC 2 control still applies — it is just not Cloudflare's job.
4. **Track gaps** in your readiness checklist with target dates.
5. **Capture configurations in IaC** (the Cloudflare Terraform provider is mature) so the evidence is reviewable, versioned, and reproducible.
6. **Confirm with your auditor** that Cloudflare's role as a subservice organization is correctly scoped — typically narrower than the primary IaaS.

If a row does not apply to your environment, mark it not applicable with a written justification. "We do not use Cloudflare Tunnel" is acceptable; "We did not implement this" is not.

---

*Mapping current as of the date of this document; Cloudflare releases new products and rebrands services regularly. Validate service names and capabilities with Cloudflare documentation at audit time.*

*Generated and maintained by SimpleAudit — https://simpleaudit.io*
