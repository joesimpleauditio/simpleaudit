# Vendor Risk Assessment — [Vendor Name]

**Vendor:** [Vendor Name]
**Service description:** [Brief description — e.g., "Customer support ticketing SaaS", "Email delivery API", "Cloud infrastructure provider"]
**Vendor URL:** [https://vendor.example.com]
**Vendor primary contact:** [Name, role, email]
**Internal owner:** [Internal owner name and role at your company]
**Assessment type:** [ Initial onboarding | Annual review | Subprocessor change | Incident-triggered ]
**Assessment date:** [YYYY-MM-DD]
**Next assessment due:** [YYYY-MM-DD — typically annual review one year from this date]

---

## 1. Engagement summary

**Purpose of engagement.** [Why we use this vendor. What business problem it solves.]

**Contract status:**
- Contract effective date: [YYYY-MM-DD]
- Renewal date: [YYYY-MM-DD]
- Term type: [annual | multi-year | month-to-month]
- DPA signed: [Yes / No / Not applicable — and reason]
- BAA signed (if PHI): [Yes / No / Not applicable]

## 2. Data exposure

**Data accessed or processed:**
- [ ] Customer-uploaded content
- [ ] Customer account metadata (org, users, settings)
- [ ] User authentication or session data
- [ ] Personal data (specify categories): [list]
- [ ] Financial / payment data
- [ ] Internal personnel data
- [ ] Internal corporate information
- [ ] None — vendor provides services without data access

**Data classification of accessed data (highest level):** [Public | Internal | Confidential | Restricted]

**Volume estimate:** [Rough scale — e.g., "Customer data for ~5,000 users", "Aggregate logs only"]

**Data flow:**

[Brief description of data flow: where data comes from in our systems, how it reaches the vendor, where it is stored, where outputs come back from, retention at vendor side.]

## 3. Risk tier

Based on the data sensitivity, operational criticality, and exposure characteristics above:

**Assigned risk tier:** [ Subprocessor (Tier 1) | Critical Service Provider (Tier 1) | Standard Vendor (Tier 2) | Low-Risk Vendor (Tier 3) ]

**Justification:** [Why this tier. Mention what would have justified a higher or lower tier and why those did not apply.]

**Listed on public subprocessor list:** [ Yes — added [date] | No, because [reason] ]

## 4. Vendor attestations reviewed

| Document type | Provided | Date | Period covered | Reviewer | Notes |
|---|---|---|---|---|---|
| SOC 2 Type 2 report | Yes/No | YYYY-MM-DD | YYYY-MM-DD to YYYY-MM-DD | [Reviewer] | [Notes — Trust Services Categories in scope; any exceptions noted] |
| SOC 1 Type 2 report | Yes/No/N/A | YYYY-MM-DD | YYYY-MM-DD to YYYY-MM-DD | [Reviewer] | |
| ISO 27001 certificate | Yes/No/N/A | YYYY-MM-DD | YYYY-MM-DD to YYYY-MM-DD | [Reviewer] | [Notes] |
| Security questionnaire response | Yes/No | YYYY-MM-DD | — | [Reviewer] | [If no SOC 2, this becomes the primary artifact] |
| Penetration test summary | Yes/No/N/A | YYYY-MM-DD | — | [Reviewer] | |
| Cyber-insurance certificate | Yes/No/N/A | YYYY-MM-DD | YYYY-MM-DD to YYYY-MM-DD | [Reviewer] | |

**Source documents stored at:** `[Path or link]`

## 5. SOC 2 review notes (if applicable)

**Categories in scope of vendor's SOC 2:** [Security | Availability | Confidentiality | Processing Integrity | Privacy]

**Trust Services Categories that align with our use of the vendor:** [Which categories are most relevant to how we use them. Example: "Security + Availability are the relevant categories; we do not use the vendor for processing customer financial transactions, so Processing Integrity is not a critical alignment."]

**Subservice organizations identified in vendor's report (carve-outs):** [List — e.g., AWS, Stripe, Twilio]

**Material exceptions reported:**

[List any exceptions noted in the vendor's report and our assessment of their impact on our environment.]

| Exception | Vendor's response | Our assessment |
|---|---|---|
| [Description] | [What the vendor noted as remediation] | [Acceptable / Compensating control in place / Risk accepted with target date / Unacceptable — action needed] |

**Bridge letter / gap letter received** covering period from report end through current date: [Yes / No / Not applicable]

## 6. Operational considerations

- **Availability commitments:** [SLA terms — uptime, response time]
- **Support availability:** [Hours, response time targets, escalation path]
- **Incident notification commitment:** [How quickly the vendor commits to notify us of an incident affecting our data]
- **Subprocessor change notification:** [Advance notice period in contract]
- **Audit rights:** [Right-to-audit terms — typically via SOC 2; on-site audits rare for small customers but should be documented]
- **Data deletion on termination:** [Vendor's commitment — typically a window after termination plus deletion confirmation on request]
- **Geography of data processing:** [Regions where data is stored / processed]

## 7. Identified risks and treatment

| # | Risk | Likelihood | Impact | Treatment | Owner | Target | Status |
|---|---|---|---|---|---|---|---|
| 1 | [Risk description] | Low / Med / High | Low / Med / High | Mitigate / Transfer / Accept / Avoid | [Owner] | [Target date] | [Open / In progress / Closed] |
| 2 | | | | | | | |

[Example: "Vendor's SOC 2 reports a Q2 exception related to access reviews. Vendor's remediation completed. Our assessment: Acceptable because (a) the exception was time-bounded, (b) the vendor's compensating monitoring detected the gap, and (c) the period of the exception did not coincide with onboarding of new vendor staff. No further action required."]

## 8. Conclusion

**Assessment outcome:** [ Approved for engagement | Approved with conditions | Not approved | Renewal approved | Termination recommended ]

**Conditions (if any):** [Required actions before or after engagement]

**Next review due:** [Date — typically 12 months from today, or earlier if specific concerns]

## 9. Approvals

| Role | Name | Date | Signature/Acknowledgement |
|---|---|---|---|
| Internal owner | [Name] | [Date] | [Initials or ticket reference] |
| [Owner Role] | [Name] | [Date] | [Initials or ticket reference] |
| Legal (for new DPAs or material concerns) | [Name] | [Date] | [Initials or ticket reference] |
| Leadership (for material risk acceptance) | [Name] | [Date] | [Initials or ticket reference] |

## 10. Retention

This record will be retained for the duration of the vendor relationship plus three (3) years per the Vendor Management Policy.

Storage location: `[Path or link]`

---

*Template generated by SimpleAudit — https://simpleaudit.io. Adapt the depth to the vendor's risk tier — Tier 3 reviews can be much shorter than this; Tier 1 reviews may need additional supporting documentation.*
