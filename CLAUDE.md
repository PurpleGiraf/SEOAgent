# Purple Giraffe — AI Agent Team

This repo hosts Purple Giraffe's Claude-based agency agent team, alongside
the existing marketing skill library (`.claude/skills/`) and the
`marketing-action-plan` MAP-build agent pool (`.claude/agents/map-*.md`).
This file defines the operating rules shared across every agent in the
agency team — individual agent files (`.claude/agents/*.md`) reference this
instead of restating it.

## Current build status

**All 12 agents are built.** Phase 1 (the core loop — Agency Lead, Client
Intelligence, Researcher, Strategist, Content, Brand QA) and Phase 2/3
(Account Manager, Social, Search Visibility, Paid Media, Creative
Director, Analytics & Reporting) all exist under `.claude/agents/`.

The spec's own guidance was to prove Phase 1 reliably before adding the
rest — that validation hasn't happened yet, so treat the Phase 2/3 agents
as built-but-unproven: their file structure, rules, and handoffs follow
the same conventions as Phase 1, but they haven't been exercised on a real
task. Test the full chain (a dummy client request through Agency Lead)
before relying on this for real client work, and expect to tighten
individual agent files once real usage surfaces gaps.

Several Phase 2/3 agents are recommendation-first by design regardless of
testing status — `paid-media` never spends money, `content`/`social`
never publish — that's a permanent property (see Human Approval Model
below), not a temporary limitation to relax once proven.

**Connector status**: `account-manager` and `creative` have live tool
access (ClickUp, Canva) since those are already connected in this
environment. `search`, `paid-media`, and `analytics` are written to
degrade gracefully to client-provided exports until GA4/Search
Console/Google Ads/Meta/LinkedIn/SEMrush connectors are added (see spec
Section 17 for the planned integration list) — don't assume they have
live platform access.

**Relationship to the MAP agent pool**: `.claude/agents/map-*.md` is a
separate, deep specialist pipeline for producing Purple Giraffe's branded
Marketing Action Plan document specifically (see
`.claude/skills/marketing-action-plan/`). Agency Lead should route a
request that's specifically "build/update a MAP for [client]" to
`map-orchestrator` rather than re-deriving that workflow through the
Researcher/Strategist/Content agents — don't duplicate that pipeline.

## Client Brain — where client knowledge lives

Each client has a folder at `clients/<client-slug>/` (copy
`clients/_TEMPLATE/` to start one). This is the **source of truth** every
agent must check before generating anything client-specific:

```text
clients/<client-slug>/
├── client.md          organisation, stakeholders, engagement status
├── brand.md           visual identity, positioning, voice summary
├── audience.md        segments, personas, audience research
├── products.md        products/services, sales channel mix
├── competitors.md     client-named competitors + intel (see hard rule below)
├── positioning.md     positioning statement, messaging pillars, funnel strategy
├── tone-of-voice.md   voice attributes, vocabulary, good/bad examples
├── approved-claims.md HUMAN-AUTHORITY — see below
├── restrictions.md    HUMAN-AUTHORITY — see below
├── campaigns/         per-campaign briefs, strategy, results
├── meetings/          meeting summaries (Account Manager, once built)
├── research/          dated research findings
└── performance/       analytics/reporting snapshots (once Analytics is built)
```

**This folder is git-ignored except `_TEMPLATE/`.** Real client data isn't
committed to this repo — it's confidential and not yet backed by a proper
data store. A Supabase migration is planned; when it lands, Client
Intelligence's read/write path changes but every other agent's contract
(read the Client Brain before acting) stays the same.

### Human-authority files

`approved-claims.md` and `restrictions.md` are **human-authority**: any
agent may propose an addition, but none may edit or remove an existing
entry without an explicit human instruction. These are the two files where
a wrong automated edit does real damage (a false approved claim or a
silently-dropped restriction becomes client-facing risk).

### Hard rule — competitors

No agent adds a competitor to `competitors.md` on its own initiative.
Competitors are listed only because the client named them, or a human
explicitly approved one after reviewing Researcher findings. This mirrors
the same rule already enforced in the MAP agent pool.

## Source-of-truth hierarchy

When agents encounter conflicting information, resolve in this order:

1. Approved client information (`approved-claims.md`, `restrictions.md`)
2. Client-provided documents (style guides, business plans, discovery-form answers)
3. Verified agency research (Researcher agent output, dated and sourced)
4. Trusted external sources
5. Agent inference
6. Agent assumption

Never let an assumption silently become a client fact. If something is
genuinely unknown, the output must say so explicitly:

```text
UNKNOWN — HUMAN INPUT REQUIRED
```

not a plausible-sounding guess.

## Human approval levels

Every deliverable carries one of three levels. Agents must state which
level applies to their output — don't leave it implicit.

- **GREEN — autonomous.** Internal research, internal summaries, competitor
  monitoring, draft analysis, internal task recommendations, data
  collection. No human sign-off needed before the next agent uses it.
- **AMBER — human approval required before anything external.** Social
  posts, client emails, campaign strategy, SEO recommendations, website
  copy, ad recommendations, creative concepts. This is most client-facing
  output — Content and Brand QA both operate here by default.
- **RED — human-only execution.** Spending client money, publishing
  sensitive content, sending legally significant communications, changing
  major strategy, contractual commitments, deleting production data, major
  changes to client accounts. No agent takes a RED action itself, ever —
  it can only recommend and hand off.

## Structured handoffs

Agents don't silently modify each other's work. When one agent's output
becomes another's input, it carries this shape (adapt fields as needed —
the point is every handoff states its dependencies and approval
requirement explicitly, not that the YAML is rigid):

```yaml
task:
  id: <short id>
  client: <client-slug>
  objective: <one line>

context:
  campaign: <if applicable>
  audience: <if applicable>

deliverable:
  type: <e.g. research-brief, strategy, content-draft, qa-review>

requirements:
  - <specific requirement>

dependencies:
  - <agent name whose output this needs>

approval:
  required: true|false
  level: GREEN|AMBER|RED
```

## Standard agent file shape

Every agent under `.claude/agents/` (this agency team; the `map-*` pool
follows its own equivalent conventions) uses: Role, Mission,
Responsibilities, Inputs, Outputs, Rules, Source of Truth, Do Not, Human
Approval, Quality Standards, Handoff. Keep new agents consistent with this
shape so Agency Lead can reason about them uniformly.

## Mistakes to avoid (learned into this file so no agent re-derives them)

- Don't create per-platform agents (Instagram Agent, Facebook Agent, etc.)
  without a demonstrated need — organise around business capability, not
  platform.
- Don't let an agent present an inference as a confirmed fact.
- Don't grant an agent tool access beyond what its role needs (least
  privilege — see each agent file's declared `tools:`).
- Don't let any agent spend client money or publish client-facing content
  autonomously — those are RED.
- Don't invoke a multi-agent chain for a simple, single-specialist task.
