# Acceptable Use Policy

**Owner:** [Owner Role — typically Head of People / HR or CTO]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy describes the responsibilities of every individual who uses [Company Name]'s information systems, devices, and data. It exists to protect the company, its employees, and its customers from harm caused by careless, inappropriate, or malicious use of company resources.

## Scope

This policy applies to all employees, contractors, consultants, interns, and any other individuals who access [Company Name]'s systems, devices, networks, or data. It applies regardless of whether the resource is owned by [Company Name] or by the individual (for example, a personal device used to access company email).

## Policy statements

### 1. General expectations

1.1. Personnel shall use [Company Name] resources for legitimate business purposes. Incidental personal use is permitted to the extent that it does not interfere with work, consume disproportionate resources, expose the company to risk, or violate this or any other policy.

1.2. Personnel shall not use [Company Name] resources to engage in or facilitate illegal activity, harassment, discrimination, intimidation, or any activity that violates [Company Name]'s code of conduct or applicable law.

1.3. Personnel shall not use [Company Name] resources to operate a personal business, conduct political fundraising, or perform substantial work for a third party without prior written approval from leadership.

### 2. Account and credential responsibilities

2.1. Personnel are responsible for actions performed with their identity. Credentials, hardware MFA tokens, and authentication devices shall not be shared, lent, or transferred.

2.2. Personnel shall use strong, unique passwords and a company-approved password manager. Reusing personal passwords for work accounts (or vice versa) is prohibited.

2.3. Suspected credential compromise — including phishing clicks, lost devices, or unexpected MFA prompts — shall be reported to [Security Contact — e.g., security@company.com or #security Slack channel] within twenty-four (24) hours.

### 3. Device use

3.1. **Company-issued devices** shall:
- Be enrolled in [Company Name]'s device management system before being used to access company data
- Run a supported, patched operating system
- Have full-disk encryption enabled
- Have endpoint security software installed and operational
- Be physically secured when unattended (locked screen, locked office, hotel safe, etc.)

3.2. **Personal devices** ("BYOD") may be used to access company email, chat, and calendaring with the following requirements:
- The device runs a supported, patched OS with a screen lock and full-disk encryption
- Company applications are installed only through approved app stores
- The device is enrolled in [Company Name]'s mobile device management if accessing email
- Production environments, source code, and customer data shall not be accessed from personal devices unless explicitly authorized in writing by the [Owner Role]

3.3. Lost or stolen devices shall be reported to [Security Contact] within four (4) hours of the discovery.

### 4. Email, chat, and communication

4.1. Personnel shall exercise judgment in email and chat use, especially when handling customer data. Customer data shall not be forwarded to personal email accounts.

4.2. Suspected phishing messages shall be reported using [Phishing Reporting Mechanism — e.g., the "Report Phish" button or phishing@company.com] and shall not be forwarded to colleagues for discussion.

4.3. Personnel shall not click links or open attachments from unknown senders, and shall verify unexpected requests from internal senders (especially those involving credentials, money, or sensitive data) through a second channel.

### 5. Data handling

5.1. Information shall be handled according to its classification under the [Data Classification Policy](data-classification-policy.md).

5.2. Customer data shall not be copied to personal devices, personal cloud storage (Dropbox, iCloud, personal Google Drive), or removable media (USB drives, external disks) except through approved tooling.

5.3. Personnel shall not share customer data with third parties (auditors, partners, contractors) without verifying that the recipient is authorized under contract and that the data is being shared through an approved channel.

5.4. When working in public spaces (cafes, airports, coworking spaces), personnel shall be mindful of shoulder-surfing and use a privacy screen when displaying classified information.

### 6. Software and tooling

6.1. Software installed on company-issued devices shall come from approved sources (the corporate app catalog, official vendor sites, or approved package managers). Pirated, cracked, or unlicensed software is prohibited.

6.2. New SaaS tools that will be used with company data shall be reviewed under the [Vendor Management Policy](vendor-management-policy.md) before purchase or use. "Shadow IT" (unmanaged SaaS subscriptions paid by individuals or expensed without review) is prohibited.

6.3. **Use of generative AI services** with company data:
- Public AI services (free-tier ChatGPT, free-tier Claude, etc.) shall not be given customer data or any classified information
- Approved AI services with appropriate data-processing terms are listed in [AI Tools Approved List]
- Personnel using AI for code generation shall review all generated code before committing, and shall ensure no secrets, customer data, or proprietary information is sent in prompts

### 7. Network use

7.1. Personnel shall not attempt to bypass [Company Name]'s network controls (firewalls, web filtering, DLP).

7.2. Personnel shall not connect company devices to untrusted networks for production access without using [VPN / Zero-Trust Access Solution].

7.3. Personnel shall not run scanning, probing, or exploitation tools against [Company Name]'s systems or third parties' systems unless explicitly authorized as part of their role (for example, security engineers performing approved testing).

### 8. Monitoring

8.1. Personnel should not have an expectation of privacy when using [Company Name] systems. [Company Name] may monitor, log, and inspect activity on company systems and devices to enforce this policy, investigate incidents, or comply with legal obligations, subject to applicable law.

8.2. Monitoring shall be performed for legitimate business purposes and limited to what is necessary. Personnel will be notified of monitoring through this policy.

### 9. Reporting and consequences

9.1. Suspected policy violations shall be reported to [Reporting Channel — manager, [Owner Role], HR, or anonymous reporting line].

9.2. Violations may result in disciplinary action up to and including termination of employment or contract. Severe violations may be referred to law enforcement.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy and oversee enforcement |
| Managers | Reinforce expectations and address violations within their teams |
| IT / Security | Provide tooling that makes compliance easier; monitor for violations |
| All personnel | Read, acknowledge, and follow this policy; report violations |

## Review and approval

This policy shall be reviewed at least annually by the [Owner Role] and approved by leadership. Every employee and contractor shall acknowledge this policy at hire and at least annually thereafter. Acknowledgement records shall be retained for the duration of the engagement plus two (2) years.

## Related SOC 2 criteria

- **CC1.1, CC1.4** — Commitment to integrity and ethical values; demonstrating commitment to competence
- **CC2.2** — Internal communication of information security responsibilities
- **CC6.2** — User authorization and accountability
- **CC6.7** — Restriction of transmission of information

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
