# Cloudflare-to-SOC 2 Mapping

How Cloudflare services support the SOC 2 Trust Services Criteria, and which configurations satisfy specific criteria. This document is the pillar reference; the detailed control-by-service table lives in [controls.md](controls.md).

## Why specific service mapping matters

Telling your auditor "we use Cloudflare, which is SOC 2 compliant" does not help you. Cloudflare's SOC 2 attests to Cloudflare's controls — physical security of its global PoPs, the foundational edge and Zero Trust service implementations, the operational controls keeping its network running — and not to *your* use of Cloudflare. Your SOC 2 must demonstrate the controls *you* operate on top of Cloudflare.

The granularity matters because auditors test specifics, not categories. They will not ask "do you have edge protection?" They will ask "show me which WAF managed rule sets are deployed, how a specific Cloudflare Access application is gated to your engineering group, and how Logpush feeds your SIEM." The answer is a series of specific policy configurations — Access application policies, Gateway rules, WAF custom + managed rules, Logpush jobs, API token scopes — not the phrase "we use Cloudflare securely."

This document and the linked table map each SOC 2 Common Criterion to the Cloudflare services and configurations that satisfy it. Use them as a checklist when implementing controls and as a script when discussing your environment with the auditor.

## A note on Cloudflare's scope

Cloudflare is **not** a full IaaS like AWS, GCP, or Azure. Cloudflare operates an edge/network/Zero Trust platform plus a focused set of compute primitives (Workers, Pages, R2, D1, Durable Objects, Workers KV). For most SOC 2 readiness programs, Cloudflare is one of *several* providers — typically paired with a primary IaaS (where your origin servers and databases live) and a primary identity provider (Okta, Microsoft Entra ID, Google Workspace).

This means several SOC 2 criteria are **operated at the policy and integration layer** rather than implemented natively in Cloudflare. The controls.md table makes this explicit — where Cloudflare provides the substrate but you operate the control in your IdP, application, or primary cloud, the cell says so.

Where Cloudflare *is* the primary control plane — edge security (CC6.6, CC7.2), Zero Trust access (CC6.1, CC6.2), DDoS protection and availability (A1.1), edge change management for Workers/Pages deploys (CC8.1) — the mappings are dense and specific.

## Shared responsibility — what Cloudflare does, what you must do

