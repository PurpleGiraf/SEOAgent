# Purple Giraffe — AI Agent Team

This repo hosts Purple Giraffe's Claude-based agency agent team, alongside
the existing marketing skill library (`.claude/skills/`) and three
dedicated deliverable pools: the `marketing-action-plan` MAP-build agent
pool (`.claude/agents/map-*.md`), a standalone SEO/AEO/GEO audit team
(`seo-*`/`aeo-geo-auditor`), an AI visibility check team (`ai-visibility-*`),
and an FAQ generation team (`faq-*`). This file defines the operating
rules shared across every agent in the agency team — individual agent
files (`.claude/agents/*.md`) reference this instead of restating it.

## Current build status

**29 agent files** under `.claude/agents/`:

- **12 agency-team roles** (general client work), 4 of which double as
  MAP pipeline agents in MAP mode (see "Consolidated agents" below)
- **4 MAP-only mechanical agents** (orchestrator, actions/calendar,
  document assembly, QA gate) with no agency-team equivalent
- **5-agent SEO/AEO/GEO audit team** — a standalone deep audit
  deliverable (`seo-audit-orchestrator` → `seo-technical-onpage-auditor` +
  `aeo-geo-auditor` + `seo-competitor-gap-auditor` → `seo-audit-report-writer`),
  distinct from `search`'s general-mode ongoing recommendations and from
  `researcher`/`search`'s MAP-mode one-time audit — this is for a
  dedicated audit ask (sales-scoping, quarterly deep-dive)
- **4-agent AI visibility check team** — measures whether a brand is
  *currently* cited by AI assistants (`ai-visibility-orchestrator` →
  `ai-visibility-query-designer` → `ai-visibility-checker` →
  `ai-visibility-report-writer`), distinct from `aeo-geo-auditor` which
  audits *readiness* to be cited, not current citation reality. Honest
  about a real limitation: only Google AI Overviews and web-findable
  signals are directly checkable from this environment — ChatGPT/
  Perplexity/Gemini/Claude's live chat answers are not, and every report
  from this team must say so explicitly rather than guessing at what
  those systems "probably" say.
- **4-agent FAQ generation team** — `faq-orchestrator` →
  `faq-question-researcher` → `faq-writer` → `faq-schema-qa`, grounded in
  real search behaviour and the `faq-writing-geo-aeo-seo` skill, checked
  against `approved-claims.md`/`restrictions.md` before reaching a human

All four deliverable-team orchestrators share the same orchestration
constraint as `agency-lead`/`map-orchestrator` (see below) — no working
`Agent` tool once invoked as a subagent, self-execution is the only mode
that exists in that case, real parallel dispatch requires the top-level
session driving it directly.

## MAP methodology — newer authoritative source

`.claude/skills/marketing-action-plan/references/pg-map-creation-standards.md`
captures the `pg-map-creation` account-level skill — Kelly's own captured
process from a real completed engagement (Companion Software MAP,
Aug-Sep 2026), postdating and more current in places than the rest of
this skill's reference material (which was built from the master template
and Lynda's process transcript before any real MAP had gone through this
pipeline). It's now wired into `map-orchestrator`, `map-document-assembly`,
`map-qa-compliance`, `researcher` (MAP mode), and `strategist` (MAP mode)
— voice rules (no italics, no em dashes, sentence case headings, third
person, no AI vocabulary), a real quality checklist from Lynda's own
annotations, escalation rules, and methodology upgrades benchmarked
against Dunford/Binet & Field/McKinsey CDJ/Gartner. Where it conflicts
with `map-structure.md`'s detailed section list, see that file's
"Reconciling with map-structure.md" section — both are legitimate, MAP
structure is genuinely engagement-specific.

Phase 1 (the core loop — Agency Lead, Client Intelligence, Researcher,
Strategist, Content, Brand QA) and Phase 2/3 (Account Manager, Social,
Search Visibility, Paid Media, Creative Director, Analytics & Reporting)
are all built. The spec's own guidance was to prove Phase 1 reliably
before adding the rest. A first end-to-end dry run (dummy client, Agency
Lead through Client Intelligence → Researcher → Strategist → Content →
Brand QA) has happened and validated the core discipline — Client Brain
built correctly from `_TEMPLATE`, no invented facts, `UNKNOWN` used
instead of guessing, client-named-competitors-only held, Brand QA caught a
real gap instead of waving it through. It also surfaced the orchestration
constraint below, which has since been fixed in
`agency-lead.md`/`map-orchestrator.md`. This was one dry run on a
fictional client, not production validation — Phase 2/3 agents (Account
Manager, Social, Search, Paid Media, Creative, Analytics) remain untested
until each has run on a real task.

