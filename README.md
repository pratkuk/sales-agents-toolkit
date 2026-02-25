# B2B Sales Agents Toolkit

AI agents that think like a seasoned sales professional — not just execute like an eager intern who can type very fast.

## The Problem

Sales frameworks like MEDDPIC and BANT reduce complex human conversations to checkboxes. CRMs capture data but don't help you think. Most AI sales tools just move data between boxes faster.

The gap between "what a great seller actually does" and "what tools help you do" is massive.

## The Approach

**First principles over frameworks.** Instead of scoring deals on acronyms, these agents help you think about what actually matters:

- Does this person have a real problem we solve?
- Can they actually buy?
- Is there urgency or a triggering event?
- What's the cost of inaction for them?
- Are we talking to the right people?
- What has to be true for this deal to close?

**No vendor lock-in.** These agents don't require specific CRMs or tools. They define what context they need, and you bring the data from whatever you use.

## The Pipeline

Three agents, chained sequentially. Each agent's output is the next agent's input.

| # | Agent | What It Does | Status |
|---|-------|-------------|--------|
| 1 | **Context Ingester** | Give it a company name, domain, or email. It searches across your tools and builds a source index of where this deal lives. | Live |
| 2 | **Signal Reader** | Reads deal context — transcripts, Slack threads, meeting patterns — and surfaces signal clusters. No fixed stages. Just what's true about this deal right now. | Next up |
| 3 | **Deal Advisor** | Takes the signals and gives you a strategic read — the kind of analysis a seasoned VP Sales would give in a deal review. Not a to-do list. A way of thinking about the deal. | Next up |

### Why 3 agents, not 6?

Things like call summarization, follow-up drafting, and call prep are already well-served by base LLMs with a good prompt. The hard problem — and where real value lives — is building the pipeline from raw deal context to strategic insight. That's what this toolkit solves.

Each agent is available as:
- **Prompt template** — copy-paste into Claude or any LLM
- **Claude Agent SDK app** — runnable code with tool use and chaining

## Getting Started

Start with the **Context Ingester** in `/agents/context-ingester`. Follow its README to build your first Deal Profile.

## Project Structure

```
agents/                   — the three pipeline agents
docs/                     — website + design docs
frameworks/               — first principles framework definition
connector-guides/         — how to wire up your data sources
examples/                 — full deal walkthroughs
```

## Built By

[Pratyush Kukreja](https://pratyushkukreja.com) — building in public.

## License

MIT