Cloudflare publishes a [Shared Responsibility Model](https://www.cloudflare.com/trust-hub/compliance-resources/shared-responsibility/) framed around their service categories. The practical division:

- **Cloudflare is responsible for security *of* the platform** — physical security of its global data center footprint (over 300 cities), the underlying network fabric, the foundational service implementations of WAF, DDoS protection, Zero Trust products, Workers runtime, R2/D1/Durable Objects storage. Cloudflare's SOC 2 Type II attests to these.
- **You are responsible for security *in* the platform** — Cloudflare account access (SSO + MFA), API token scope and rotation, Zero Trust policy configuration (Access applications, Gateway rules), WAF rule selection and tuning, Workers code and secret management, DNS record integrity, origin protection (authenticated origin pulls, IP allowlists), Logpush destination management, audit log review.

In your SOC 2 system description, Cloudflare will appear as a **subservice organization**, "carved out" of your report. The auditor will note that Cloudflare's SOC 2 is reviewed and that Cloudflare's controls support your environment. The auditor will not test Cloudflare's controls; the auditor will test yours.

Note: because Cloudflare is typically not the system of record for customer data (data usually lives on your origin / primary IaaS), Cloudflare's subservice scope is usually narrower than the primary IaaS — focused on edge security, DNS, and Zero Trust rather than data storage and processing.

## Inheriting Cloudflare's controls

You inherit Cloudflare's controls by:

1. **Reviewing Cloudflare's SOC 2 report at least annually.** Available through the [Cloudflare Trust Hub](https://trust.cloudflare.com/). Cloudflare publishes SOC 2 Type II, ISO 27001/27017/27018/27701, PCI DSS, and other attestations. The latest SOC 2 Type II covers a 12-month period; review it for exceptions relevant to your environment.
2. **Subscribing to Cloudflare Status, security advisories, and Trust Hub updates** to be aware of incidents and policy changes affecting your controls.
3. **Documenting Cloudflare as a subservice organization** in your system description, including which Trust Services Categories Cloudflare's SOC 2 covers and the carve-out methodology.
4. **Defining Complementary User Entity Controls (CUECs)** — controls *you* must operate for Cloudflare's controls to be effective. Examples specific to Cloudflare: enforcing SSO + MFA on Cloudflare dashboard access, scoping API tokens to specific zones and permissions with expirations, configuring Zero Trust Access policies that integrate with your primary IdP, enabling Logpush to your SIEM, configuring authenticated origin pulls so origins reject non-Cloudflare traffic.

## Where the Cloudflare Trust Hub fits

The [Cloudflare Trust Hub](https://trust.cloudflare.com/) is Cloudflare's portal for compliance documentation. From it, you can download:

- Cloudflare SOC 2 Type II report
- ISO 27001, 27017, 27018, 27701 certificates
- PCI DSS Attestation of Compliance
- HIPAA / FedRAMP / other regional/industry-specific attestations
- Sub-processor list and DPA materials

For SOC 2 readiness, the SOC 2 Type II report is the primary document. Access requires an NDA — typically clicked through in the portal — before downloading restricted reports.

## Architectural patterns that support SOC 2

Several Cloudflare architectural patterns substantially simplify SOC 2 compliance when Cloudflare sits in front of (or alongside) your primary cloud:

### SSO + MFA for the Cloudflare dashboard

Federate Cloudflare dashboard access from your corporate identity provider (Okta, Microsoft Entra ID, Google Workspace) via SAML SSO. Enforce MFA at the IdP or via Cloudflare's Account Two-Factor enforcement (require all members). Use Cloudflare's role-based access (Super Administrator, Administrator, Analytics, Audit Logs Viewer, custom roles) — never share dashboard accounts. SCIM provisioning automates lifecycle.

### API token discipline

Cloudflare's API supports scoped, expiring tokens (the modern alternative to global API keys). Every automation should use a scoped token with the minimum required permissions, an explicit IP allowlist where feasible, and an expiration date. Rotate routinely. Audit log token usage. The legacy Global API Key should be disabled / unused except for break-glass scenarios.

### Cloudflare Zero Trust as the access plane

**Cloudflare Access** (part of Cloudflare One / Zero Trust) places identity-aware proxy in front of internal applications. Access policies integrate with your primary IdP (Okta, Entra ID, Google Workspace) and enforce per-application identity + device-posture + MFA requirements. Pair with **Cloudflare Tunnel** (formerly Argo Tunnel) to expose internal applications without inbound firewall rules on origin VPCs.

**Cloudflare Gateway** provides DNS, HTTP, and Network filtering for outbound traffic — typically deployed via the WARP client on managed devices. Gateway DNS resolves against Cloudflare's threat intelligence; HTTP filtering blocks unsanctioned SaaS; Network filtering enforces L4 policies.

> **Service-name note:** Cloudflare for Teams was rebranded **Cloudflare Zero Trust** (and now sits inside the **Cloudflare One** SASE platform). Argo Tunnel was renamed **Cloudflare Tunnel**. Documentation may still reference the older names.

### Edge security via WAF, Bot Management, API Shield, and DDoS Protection

**Cloudflare WAF** with Cloudflare Managed Ruleset + OWASP Managed Ruleset + custom rules provides L7 web application firewall. **Cloudflare Bot Management** distinguishes verified bots, likely automation, and humans. **API Shield** provides schema validation, JWT validation, sequence mitigation, and discovery for APIs. **Cloudflare DDoS Protection** (always-on for HTTP / DNS / Layer 3-4) mitigates volumetric and protocol attacks; **Magic Transit** extends L3 DDoS protection to entire IP ranges via BGP; **Spectrum** extends DDoS + proxying to arbitrary TCP/UDP services.

### Origin protection

The default Cloudflare deployment proxies traffic from the edge to your origin servers, but the origin remains internet-routable unless protected. Pattern: combine **Authenticated Origin Pulls** (origin only accepts TLS from Cloudflare), **Cloudflare Tunnel** (no inbound firewall rules at all — origin reaches out), and **IP allowlists** on the origin firewall restricted to Cloudflare's published IP ranges. This prevents direct-to-origin attacks that bypass the edge.

### Workers and edge compute

**Cloudflare Workers** runs JavaScript/TypeScript/Rust at the edge with V8 isolates. **Workers KV** provides eventually-consistent key-value storage; **D1** is a serverless SQL database; **R2** is S3-compatible object storage (no egress fees); **Durable Objects** provides strongly-consistent stateful coordination. For SOC 2, Workers deploys go through Wrangler + source control with branch protection, and secrets are managed via `wrangler secret put` (encrypted at rest, scoped per Worker).

### Logpush + Audit Logs

**Logpush** ships zone-level logs (HTTP requests, WAF events, DDoS events, Zero Trust events, R2 events) to your SIEM (Splunk, Microsoft Sentinel, Datadog, S3 bucket, etc.) on a near-real-time cadence. **Account audit logs** record administrative actions in the Cloudflare dashboard — these should also feed your SIEM and be reviewed during access reviews.

### Email Security (Area 1) and Browser Isolation

For organizations using Cloudflare for inbound email defense, **Cloudflare Email Security** (formerly Area 1) provides phishing and BEC detection. **Browser Isolation** renders web pages in a remote browser, useful for treating Gateway-classified risky categories.

## A realistic minimum Cloudflare configuration for SOC 2 readiness

If you are starting from a minimal Cloudflare footprint, the following provides a reasonable foundation:

1. SSO from corporate IdP enabled; MFA enforced for all account members; legacy API keys disabled
2. Account roles assigned via groups, not directly to users; least-privilege role assignments documented
3. Scoped, expiring API tokens for every automation; no use of the Global API Key except break-glass
4. WAF Managed Ruleset + OWASP Managed Ruleset deployed on all production zones; custom rules tuned for application-specific risks
5. Always-Use-HTTPS, minimum TLS 1.2, HSTS enabled on production zones
6. Authenticated Origin Pulls + IP allowlist (or Cloudflare Tunnel) on all origins — direct-to-origin traffic blocked
7. Cloudflare Access in front of all internal applications, integrated with the primary IdP, requiring MFA + device posture for sensitive apps
8. Cloudflare Tunnel for any application that must not have inbound firewall rules
9. Logpush jobs streaming HTTP request logs, WAF events, Access events, Audit Logs, and DNS Firewall events to your SIEM
10. DDoS Protection (always-on) reviewed; HTTP DDoS Managed Ruleset and Network DDoS Managed Ruleset deployed
11. API Shield enabled for production APIs (schema validation + JWT validation where applicable)
12. Bot Management deployed on user-facing properties
13. DNSSEC enabled on critical zones
14. Account audit logs reviewed monthly; quarterly user access review against the IdP

This is the baseline. The detailed table in [controls.md](controls.md) maps each item to specific SOC 2 criteria and notes additional configurations.

## On Cloudflare as a subservice organization

In your SOC 2 system description, you will explicitly identify Cloudflare as a subservice organization. The standard pattern is:

> *"[Company Name] uses Cloudflare as a subservice organization for edge security, content delivery, and Zero Trust access. Cloudflare is responsible for the controls supporting physical and environmental security of its global data center footprint, the underlying network fabric, and the foundational service implementations of WAF, DDoS protection, and Zero Trust products. [Company Name] is responsible for the controls supporting logical access to and configuration of Cloudflare services in its account, including SSO, API token management, WAF rule selection, Zero Trust policy configuration, and Logpush destination management."*

The carve-out is then listed alongside the Complementary User Entity Controls (CUECs) you implement.

## Practical first steps

If you are early in readiness:

1. **Sign in to the [Cloudflare Trust Hub](https://trust.cloudflare.com/)** so you can download Cloudflare's SOC 2 report and review it.
2. **Set up SSO + MFA enforcement** on the Cloudflare account; rotate or disable any pre-existing personal-account members.
3. **Audit API tokens** — disable the Global API Key, replace ad hoc tokens with scoped/expiring tokens.
4. **Deploy the WAF Managed Ruleset** and **enable Logpush** to your SIEM. These two changes alone produce substantial SOC 2 evidence.
5. **Walk the [controls.md](controls.md) table** with your engineering team. Mark which rows you already satisfy, which need work, and which do not apply — many cells will say "operated at the IdP / application / primary IaaS layer" and that is fine, as long as the cross-reference is documented.

See also: the [root cloud-mapping README](../README.md) for cross-provider guidance, and the [project README](../../README.md) for how this fits into a full SOC 2 readiness program.

---

When you have implemented the controls described here, you will have addressed the Cloudflare-specific work that SOC 2 requires. The remaining work spans your primary IaaS (see the [AWS](../aws/), [GCP](../gcp/), and [Azure](../azure/) sections), your identity provider, and the *human* controls — access reviews, incident response, training, vendor management — governed by the policies in [/policy-templates/](../../policy-templates/).

*Generated and maintained by SimpleAudit — https://simpleaudit.io*