## Consolidated agents — one file, two modes

The MAP pool originally had 13 agents, several of which duplicated an
agency-team agent's expertise under a different operating constraint (a
one-shot audit fixed to a document template vs. ongoing freeform client
work). Audited and merged into four files, each with a **general mode**
(default — ongoing agency work) and a **MAP mode** (invoked by
`map-orchestrator` with a specific topic assignment, audit/synthesis only,
tied to the MAP document's exact structure):

- **`researcher`** — general mode unchanged; MAP mode covers six topics
  that used to be six separate files: industry research, competitor
  research, brand & positioning audit, traditional marketing & customer
  experience audit, target market & customer journey, digital channels
  audit. `map-orchestrator` invokes it once per topic.
- **`strategist`** — general mode unchanged; MAP mode is what
  `map-strategy-synthesis` used to be (Strategy layer, SWOT, KPIs — runs
  only after every research topic above has returned).
- **`search`** — general mode unchanged; MAP mode is what
  `map-website-seo-geo-audit` used to be (one-shot Website/SEO
  content/GEO content audit).
- **`social`** — general mode unchanged; MAP mode is what
  `map-social-audit` used to be (one-shot per-platform checklist audit,
  no strategy).

Why this was safe to merge and not just cosmetic: every absorbed MAP
agent's tool list was a strict subset of its merge target's (none needed
`Write`; the general-mode agent already had it and simply doesn't use it
in MAP mode), and the underlying skill in every case was genuinely the
same — "gather current-state facts against a defined checklist, cite
sources, don't invent" for the `researcher` topics, SEO/GEO expertise for
`search`, social-platform expertise for `social`, turning research into
recommendations for `strategist`. Parallelism in `map-orchestrator`'s
research phase isn't lost — it invokes the same agent file multiple times
with different topic-scoped prompts, which works identically to invoking
different files.

**Not merged, deliberately**: `map-orchestrator`, `map-actions-calendar`,
`map-document-assembly`, `map-qa-compliance` have no agency-team
equivalent (docx assembly, template-comment stripping, calendar
reconciliation) — genuinely MAP-only mechanics, nothing to consolidate
into. `brand-qa` and `map-qa-compliance` were considered and kept separate
— one reviews prose for brand/accuracy/AI-quality/risk, the other verifies
a docx file's structural integrity; different skill types, and combining
them risked one being shortchanged when the other dominates a given
review.

## Orchestration constraint — read before invoking agency-lead or map-orchestrator

**No subagent in this environment can dispatch further subagents — ever,
regardless of how it was invoked.** The `Agent` tool is only available to
the top-level session (the actual conversation a human is having with
Claude Code). A first dry run found `agency-lead` invoked as a subagent
had no working `Agent` access despite it being declared in `tools:`; a
second test invoked `agency-lead` **directly from the top-level session**
specifically to check whether that changed anything — it didn't. The
subagent's tool set that run was `Read, Write, Edit, Grep, Glob, Skill`
with no `Agent` tool present at all, regardless of invocation depth. This
rules out the "nested vs. top-level dispatch" distinction the first fix
assumed — there is no dispatch mode available to `agency-lead` or
`map-orchestrator` once either is running as a subagent, full stop.

Practical effect: **genuine multi-agent coordination — each specialist
running in its own isolated context, independent QA review that isn't the
same context grading its own work — can only happen if the top-level
session itself calls each specialist agent directly** (`client-intelligence`,
then `researcher`, then `strategist`, then `content`, then `brand-qa`, in
order), using `agency-lead`'s Responsibilities section as the plan to
follow. `agency-lead` and `map-orchestrator`, whenever invoked via the
`Agent` tool as a subagent, always self-execute every phase in one process
by reading and following each named specialist's `.claude/agents/*.md`
file in turn — this is not a fallback for an edge case, it is the only
mode that exists for either orchestrator once dispatched as a subagent.
Both still enforce the same dependency ordering and rules internally, and
both state plainly in their output that no real isolation between phases
occurred. If independent phase isolation genuinely matters for a task,
don't invoke `agency-lead`/`map-orchestrator` as a subagent at all — drive
the sequence from the top-level session instead.

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
