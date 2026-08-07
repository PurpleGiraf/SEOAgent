---
name: marketing-beast
description: >
  Master marketing orchestrator. Use when the user gives business/product context
  (a URL, a product description, or both) and wants a comprehensive marketing
  report and action plan — not a single-channel task. Triggers include "full
  marketing report", "marketing audit", "analyze my marketing", "marketing action
  plan", "audit my business", "competitor research report", "SEO and social
  audit", "go-to-market plan", "what should I be doing for marketing", or any
  request that names multiple marketing areas at once (e.g. "SEO + social",
  "SEO and content strategy"). Classifies the request, routes to the right
  installed skills, and always produces MARKETING-REPORT.md + ACTION-PLAN.md.
  For a single specific channel/task (just SEO, just an email sequence, just ad
  copy), use that skill directly instead — this orchestrator is for requests
  that span multiple areas or ask for "everything."
---

# Marketing Beast (Orchestrator)

Routes a single piece of user-supplied context (business description, URL,
goals) through the right combination of the ~220 installed marketing skills
in `.claude/skills/`, and synthesizes the results into one coherent report and
prioritized plan. This skill does not replace the individual skills — it reads
them (via the `Skill` tool) in sequence and merges their output.

## Step 0 — Load Foundational Context

Before classifying anything, check for `.agents/product-marketing.md` (created
by the `product-marketing` skill — product, audience, ICP, positioning).

- If it exists, read it and use it as ground truth for audience/positioning.
- If it doesn't exist and the user's context doesn't already cover product,
  audience, and positioning, either:
  - Ask one clarifying question for the minimum missing piece (don't block on
    everything), or
  - Proceed with what's given and flag "Positioning/ICP not established" as a
    gap in the final report rather than guessing.

Do not run the full `product-marketing` interview mid-orchestration unless the
user explicitly asks to set up context first — that's a separate, longer flow.

## Step 1 — Classify the Request

