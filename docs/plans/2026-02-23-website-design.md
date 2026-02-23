# Website Design: pratyushkukreja.com

**Date**: 2026-02-23
**Status**: Approved

## Summary

Personal brand site for Pratyush Kukreja. Showcases the B2B Sales Agents Toolkit as the flagship project. Serves two audiences: B2B salespeople (who use the tools) and GTM/SaaS peers (who follow the thinking).

## Decisions

| Decision | Choice |
|----------|--------|
| Format | Single HTML file |
| Flow | Hero → Why → Agents (story-first) |
| Aesthetic | Dark bg + monospace accents + sans-serif body |
| Accent color | Blue `#60a5fa` |
| Sections | 3 sections, anchor-linked from fixed nav |
| Deploy | GitHub Pages (from `website/` directory) |
| Domain | pratyushkukreja.com (custom domain on GitHub Pages) |

## Page Structure

### Nav (fixed top)
- Left: `PRATYUSH KUKREJA`
- Right: `Why` / `Agents` / `GitHub` / `LinkedIn`
- Minimal, always visible

### Section 1: Hero (above the fold)
- Headline: "First principles over frameworks."
- Subtext: Pratyush Kukreja — GTM operator, 3x 0-to-$15M in SaaS. Building open-source AI agents that help B2B sales teams execute deals without checkbox qualification frameworks.
- Two CTAs: [Read Why ↓] [View Agents ↓]

### Section 2: Why (philosophy + story)
- Manifesto-style copy, not a wall of text
- The problem: MEDDPIC/BANT are checkbox exercises, not thinking tools
- The philosophy: assess every deal from first principles
- What we're building: intelligence layers, not integrations
- Background: Head of CS & Growth @ Reo.dev

### Section 3: Agents (the toolkit)
- Card grid (2-3 columns desktop, stacked mobile)
- Context Ingester: live badge, GitHub link, one-line description
- Other 6 agents: "coming soon" state, greyed out
- Each card uses `$` prompt prefix for terminal feel
- Link to GitHub repo

### Footer
- Social links: GitHub, LinkedIn, Twitter
- "Built in public with Claude"

## Visual Design

- **Background**: near-black (`#0a0a0a`)
- **Primary text**: off-white (`#e0e0e0`)
- **Accent**: blue (`#60a5fa`) for links, CTAs, highlights
- **Headings**: monospace (JetBrains Mono or IBM Plex Mono)
- **Body**: sans-serif (Inter or system font)
- **Terminal touches**: `>` prompt chars, `//` comment markers, code-block card styling
- **Interactions**: smooth scroll only, no JS framework
- **Responsive**: mobile-first, works on all screen sizes

## Technical

- Single `index.html` in `website/` directory
- No build step, no dependencies, no framework
- CSS embedded in the file (or single linked stylesheet)
- Google Fonts for JetBrains Mono + Inter
- GitHub Pages serves from `website/` folder
- Custom domain: pratyushkukreja.com via DNS CNAME

## What's NOT in scope

- Blog (add later when there's content)
- Individual agent detail pages (link to GitHub instead)
- JavaScript interactivity beyond smooth scroll
- Analytics (can add later)
- Contact form
