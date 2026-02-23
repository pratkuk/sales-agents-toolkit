# B2B Sales Agents Toolkit

Open-source AI agents that help B2B SaaS salespeople execute deals — from the moment an SQL lands on your plate through to CSM handover.

## The Problem

Sales frameworks like MEDDPIC and BANT reduce complex human conversations to checkboxes. CRMs capture data but don't help you think. And the gap between "what a great seller actually does" and "what tools help you do" is massive.

## The Approach

**First principles over frameworks.** Instead of scoring deals on acronyms, these agents help you think about what actually matters:

- Does this person have a real problem we solve?
- Can they actually buy?
- Is there urgency or a triggering event?
- What's the cost of inaction for them?
- Are we talking to the right people?
- What has to be true for this deal to close?

**No vendor lock-in.** These agents don't require specific CRMs or tools. They define what context they need, and you bring the data from whatever you use — Salesforce, HubSpot, Gong, Fireflies, or just your own notes.

## The Agents

Six horizontal agents that work across every stage of a deal:

| Agent | What It Does |
|---|---|
| **Call Prep** | Generates pre-call briefs with talking points and open questions |
| **Post-Call Summarizer** | Turns transcripts into structured summaries, qualification updates, and next steps |
| **Follow-Up Drafter** | Writes context-aware follow-up emails after any interaction |
| **Deal Risk Monitor** | Flags deals that are stalling, single-threaded, or showing warning signs |
| **Stakeholder Tracker** | Maps every person in the deal — their role, sentiment, and engagement |
| **Next Best Action** | Tells you what to do next on a deal and why |

Each agent is available as:
- **Prompt template** — copy-paste into Claude or any LLM
- **Claude Agent SDK app** — runnable code with tool use and chaining

## Getting Started

Pick any agent folder in `/agents` and follow its README. The **Post-Call Summarizer** is the best starting point — it's the keystone that feeds the other agents.

## Project Structure

```
agents/                   — the six horizontal agents
frameworks/               — first principles framework definition
connector-guides/         — how to wire up your data sources
examples/                 — full deal walkthroughs
website/                  — pratyushkukreja.com source
```

## Built By

[Pratyush Kukreja](https://pratyushkukreja.com) — building in public. Follow the journey on the blog.

## License

MIT
