<!-- Added: 2026-08-31. Source: the `pg-map-creation` account-level skill,
Kelly's own captured process from a real completed engagement (Companion
Software MAP, Aug-Sep 2026) — more current than the rest of this skill's
reference material, which was built from the master template + Lynda's
process transcript before any real MAP had gone through this pipeline.
This file captures the parts of that process that are universally
applicable regardless of which tooling builds the document. Where it
conflicts with `map-structure.md`'s detailed section list, see
"Reconciling with map-structure.md" at the bottom. -->

# PG MAP creation standards (from a real completed engagement)

## Source collection protocol

Before any writing begins, request or locate, from the client: discovery
meeting transcript/notes (the primary source of voice, context, intent),
existing marketing documents (website copy, proposals, brochures,
capability statements), licence/service agreements (for accurate
product/service descriptions), RFP responses or pitch documents, LinkedIn
profiles for key principals, Google Business Profile listing, existing
social content, citation/directory audit data if one exists, current
pricing/packaging information. From Purple Giraffe: the signed proposal
for this engagement, any previous MAP examples for reference, SEMrush
access, the current PG Style Guide.

**Immediately flag to a human if any of these are missing. Do not write
speculative content to fill a gap — ask first.** This is the same
discipline as the rest of this skill, restated from real production use.

Build a knowledge directory listing every source file, what it contains,
and its reliability rating (primary source / secondary / inferred) — this
becomes the audit trail for every claim in the MAP. Format:

```
## Source: [File or URL name]
- Type: Primary / Secondary / Inferred
- What it contains: [Brief description]
- Key facts extracted: [Bullet list]
- Reliability: High / Medium / Low
- Gaps or caveats: [Any limitations]
```

## Data requirements matrix

Before writing, list every data point the MAP will need and mark each:
**Available** (found in source materials), **Ask [client contact]** (not
in sources, needs confirming), or **Estimate acceptable** (industry
standard or clearly inferable — flag it as an estimate). **Never write a
figure, statistic, or claim marked "Ask" as if it's confirmed** — flag it
explicitly in the draft.

## Positioning pack — answer from research, don't speculate

- What category is the client actually in?
- Who are the real alternatives (not just named competitors)?
- What do customers switch *from* to get to this product/service?
- What are the provable, specific differentiators (not generic claims)?
- What is the primary audience, and what do they care about most?
- What is the current brand perception vs. desired brand perception?

## MAP-specific voice rules (in addition to general PG voice)

The MAP is a professional strategic document, not a website or blog post
— the general PG voice (pg-humanizer/pg-voice-quickref) applies, but
calibrated:

- **No italics anywhere.** Firm rule from Lynda. Bold for emphasis only
  where essential.
- **No em dashes.** Replace with a comma, full stop, or colon.
- **Sentence case for all headings** — not Title Case.
- Contractions are acceptable in body copy, used moderately — this is
  still a formal strategic document.
- **Third person throughout the MAP body.** "[Client] should..." not "You
  should..." The discovery section and direct quotes are the only
  exceptions.
- **No hedging language.** "Could potentially consider exploring" →
  "Should." If you're not confident enough to make a direct
  recommendation, flag it rather than hedge it in the text.
- **No AI vocabulary**: leverage, delve, multifaceted, robust, landscape,
  ecosystem, testament to, showcase. Check every paragraph.
- **Strategic framing over activity framing.** Not "Run a quarterly
  newsletter" — instead "A quarterly newsletter addresses the gap between
  implementation and renewal, the period when clients are most likely to
  assess alternatives." The activity follows from the strategy, stated
  first.
- **Australian English**: colour, organisation, programme, recognise,
  behaviour, centre.

## Section-writing rules

- Open each section with the strategic point, not context. If the point
  is "the website is underselling the product," that's sentence one — not
  a history of when the website was built.
- Every recommendation needs a rationale: what's the evidence, what does
  it address, what does it unlock.
- Budget: never write specific figures without client confirmation. Label
  indicative figures explicitly as indicative, and flag that confirmation
  is required before any commitment.

## Methodology improvements to apply (benchmarked against Dunford, Binet &
Field, McKinsey CDJ, Gartner)

- **Message hierarchy**: a clear hierarchy between Positioning and
  Programme Detail — primary claim, proof layer, secondary messages.
- **Buying journey (B2B)**: not a linear funnel. Where relevant, describe
  the buying process as jobs-to-be-done at each stage, not just
  awareness/consideration/conversion.
- **Intent segmentation**: if the client has meaningfully different
  prospect types, segment programme recommendations accordingly rather
  than treating all prospects the same.
- **Benchmarks**: anchor recommendations to industry benchmarks where they
  exist — gives the client something to aim at, makes the recommendation
  credible.
- **Go/no-go gates**: explicit decision points in the roadmap — "By Month
  3, if traffic has not increased by X, review the SEO programme before
  Phase 2." Makes the MAP a living document, not a one-off.
- **Proof layer for differentiators**: every differentiator claim needs a
  proof point — a client quote, a retention rate, an SLA, a case study.
  Without proof, a differentiator is just an assertion.

## Quality checklist before delivery

- [ ] Every claim is sourced (check the knowledge directory)
- [ ] No second-person "you" in the MAP body
- [ ] No em dashes anywhere
- [ ] No italics
- [ ] All headings are sentence case
- [ ] Budget figures are labelled "indicative" or confirmed with client
- [ ] KPIs have actual metrics attached, not just "increase awareness"
- [ ] Implementation roadmap phases are realistic for this client's
      resources
- [ ] Header/footer present with correct logo, page numbers correct
- [ ] Document visually verified page by page
- [ ] No AI vocabulary (run the pg-humanizer 33-pattern audit)
- [ ] Strategic framing used throughout
- [ ] Third person used consistently in the MAP body

## Lynda's standards (distilled from her own annotation set)

- The MAP must tell a coherent story, not just be a list of activities.
- Each section must earn its place — no padding, no filler.
- Recommendations must flow logically from the audit findings.
- Every figure, statistic, and claim must be sourced.
- No estimates presented as facts.
- No generic industry claims — if it's in the MAP, it's about this
  client specifically.

## Escalation rules

- Can't find a piece of information in any source file: don't estimate.
  Flag it as "not available in source materials" and add it to the ask
  list.
- Source materials contradict each other: note the contradiction and ask
  a human to confirm before proceeding.
- A recommendation has no evidential basis in the research: don't write
  it. Either find the evidence or flag it.
- Unsure whether a tone or content choice meets Lynda's standard: flag it,
  don't guess. A wrong guess costs more to fix than asking once upfront.

## Reconciling with `map-structure.md`

This document's source (`pg-map-creation`) describes a 15-section
high-level narrative structure (Executive Summary, About, Market Context,
Audience, Product/Service Overview, Competitive Landscape, Current
Marketing Audit, Brand and Positioning, Digital Presence, Strategic
Recommendations, Programme Detail, Implementation Roadmap, Budget,
Measurement and KPIs, Next Steps) drawn from one real engagement.
`map-structure.md` in this skill documents a more granular topic-by-topic
structure straight from the master template. Both are legitimate — MAP
structure is genuinely engagement-specific (`pg-map-creation` itself says
to read the per-engagement "MAP Instructions" document first, when one
exists). Default to `map-structure.md`'s detailed structure for what
topics to cover, but apply this document's voice rules, quality
checklist, and methodology improvements regardless of which structure is
used, and check for an engagement-specific instructions document before
assuming either structure applies verbatim.