Read the user's actual wording. Requests are rarely identical — map to **one
or more** tracks below. Multiple tracks can and should combine (e.g. "SEO and
social" = Track A + Track C). If the request is genuinely open-ended ("analyze
my business", "what should I be doing"), run the **Full-Spectrum** default
(all tracks, lighter depth per track).

| Track | Trigger signals | Primary skills |
|---|---|---|
| **A — Technical/Content SEO** | "SEO", "search ranking", "technical audit", "site speed", "schema", "sitemap" | `seo` (script-backed full audit), `schema`, `site-architecture` |
| **B — AI Search / GEO-AEO** | "AI search", "ChatGPT", "AI Overviews", "GEO", "AEO", "llms.txt", "get cited by AI" | `ai-seo` |
| **C — Social & Content** | "social media", "content strategy", "what to post", "content calendar", "blog" | `social`, `content-strategy-ch` (or `content-strategy`), `copywriting-ch`, `video`, `image` |
| **D — Competitor Research** | "competitor", "vs [product]", "competitive landscape", "who else does this" | `competitor-profiling` (research/dossier), `competitors` (comparison pages), `marketing-council` (optional: contrarian read on positioning vs. competitors) |
| **E — Paid Acquisition** | "ads", "PPC", "paid media", "Google Ads", "Meta ads", "ROAS" | `ads`, `ad-creative` |
| **F — Conversion & Pricing** | "conversion rate", "CRO", "pricing", "landing page", "signup flow" | `cro`, `pricing`, `signup`, `onboarding`, `popups`, `paywalls` |
| **G — Lifecycle & Retention** | "email", "churn", "retention", "referral", "onboarding emails" | `emails`, `churn-prevention`, `referrals` |
| **H — Full GTM / Growth Plan** | "marketing plan", "go-to-market", "GTM", "growth plan", "AARRR", "90-day plan", "what should I be doing for marketing" | `marketing-plan` (comprehensive AARRR plan — if this track fires, it becomes the report's spine; other tracks feed into its sections instead of running standalone) |
| **I — Analytics & Attribution** | "tracking", "GA4", "attribution", "which channel works" | `analytics`, `attribution` |
| **Full-Spectrum (default for vague requests)** | no clear single track, or "everything", "full audit" | A + C + D + F, plus H's AARRR structure as the report skeleton |

If Track H (`marketing-plan`) fires alongside other tracks, treat `marketing-plan`
as the **skeleton** — its AARRR sections absorb the other tracks' findings
rather than producing a second, competing structure.

## Step 2 — Collect Evidence

If a URL is provided, use the `seo` skill's scripts directly for deterministic
evidence rather than re-deriving it by hand — do not duplicate that skill's
logic here:

```bash
python3 <SEO_SKILL_DIR>/scripts/fetch_page.py <url> --output /tmp/page.html
python3 <SEO_SKILL_DIR>/scripts/parse_html.py /tmp/page.html --url <url> --json
python3 <SEO_SKILL_DIR>/scripts/robots_checker.py <url>
python3 <SEO_SKILL_DIR>/scripts/llms_txt_checker.py <url>
```

`<SEO_SKILL_DIR>` = the installed `seo` skill's directory (sibling to this
skill's directory).

For competitor tracks, competitor URLs must come from the user or from
LLM/web-search discovery — do not invent competitor names or data.

## Step 3 — Run the Routed Skills

For each track selected in Step 1, invoke the matching skill(s) with the
`Skill` tool (or read their `SKILL.md` directly) and follow their own
workflow — each already defines its own evidence, structure, and output
contract. Do not re-implement their logic here; this orchestrator's job is
routing and synthesis, not duplicating per-skill expertise.

Run tracks in this order when several are selected, so later tracks can build
on earlier findings:
1. Foundational (Step 0 context) →
2. A/B (technical + AI-search evidence) →
3. D (competitor landscape, if requested) →
4. C/E/F/G (channel-specific work) →
5. H (if selected, folds everything above into the AARRR plan) →
6. I (measurement plan for whatever was recommended)

## Step 4 — Bound the Work

- Retry a failed evidence source at most once, then finalize with that section
  marked as an environment limitation — same rule as the `seo` skill.
- Do not chain more than ~6 sub-skills for one report unless the user
  explicitly asked for "everything" — prefer depth over breadth when the
  request was scoped (e.g. "SEO and social" is 2 tracks, not 6).
- If a track's skill produces its own report file (e.g. `seo`'s
  `FULL-AUDIT-REPORT.md`), incorporate its findings by reference/summary into
  the unified report rather than duplicating its full content verbatim.

## Step 5 — Synthesize and Report

Always produce, in the current working directory:

1. **`MARKETING-REPORT.md`** — structured as:
   - Executive summary (3-5 sentences: what was analyzed, biggest opportunity,
     biggest risk)
   - One section per track that ran, each with: Finding → Evidence → Impact →
     Confidence (Confirmed / Likely / Hypothesis)
   - Cross-track patterns (e.g. "weak positioning shows up in both the
     homepage copy and the ad copy")
2. **`ACTION-PLAN.md`** — prioritized by impact and effort, structured as:
   - Now (this week)
   - Next (30 days)
   - Later (60-90 days)
   - Each item traces back to a finding in the report

3. In the final chat response, explicitly list which tracks ran, which skills
   were used, and the exact paths of both generated files.

## Rules

1. Never fabricate metrics, traffic numbers, or competitor data. Missing data
   is a stated gap, not a filled-in guess.
2. Every routed skill keeps its own domain rules (e.g. `seo`'s "FAQPage schema
   is restricted", "INP not FID") — don't override them here.
3. If the user's context is too thin to route confidently (no URL, no
   product description, no goal), ask one targeted question before running
   anything — don't guess the business into existence.
4. This skill is for multi-track requests. A single clear ask ("write me a
   cold email sequence") should go straight to the matching skill, not through
   this orchestrator.
