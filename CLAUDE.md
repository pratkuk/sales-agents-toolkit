# Project: B2B Sales Agents Toolkit

## Owner
Pratyush Kukreja — pratyushkukreja.com

## What This Is
An open-source collection of AI agents that help B2B SaaS salespeople execute deals from SQL assignment through CSM handover. Plus a personal brand website to showcase and distribute them.

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

### The Sales Journey (Vertical Stages)
SQL Assignment → Qualification → POC → POC Assessment → Commercial Discussion → Multithreading → Security Assessment → Contracting → Closed Won/Lost → CSM Handover

### Foundation Agent (Build First)
0. **Context Ingester** — the foundation layer. Takes a single seed (domain, email, or company name), searches all connected sources, builds a Deal Profile, and curates relevant context. Every other agent consumes its output.

### Horizontal Agents (Cross-Stage)
1. **Call Prep Agent** — pre-call briefs based on deal context
2. **Post-Call Summarizer** — transcripts → structured summaries + qualification updates
3. **Follow-Up Email Drafter** — context-aware follow-ups after any interaction
4. **Deal Risk Monitor** — flags stalling deals, engagement drops, competitor signals
5. **Stakeholder Tracker** — maps people, roles, sentiment, engagement across the deal
6. **Next Best Action Engine** — "here's what to do next on this deal and why"

### Agent Output Chaining
Context Ingester is the true foundation — it builds the Deal Profile that all agents consume. Post-Call Summarizer is the keystone horizontal agent — its output feeds into Follow-Up Drafter, Stakeholder Tracker, Deal Risk Monitor, and Call Prep.

### Context Ingester — Two-Step Architecture
The Context Ingester is split into two steps to minimize token cost and enable incremental updates:

**Step 1: Deal Profile (source index)**
- User provides ONE seed (domain, email, or company name)
- 3-pass discovery: internal sources → cross-enrichment → web enrichment
- Collects identifiers ONLY: meeting IDs, channel IDs, page IDs, contact emails, calendar events
- Does NOT read content (no transcripts, no messages, no page content)
- Output: Deal Profile with company info, contacts, and source index
- Sources: Fireflies, Slack, Notion, Google Calendar, Web

**Step 2: Context Pull (separate agent, built next)**
- Takes the Deal Profile as input
- Uses source index to read and synthesize actual deal content
- Can run incrementally (timestamp-based or event-triggered)
- This is where deal signals, risk indicators, summaries live

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

### Pages
- Home / Landing
- Why (the story, first principles philosophy)
- Agents (showcase of all six horizontal agents)
- Individual agent pages (setup, examples, connectors)
- How It Works (architecture)
- Blog (build-in-public updates)

### Tech
- Built with frontend-design skill
- Static site, deployable to Vercel/Netlify
- Content-driven, not a SaaS app (yet)

## Project Structure
```
Builder/
├── CLAUDE.md
├── README.md
├── .gitignore
├── website/              ← personal brand site
├── agents/               ← the agents
│   ├── context-ingester/    ← FOUNDATION — build first
│   ├── call-prep/
│   ├── post-call-summarizer/
│   ├── follow-up-drafter/
│   ├── deal-risk-monitor/
│   ├── stakeholder-tracker/
│   └── next-best-action/
├── frameworks/           ← first principles framework definition
├── connector-guides/     ← how to wire up data sources
└── examples/             ← full deal walkthroughs
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
- Phase: Building Context Ingester (Agent #0)
- Next: Post-Call Summarizer, then website
