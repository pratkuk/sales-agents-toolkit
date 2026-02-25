# Project: B2B Sales Agents Toolkit

## Owner
Pratyush Kukreja — pratyushkukreja.com

## What This Is
AI agents that think like a seasoned sales professional — not just execute like an eager intern who can type very fast. A three-agent pipeline that builds deal context, reads signals, and delivers strategic analysis. Plus a personal brand website to showcase and distribute them.

## Core Philosophy

### First Principles Over Frameworks
We do NOT use BANT, MEDDPIC, or any checkbox qualification framework. Instead, agents assess deals based on fundamental questions:
- Does this person have a real problem we solve?
- Can they actually buy? (authority + budget reality)
- Is there urgency or a triggering event?
- What's the cost of inaction for them?
- Are we talking to the right people?
- What has to be true for this deal to close?

### No Custom Connectors
We don't build integrations. We build intelligence layers that sit on top of existing connector ecosystems (MCP servers, Zapier, native APIs). The value is in what we do with the data, not in pulling it.

### Agents Expect Context, Not Connections
Each agent defines a context schema (what shape the input should be). Users bring their own data from whatever tools they use. This makes agents connector-agnostic.

## Architecture

### The Agent Pipeline
Three agents, chained sequentially. Each agent's output is the next agent's input.

| # | Agent | Input | Output |
|---|-------|-------|--------|
| 1 | **Context Ingester** | Seed (domain, email, or company name) | Deal Profile: source index of meetings, channels, pages, contacts, calendar events |
| 2 | **Signal Reader** | Deal Profile + actual content from sources | Signal clusters: what's actually true about this deal right now |
| 3 | **Deal Advisor** | Signal clusters + deal context | Strategic analysis: how a seasoned VP Sales would read this deal |

### Why This Pipeline (Not 6 Agents)
Things like call summarization, follow-up drafting, and call prep are already well-served by base LLMs with good prompts. The hard problem — and where real value lives — is: Context → Signals → Strategy. That's what this pipeline solves.

### Signal Reader — Signal Clusters, Not Stages
The Signal Reader does NOT map deals to linear CRM stages (Discovery → POC → Negotiation). Instead, it surfaces **signal clusters** — what's actually true about this deal right now. No fixed stages. The seller interprets the signals.

### Deal Advisor — Strategic Analysis, Not To-Do Lists
The Deal Advisor gives a strategic read of the deal — the kind of analysis a seasoned VP Sales would give in a deal review. It's not a checklist of tactical actions. It's a way of thinking about the deal.

### Context Ingester — Two-Step Architecture
The Context Ingester is split into two steps to minimize token cost and enable incremental updates:

**Step 1: Deal Profile (source index)**
- User provides ONE seed (domain, email, or company name)
- 3-pass discovery: internal sources → cross-enrichment → web enrichment
- Collects identifiers ONLY: meeting IDs, channel IDs, page IDs, contact emails, calendar events
- Does NOT read content (no transcripts, no messages, no page content)
- Output: Deal Profile with company info, contacts, and source index
- Sources: Fireflies, Slack, Notion, Google Calendar, Web

**Step 2: Signal Reader (next agent to build)**
- Takes the Deal Profile as input
- Uses source index to read actual deal content (transcripts, Slack threads, meeting patterns)
- Surfaces signal clusters — what's true about this deal right now
- No fixed stages — signals, not checkboxes

**Design Decisions:**
- **Ambiguity handling**: exclude uncertain matches but mention them to user
- **Gaps**: explicitly shown ("Notion: no matches found")
- **Curation**: light touch — skip scheduling noise, keep everything else
- **Incremental updates**: add contacts/sources without full re-run, or re-run discovery to find new things
- **Output**: Notion page (persistence) + agent memory (chaining)

### Dual Distribution
Each agent ships as:
- **Prompt template** — copy-paste into Claude/any LLM for quick use
- **Claude Agent SDK app** — runnable code with tool use and chaining for power users

## Website: pratyushkukreja.com

### Purpose
Personal brand site. Pratyush + his projects. The sales agents toolkit is the flagship project.

### Sections (single-page)
- Hero (identity + philosophy)
- Why (first principles thesis, 4 cards + manifesto)
- Agents (3-agent pipeline: context-ingester, signal-reader, deal-advisor)
- Writing (blog posts, thoughts, build logs)
- Footer

### Tech
- Single HTML file: `docs/index.html`
- Dark theme, blue accent (#60a5fa), JetBrains Mono + Outfit fonts
- Deployed via GitHub Pages from `docs/` directory
- Live at: https://pratkuk.github.io/sales-agents-toolkit/
- Writing section supports three content types: article, thought, build log

## Project Structure
```
Builder/
├── CLAUDE.md
├── README.md
├── .gitignore
├── docs/
│   ├── index.html           ← website (GitHub Pages source)
│   └── plans/               ← design docs
├── agents/
│   ├── context-ingester/    ← BUILT (Step 1: Deal Profile)
│   ├── signal-reader/       ← NEXT
│   └── deal-advisor/        ← AFTER signal-reader
├── frameworks/              ← first principles framework definition
├── connector-guides/        ← how to wire up data sources
└── examples/                ← full deal walkthroughs
```

## Conventions
- Use clear, simple language. This is for salespeople, not engineers.
- Agent prompts should be conversational, not robotic.
- Every agent folder must have: README.md, prompt.md, context-schema.md, examples/
- Prefer markdown for all documentation and content.
- Keep things modular — each agent must work standalone.

## What NOT To Do
- Don't build custom API connectors or integrations
- Don't use MEDDPIC/BANT/SPICED terminology in agent prompts
- Don't over-engineer — these are tools for practitioners, not enterprise software
- Don't add features without a clear use case from real sales workflows
- Don't make the website feel like a SaaS landing page — it's a personal brand site

## Current Status
- Context Ingester (Step 1: Deal Profile): BUILT
- Website: LIVE (GitHub Pages)
- Next: Signal Reader agent
- Then: Deal Advisor agent
