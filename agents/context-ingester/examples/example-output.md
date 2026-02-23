# Example: Deal Profile for Acme Corp

> This is an anonymized example of what the Context Ingester produces. The Deal Profile is a **source index** — it maps where the deal lives across your tools, not what the deal context says. Context analysis happens in Step 2.

---

## User Input

```
"Build a deal profile for acme.com"
```

## Agent Output

```
DEAL PROFILE: Acme Corp
Domain: acme.com
Status: Draft
Created: 2026-02-21

COMPANY
  Description: Cloud infrastructure platform for mid-market companies [WEB]
  Industry: Developer Tools / Cloud Infrastructure [WEB]
  Size: 200-500 employees [WEB]
  Funding: Series B ($40M, raised Jan 2025) [WEB]
  HQ: San Francisco, CA [WEB]
  Website: https://acme.com [WEB]
  LinkedIn: https://linkedin.com/company/acme-corp [WEB]

CONTACTS
  Sarah Chen — VP Engineering — sarah@acme.com
    LinkedIn: https://linkedin.com/in/sarachen [WEB]
    Confidence: high
    Found in: [FF], [SL], [GCAL]

  Tom Liu — CTO — tom@acme.com
    LinkedIn: https://linkedin.com/in/tomliu [WEB]
    Confidence: high
    Found in: [FF], [GCAL]

  Priya Patel — DevOps Lead — priya@acme.com
    LinkedIn: https://linkedin.com/in/priyapatel [WEB]
    Confidence: high
    Found in: [FF], [SL]

  Raj Mehta — unknown role — raj@acme.com
    LinkedIn: not found
    Confidence: low
    Found in: [SL]

SOURCE INDEX
  Fireflies:
    Emails: sarah@acme.com, tom@acme.com, priya@acme.com
    Meetings: 3 (Jan 5 – Feb 10, 2026)
    Meeting IDs: ff_abc123, ff_def456, ff_ghi789
  Slack:
    Channels: #deal-acme (C0123ACME), #cs-acme (C0789CS)
    DM Users: Sarah Chen (U0456SARAH)
    Search terms: "acme", "acme.com"
  Notion:
    Pages: "Acme Corp - Deal Notes" (abc123), "Acme Onboarding Plan" (def456)
  Google Calendar:
    Past meetings: 3 (Jan 5 – Jan 19)
      - Jan 5 — Discovery Call - Acme — sarah@acme.com, tom@acme.com
      - Jan 12 — Technical Deep Dive — sarah@acme.com, tom@acme.com, priya@acme.com
      - Jan 19 — POC Kickoff — sarah@acme.com, priya@acme.com
    Upcoming: 1
      - Feb 25 — Acme POC Review — sarah@acme.com
  Web: searched

GAPS
  - Gmail: not connected
  - CRM: not connected

EXCLUDED (uncertain matches)
  - Slack: #proj-acme-migration — appears to be an internal project, not deal-related
  - Notion: "Acme Corp Invoice Template" — finance template, not deal-related
```

## User Interaction

```
User: "Looks good. Raj is actually their procurement lead."

Agent: Updated Raj Mehta — role: Procurement Lead.
       Confidence stays low (single source). Want me to search for him
       specifically to boost confidence?

User: "Yes, search for him."

Agent: Found raj@acme.com in Google Calendar — attended "Acme Commercial
       Discussion" on Feb 3. Confidence upgraded to medium.
       Updated profile.

User: "Approve. Save to Notion."

Agent: Deal Profile saved to Notion: "Acme Corp - Deal Profile"
       Ready for Step 2 — want me to pull deal context using this index?
```
