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
  strategy → actions structure used in Purple Giraffe's actual master MAP template.
  For a lighter-weight, non-branded, ad-hoc report spanning a couple of marketing
  areas, use marketing-beast instead — this skill is specifically for the formal
  client deliverable.
---

# Marketing Action Plan (Purple Giraffe consultant)

Produces a Purple Giraffe-branded, client-ready Marketing Action Plan as a `.docx`,
built directly from Purple Giraffe's real master template and its founder's own
documented process.

## Agent pool (for automated/batch builds)

For a full end-to-end build from a validated client discovery-form submission
(e.g. intake landing from Supabase), delegate to the `map-orchestrator` agent
(`.claude/agents/map-orchestrator.md`) instead of running this skill's steps
manually — it sequences the 12 specialist agents in `.claude/agents/map-*.md`
(research → synthesis → assembly → QA) in the correct dependency order and
stops for human sign-off before delivery. Use the manual steps below when
working a single MAP interactively in conversation, editing an existing MAP,
or when no orchestrator/agent pool is available.

## Manual workflow

Read these references before drafting, in this order:
1. `references/service-context.md` — the real research/build workflow, the tools
   consultants actually use (SEMrush, ChatGPT for GEO, PageSpeed Insights, direct
   platform access), typical time/depth, and hard data-sourcing rules.
2. `references/map-structure.md` — the canonical document skeleton (Purple Giraffe's
   actual build order and section list), the repeating Audit → Strategy → Actions
   unit, and the exact delete-if-not-applicable list.
3. `references/section-playbook.md` — section-by-section guidance (what to review,
   what tools/resources each section needs) distilled from the template author's own
   instructions.
4. `references/social-media-checklists.md` — exact per-platform audit checklists
   (Facebook, Instagram, X, LinkedIn, YouTube).
5. `references/brand-style-guide.md` — exact colours, fonts, named Word styles, the
   template's real header/footer/section structure, and the docx editing workflow.
6. `references/discovery-questions.md` — Purple Giraffe's real client discovery
   questionnaire (`assets/PG-MAP-discovery-questions.pdf`), mapped to where each
   answer feeds the MAP. This is the actual intake checklist — use it for Step 1.

## Step 1 — Intake

Work through `references/discovery-questions.md` (Purple Giraffe's real discovery
questionnaire) against what the user has already told you, and ask for what's
genuinely missing — don't block on items that are optional or that you can source
yourself:
- Business/organisation basics: mission, vision, values, business goals, what
  success looks like, USP, company history in their own words, positioning
