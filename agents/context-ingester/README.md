# Context Ingester

The foundation agent. Give it a company name, domain, or email address — it searches your connected tools and the web, then builds a **Deal Profile** (source index) you can use across all other agents.

## Two-Step Architecture

The Context Ingester works in two steps:

### Step 1: Deal Profile (this agent)
Builds the **source index** — maps WHERE the deal lives across your tools, WHO the contacts are, and the exact lookup keys to find them later. Fast, cheap, reusable.

### Step 2: Context Pull (separate agent, coming next)
Uses the Deal Profile to actually **read and synthesize** deal content — call summaries, Slack threads, deal signals, risk indicators. Can run incrementally ("just pull new stuff since last time").

**Why two steps?**
- Step 1 is cheap to run and rarely changes (new contacts don't appear daily)
- Step 2 can be expensive but only needs to run when you need fresh context
- Step 2 can run incrementally — no need to re-read everything each time
- Different agents need different context depths from Step 2

## What Step 1 Produces

A Deal Profile containing:
- **Company identity** — name, domain, description, industry, size, funding, HQ, LinkedIn
- **Contacts** — name, email, role, LinkedIn URL, confidence, which sources they appear in
- **Source index** — exact lookup keys per tool (meeting IDs, channel IDs, page IDs, calendar events)
- **Gaps** — sources searched but empty, or not connected
- **Excluded matches** — things found but flagged as uncertain

See [context-schema.md](./context-schema.md) for the full object definition.

## Connected Sources

The agent works with whatever you have. It gracefully handles missing sources.

| Source | What It Indexes | Connector |
|---|---|---|
| Fireflies | Meeting IDs, participant emails, dates | Fireflies MCP |
| Slack | Channel IDs, DM users, search terms | Slack MCP |
| Notion | Page IDs, titles | Notion MCP |
| Google Calendar | Event dates, titles, attendee emails | Google Calendar MCP |
| Web | Company info, LinkedIn URLs | Built-in web search |

**Don't have a connector?** No problem. The agent notes it as a gap and moves on.

## How To Use

### Option 1: Claude Project (Easiest — no code)

1. Go to [claude.ai](https://claude.ai) → Create a new Project
2. Upload `prompt.md` as a Project Knowledge file
3. Start a conversation: *"Build a deal profile for acme.com"*

**Best for:** Quick use, trying it out.

**Limitation:** No MCP tool access — the agent will ask you to paste search results.

### Option 2: Claude Code (Best experience)

1. Clone this repo
2. `cd agents/context-ingester/claude-code`
3. Run `claude` in your terminal
4. Say: *"Build a deal profile for acme.com"*

**Best for:** Users with Claude Code and MCP servers configured. The agent searches your tools directly.

### Option 3: Agent SDK App (For developers)

Coming soon. The SDK version will support:
- Programmatic deal profile creation
- Chaining into Step 2 and downstream agents automatically
- Batch processing multiple deals

## Example

See [examples/example-output.md](./examples/example-output.md) for a full example of what a Deal Profile looks like.

## Incremental Updates

The Deal Profile supports two update modes:

1. **Add to index** — "I met someone new at Acme, add them" → append to contacts and source index, no full re-run needed
2. **Re-run discovery** — "Do a fresh search" → re-run the 3-pass playbook, merge new findings, flag what's new

## What's Next After Building a Profile?

The Deal Profile feeds into every other agent as their starting context:

- **Step 2: Context Pull** → reads the actual content from indexed sources
- **Call Prep Agent** → uses contacts + company info for pre-call briefs
- **Post-Call Summarizer** → uses contacts to identify speakers
- **Deal Risk Monitor** → uses source index to track engagement
- **Stakeholder Tracker** → uses contacts as the starting map

Build the index once, use it everywhere.
