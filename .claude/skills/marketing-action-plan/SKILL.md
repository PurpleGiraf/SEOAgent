---
name: marketing-action-plan
description: >
  Purple Giraffe's consultant skill for producing a client-ready Marketing Action Plan
  (MAP) — the branded, structured strategic document Purple Giraffe delivers to new
  and existing clients. Use when the user gives details about a client (business
  info, a URL, competitors, industry) and asks for a "marketing action plan", "MAP",
  "client report", "audit and strategy for [client]", or says something like "we've
  got a new client, create their marketing plan". Produces a Word (.docx) document
  matching Purple Giraffe's exact brand styles, header/footer, and the audit →
  strategy → actions structure used in real delivered MAPs. For a lighter-weight,
  non-branded, ad-hoc report spanning a couple of marketing areas, use
  marketing-beast instead — this skill is specifically for the formal client
  deliverable.
---

# Marketing Action Plan (Purple Giraffe consultant)

Produces a Purple Giraffe-branded, client-ready Marketing Action Plan as a `.docx`,
following the exact structure and style of real delivered MAPs (Duco Limited, History
Trust of South Australia) and grounded in Purple Giraffe's actual service offerings
and constraints.

Read these references before drafting, in this order:
1. `references/service-context.md` — who Purple Giraffe is, what it sells, commercial
   constraints, known gaps. Keeps recommendations realistic and internally consistent.
2. `references/map-structure.md` — the canonical document skeleton, the repeating
   Audit → Strategy → Actions unit, which sections are always-included vs. conditional
   on client type.
3. `references/brand-style-guide.md` — exact colours, fonts, and named Word styles to
   apply, plus the docx editing workflow.

## Step 1 — Intake

Collect from the user (ask only for what's genuinely missing, don't block on
everything):
- Client name, industry/sector, and a short description of what they do
- Client website URL (if any) — needed for the digital media audit
- Known competitors, or permission to identify them via research
- Anything the client has already told Purple Giraffe about goals, budget, target
  market, or pain points
- Client type signals that affect which conditional sections apply — public
  sector/not-for-profit (→ ESG, Public value, Accessibility, DEI), multi-brand
  portfolio (→ brand hierarchy handling), simple single-entity commercial (→ lean plan)

If the client has a URL, do not skip straight to writing — run real evidence
collection first (Step 2). A MAP with fabricated metrics is a real failure mode seen
in the source proposal template (see `service-context.md`) — don't repeat it.

## Step 2 — Research and evidence collection

Use the installed skills as the analytical engine — don't re-derive their expertise
here:

- **Website/technical/SEO audit**: use the `seo` skill's scripts directly for
  deterministic evidence (`fetch_page.py`, `parse_html.py`, `robots_checker.py`,
  `pagespeed.py`, `security_headers.py`, `broken_links.py`, `social_meta.py`,
  `internal_links.py`) — these feed the Digital media → Websites and SEO content
  sections.
- **AI search / GEO readiness**: `ai-seo` skill — feeds a "SEO & GEO" callout inside
  Digital media if relevant to the client's sector.
- **Competitor research**: `competitor-profiling` for deep dossiers on named
  competitors, `competitors` skill's framing for how to structure the comparison.
  Competitor URLs must come from the user or genuine web research — never invented.
- **Industry research**: use web search for sector trends, regulatory environment,
  market size/growth — cite real, checkable sources exactly as both real MAPs do
  (numbered footnotes to real URLs). This feeds Appendix 1.
- **Positioning/ICP**: `product-marketing` skill if `.agents/product-marketing.md`
  doesn't already exist for this client.
- **Social media audit**: check the client's actual social platforms (followers, post
  frequency, content themes, engagement) rather than guessing.
- **Analytics**: if the client has shared GA4/GSC access or exported data, use it
  directly; otherwise mark those KPI rows "Baseline to be established" per
  `map-structure.md`'s rule against fabricated metrics.

Bound this like the `seo` skill does: retry a failed evidence source once, then
finalize with that item marked an environment/data limitation rather than looping.

## Step 3 — Draft the content

Follow `map-structure.md` section by section. For every "Marketing audit, strategy and
actions" topic, always produce the repeating unit in order: `[Topic]` → **Audit**
(current-state only, evidence-backed) → **Strategy** (recommended direction) →
**`[Topic] actions`** (concrete, assignable bullets). Include only the topics relevant
to this client — don't force every topic from the reference list into every plan.

Cross-check against `service-context.md` as you draft:
- Don't recommend a channel/service PG doesn't offer without flagging it as a gap.
- Respect the 3-month minimum on paid/social/SEO retainers when recommending timelines.
- Never include PG's own service fees in the MAP — a MAP is strategy for the client's
  marketing, not a PG sales proposal.

Build the Marketing and communication calendar of activities last, once all actions
are finalized — every action bullet from every callout box must appear in the
calendar, and nothing should appear in the calendar that isn't backed by an action
bullet.

## Step 4 — Produce the branded `.docx`

Follow `brand-style-guide.md`'s workflow exactly:
1. Copy `assets/PG-MAP-reference-template.docx` to the client's output filename.
2. Use the docx skill's **editing an existing document** path (unzip →
   edit `word/document.xml` → rezip) — not create-from-scratch — so the existing named
   styles (`PGHeading1/2/3`, `PGBodycopy`, `PGAction`, `PGActionbullets`,
   `PGTableheading`, `PGTabletext`, `PGBullets`, `PGHyperlink`), header/footer images,
   and cover page survive intact.
3. Update the cover page (client name/logo, date) and the running header
   (`header2.xml`) with the client name.
4. Replace body content, referencing existing style IDs only — never invent new
   formatting.
5. Verify with `soffice --headless --convert-to pdf` + `pdftoppm`, then visually
   check several pages (cover, a KPI table, an actions callout box, the calendar)
   against the style guide before calling it done.

## Step 5 — Pre-delivery QA

Before presenting the finished MAP, check:
- No leftover placeholder text, no other client's name/data anywhere in the document
  (a real failure mode found in Purple Giraffe's own proposal template).
- Every action box bullet appears in the calendar and vice versa.
- No fabricated metrics — every number is either sourced from real evidence collected
  in Step 2 or explicitly marked "Baseline to be established"/"TBC".
- No PG service pricing anywhere in the document.
- Conditional sections match the client type (don't include ESG/DEI/Accessibility for
  a small private commercial client; don't omit them for a public-sector client if
  relevant).

Then tell the user exactly what was produced, the file path, and explicitly list any
KPIs/sections marked as needing a baseline or further client input.
