# Context Ingester — Deal Profile Schema

## Overview

The Deal Profile is the **source index** for a deal. It maps WHERE a deal lives across your tools — the contacts, the lookup keys, the channels, the pages, the meetings. It does NOT contain deal context, signals, or summaries.

Think of it as an address book: it tells you where to look, not what you'll find.

**Step 2 (Context Pull)** uses this index to actually read and synthesize deal intelligence. That's a separate process that can run incrementally.

## Seed Input

The user provides ONE of:
- **Domain** — e.g., `acme.com`
- **Email** — e.g., `sarah@acme.com`
- **Company name** — e.g., `Acme Corp`

## Deal Profile Object

```yaml
deal_profile:
  # --- Identity ---
  company_name: "Acme Corp"
  domain: "acme.com"
  profile_status: draft | approved | stale
  created_at: "2026-02-21"
  last_updated: "2026-02-21"

  # --- Company (from web enrichment) ---
  company:
    description: "Cloud infrastructure platform for mid-market companies"
    industry: "Developer Tools"
    size: "200-500 employees"
    funding: "Series B ($40M, Jan 2025)"
    hq: "San Francisco, CA"
    website: "https://acme.com"
    linkedin_url: "https://linkedin.com/company/acme-corp"

  # --- Contacts ---
  contacts:
    - name: "Sarah Chen"
      email: "sarah@acme.com"
      role: "VP Engineering"
      linkedin_url: "https://linkedin.com/in/sarachen"
      confidence: high        # high | medium | low
      sources:                # which tools this person was found in
        - fireflies
        - slack
        - google_calendar

    - name: "Tom Liu"
      email: "tom@acme.com"
      role: "CTO"
      linkedin_url: "https://linkedin.com/in/tomliu"
      confidence: high
      sources:
        - fireflies

    - name: "Raj Mehta"
      email: "raj@acme.com"
      role: null              # unknown — not yet identified
      linkedin_url: null
      confidence: low
      sources:
        - slack

  # --- Source Index (lookup keys per tool) ---
  # This is the core value. Step 2 uses these to pull context.
  source_index:

    fireflies:
      participant_emails:     # emails that appear in call participant lists
        - "sarah@acme.com"
        - "tom@acme.com"
        - "priya@acme.com"
      meeting_count: 3        # how many meetings found
      meeting_ids:            # specific meeting IDs for direct lookup
        - "ff_abc123"
        - "ff_def456"
        - "ff_ghi789"
      date_range: "2026-01-05 to 2026-02-10"

    slack:
      channels:
        - id: "C0123ACME"
          name: "#deal-acme"
        - id: "C0789CS"
          name: "#cs-acme"
      dm_users:
        - id: "U0456SARAH"
          name: "Sarah Chen"
      search_terms:           # what queries found results
        - "acme"
        - "acme.com"

    notion:
      pages:
        - id: "abc123"
          title: "Acme Corp - Deal Notes"
        - id: "def456"
          title: "Acme Onboarding Plan"

    google_calendar:
      past_meetings:
        - date: "2026-01-05"
          title: "Discovery Call - Acme"
          attendees: ["sarah@acme.com", "tom@acme.com"]
        - date: "2026-01-12"
          title: "Technical Deep Dive"
          attendees: ["sarah@acme.com", "tom@acme.com", "priya@acme.com"]
        - date: "2026-01-19"
          title: "POC Kickoff"
          attendees: ["sarah@acme.com", "priya@acme.com"]
      upcoming_meetings:
        - date: "2026-02-25"
          title: "Acme POC Review"
          attendees: ["sarah@acme.com"]

    web:
      searched: true

  # --- Source Gaps ---
  source_gaps:
    - source: "gmail"
      detail: "Not connected"
    - source: "crm"
      detail: "Not connected"

  # --- Excluded Matches ---
  excluded_matches:
    - source: "slack"
      item: "#proj-acme-migration"
      reason: "Appears to be an internal project, not deal-related"
    - source: "notion"
      item: "Acme Corp Invoice Template"
      reason: "Finance template, not deal-related"
```

## Field Definitions

### What's IN the Deal Profile
- **Company identity**: name, domain, description, industry, size, funding, HQ, LinkedIn
- **Contacts**: name, email, role, LinkedIn URL, confidence level, which sources they appear in
- **Source index**: the exact lookup keys to find this deal in each tool
- **Google Calendar**: past and upcoming meetings with deal contacts
- **Gaps**: sources searched but found nothing, or not connected
- **Excluded**: matches found but deemed unrelated

### What's NOT in the Deal Profile
- Call summaries, action items, or transcript content
- Slack message content or conversation threads
- Notion page content
- Deal signals, risk indicators, or sentiment
- Contract values, renewal dates, or stage
- Relationship context ("what this person cares about")

All of that lives in **Step 2: Context Pull**.

### Confidence Levels (for contacts)
- **high** — Found in 2+ sources, or email directly in call participant lists
- **medium** — Found in 1 source with clear role context
- **low** — Name mentioned without email, or appeared once with minimal context

### Profile Status
- **draft** — Agent has assembled the profile, awaiting user review
- **approved** — User has reviewed and confirmed
- **stale** — Not updated in 14+ days (configurable)

### Source Gaps vs. Excluded Matches
- **Source gaps**: searched but found nothing, or source not connected
- **Excluded matches**: found something, but not confident it's related to this deal. User can promote these.

## How Step 2 Uses This

Step 2 (Context Pull) takes the source index and reads actual content:

| Source | Index Keys Used | What Step 2 Pulls |
|---|---|---|
| Fireflies | meeting_ids, participant_emails | Call summaries, action items, key decisions |
| Slack | channel IDs, DM user IDs | Message threads, deal signals, engagement patterns |
| Notion | page IDs | Page content, deal notes, account plans |
| Google Calendar | meeting dates, attendees | Meeting frequency, upcoming touchpoints |
| Web | domain, company name | Recent news, competitor mentions |

Step 2 can run incrementally using `last_updated` — only pull content newer than the last run.

## Incremental Updates

The Deal Profile supports two types of updates:

1. **Add to the index** — "I found a new contact, add them" or "There's a new Slack channel for this deal"
   - Just append to the relevant section, update `last_updated`
   - No need to re-run full discovery

2. **Re-run discovery** — "Do a fresh search, see if anything new has appeared"
   - Re-run the 3-pass playbook, merge new findings into existing profile
   - Flag what's new vs. what was already known