- **Known competitors** — must come from the client/user, never invented or
  inferred (Purple Giraffe's own rule — see `service-context.md`)
- Whether the client already has a business plan or industry data to share
  (feed it in directly — faster and more accurate than researching from scratch)
- Channels to market, sales % by channel, sales % by product/service, target
  market segments and desired future split
- Brand collateral (style guide, language guide) and platform access (social,
  Google Analytics, website, EDM, CRM) — use directly wherever available; treat
  website/CRM access as nice-to-have, not blocking
- Whether this MAP accompanies an ongoing Purple Giraffe retainer engagement (only
  then does the "Marketing investment" section apply)
- Client type signals that affect which conditional sections/topics apply — see the
  delete-if-not-applicable list in `map-structure.md` (industry-specific modules,
  ESG/DEI/accessibility, which social platforms are even active, etc.)

If the client has a URL, do not skip straight to writing — run real evidence
collection first (Step 2). A MAP with fabricated metrics or invented competitors is
the single most avoidable failure mode here — don't produce one.

## Step 2 — Delete what doesn't apply, then research

Per Purple Giraffe's own process: first strip the template down to what's relevant
to this client (industry-specific modules, unused social platforms, optional
sections) before filling anything in — this keeps drafting focused and avoids
half-filled irrelevant sections.

Then collect evidence, using the installed skills as the analytical engine:

- **Website/technical/SEO audit**: use the `seo` skill's scripts directly for
  deterministic evidence (`fetch_page.py`, `parse_html.py`, `robots_checker.py`,
  `pagespeed.py`, `security_headers.py`, `broken_links.py`, `social_meta.py`,
  `internal_links.py`) — these feed the Website and SEO content sections. Purple
  Giraffe's own process uses SEMrush + Google PageSpeed Insights for the same job;
  use the closest available equivalent and say which you used.
- **AI search / GEO readiness**: `ai-seo` skill — feeds the GEO content section.
- **Competitor research**: `competitor-profiling` for dossiers on the client's named
  top 3 competitors (never more, never invented — see `section-playbook.md`).
- **Industry research**: use web search for sector trends, regulatory environment,
  market size/growth — cite real, checkable sources. This feeds About → Industry
  research and insights (client-facing summary) and Appendix 1 (full detail).
- **Positioning/ICP**: `product-marketing` skill if `.agents/product-marketing.md`
  doesn't already exist for this client.
- **Social media audit**: check the client's actual social platforms per
  `social-media-checklists.md` — audit only, no strategy yet.
- **Analytics**: if the client has shared GA4/GSC/social-insights access or exported
  data, use it directly; otherwise mark those KPI rows "Baseline to be established" —
  never fabricate a number.

Bound this like the `seo` skill does: retry a failed evidence source once, then
finalize with that item marked an environment/data limitation rather than looping.

## Step 3 — Draft the content (in Purple Giraffe's real build order)

Per the template author's own instructions, draft in this order, not top-to-bottom
reading order:

1. Current Marketing Audit (all of it — traditional + digital, per
   `section-playbook.md`)
2. About
3. Brand Strategy (Branding and positioning, through Point of difference)
4. Marketing Strategy (the Strategy paragraphs across every audited topic)
5. Marketing Activity Plan (the Actions callout boxes)
6. Marketing Strategy on a page (executive summary — can only be written once the
   above exists)
7. Marketing Calendar (consolidate every action bullet into the Qtr1–4 table)
8. Marketing Strategy roadmap (the Q1–Q4 goals-and-steps summary)
9. Table of Contents (regenerate/update last, once headings are final)

For every "Marketing audit, strategy and actions" topic, always produce the
repeating unit in order: `[Topic]` → **Audit** (current-state only, evidence-backed)
→ **Strategy** (recommended direction — only where warranted) → **`[Topic] actions`**
(concrete, assignable bullets, styled `PGAction`/`PGActionbullets`). Every action
bullet must also appear in the calendar, and vice versa.

Cross-check against `service-context.md` as you draft:
- Don't recommend a channel/topic that was deleted in Step 2.
- Only include a "Marketing investment" section if this MAP supports an ongoing PG
  retainer — otherwise it doesn't exist in this document at all.
- KPIs need a timeframe and must be achievable — not aspirational fantasy numbers.

## Step 4 — Produce the branded `.docx`

Follow `brand-style-guide.md`'s workflow exactly — copy the real master template,
edit it in place (unzip → edit XML → rezip), fill sections per Steps 2–3, delete
whatever doesn't apply, update every placeholder occurrence across `document.xml`
and all header parts, then **strip every Word comment and instructional blue-text
placeholder** before rezipping. This last step is Purple Giraffe's own hard rule,
not optional cleanup.

## Step 5 — Pre-delivery QA

Before presenting the finished MAP, check:
- **No Word comments and no blue instructional/placeholder text remain anywhere** —
  this is the single most important check per Purple Giraffe's own process.
- No leftover placeholder text or another client's name/data anywhere in the
  document.
- Every action box bullet appears in the calendar and vice versa.
- No fabricated metrics or invented competitors — every number/competitor is either
  sourced from real evidence/client input or explicitly marked "Baseline to be
  established"/"TBC".
- "Marketing investment" section present only if this is an ongoing PG engagement.
- Deleted sections were actually removed, not just left blank.
- Document is Arial throughout; images (if any) are reasonably sized, don't crowd
  the footer or the PG giraffe mark.

Then tell the user exactly what was produced, the file path, which sections were
deleted as not applicable, and explicitly list any KPIs/sections marked as needing
a baseline or further client input.
