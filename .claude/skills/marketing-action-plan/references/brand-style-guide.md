<!-- Updated: 2026-08-07 -->
# Purple Giraffe brand style guide (extracted from source .docx)

Extracted directly from the OOXML of a real, delivered Marketing Action Plan. These
are exact values, not visual approximations — use them verbatim when generating or
editing a MAP.

`assets/PG-MAP-reference-template.docx` is Purple Giraffe's **actual master MAP
template** — the real file consultants copy into a client folder and rename to start
a new MAP. It contains no client data, only placeholder text (`<Insert company name
here>`, `BUSINESS NAME`, etc.) and Word comments with Lynda Schenk's (founder)
instructions for completing each section. See `section-playbook.md` and
`map-structure.md` for that guidance distilled into this skill's own workflow.

**Before delivering a MAP built from this template**: every blue instructional
comment/placeholder must be actioned and deleted — see `service-context.md`'s
formatting rule. Never send a client a document with template comments still
attached.

## Colours (hex)

| Role | Hex | Used for |
|---|---|---|
| Primary deep purple | `#2E1A47` | Page/section titles (PG Heading 1), hyperlinks, table header fills, footer bar base |
| Muted mauve | `#948794` | Subheadings (PG Heading 2 — e.g. "Branding and positioning"), the bold intro line and bullets inside every "[Topic] actions" callout box |
| Blob-pattern accent | `#433159` / `#423159` | Secondary shade inside the footer's organic blob texture — do not attempt to recreate the blob artwork; reuse the footer image asset instead |
| Body text | `#343433` | PG Body copy, general paragraph text |
| Bullet text | `#262626` | PG Bullets (standard, non-action bullet lists) |
| Table header text | `#FFFFFF` | Text inside purple-filled table header rows |

Do not invent new brand colours. If a new UI element is needed, reuse one of the above.

## Typography

All type is **Arial** (not a display/brand font — despite "Nordique Pro" appearing in the
document's font table, it is only used inside embedded graphic captions, e.g. the Q1–Q4
roadmap wave graphic — the running document body and headings are Arial throughout).

| Named style (`w:styleId`) | Font | Size | Weight/colour | Use |
|---|---|---|---|---|
| `PGHeading1` | Arial | 16pt | Bold, `#2E1A47` | Page/section titles ("Purpose", "Digital media", etc.) |
| `PGHeading2` | Arial | 12pt | Bold, `#948794` | Grey subheadings within a section (e.g. "Branding and positioning", "Website") |
| `PGHeading3` | Arial | 11pt | Bold | Inline labels: "Audit", "Strategy" |
| `PGBodycopy` | Arial | 10pt | `#343433`, +0.1pt tracking, ~1.08 line height | Standard paragraph text |
| `PGBullets` | Arial | 10pt | `#262626` | Standard bullet lists |
| `PGAction` | Arial | 10pt | Bold, `#948794` | The heading line of an actions callout box, e.g. "Website actions" |
| `PGActionbullets` | Arial | 10pt | `#948794` | The bullet items inside an actions callout box |
| `PGTableheading` | Arial | 10pt | White | Table header row cells (on `#2E1A47` fill) |
| `PGTabletext` | Arial | 10pt | inherits | Table body cells |
| `PGHyperlink` | Arial | — | `#2E1A47`, underlined | Links |

## Applying styles when editing the template's `document.xml`

Reference an existing style by `w:styleId` inside a paragraph's `<w:pPr>`:

```xml
<w:p><w:pPr><w:pStyle w:val="PGHeading2"/></w:pPr><w:r><w:t>Website</w:t></w:r></w:p>
```

Do not redefine these styles — they already exist in `styles.xml` inside the template.
Only ever add new paragraphs/tables that *reference* the existing style IDs.

## Header, footer, and cover page

- **Cover page**: full-bleed photo background (laptop/plant/desk lifestyle shot — reuse
  the existing image or source a similarly-toned stock photo), client logo on a white
  card, "Marketing Action Plan" title in grey, month/year subtitle, footer contact bar.
- **Running header** (every content page, `header2.xml`): `<Client name> | Marketing
  Action Plan | page <n>` — right-aligned, grey/muted text, thin rule beneath.
- **Running footer** (every content page, `footer1.xml`): the purple blob-pattern bar
  with office phone numbers, `purplegiraffe.com.au`, and the Purple Giraffe giraffe
  logo — reuse the existing footer image assets (`media/image4.png`/`image5.png`)
  rather than recreating them.
- Cover page uses a different, simpler header/footer pair (`header1.xml`/`footer2.xml`)
  with no running text.

## Header/footer structure (real master template)

The master template uses **Different First Page** plus later section breaks,
not a simple two-section cover/content split:

- Page 1 (cover) uses `header3`/`footer3` (the "first" header/footer).
- The rest of the first section uses `header2`/`footer2` (the "default"
  header/footer — this carries the running `<Client> | Marketing action
  plan | page N` text).
- `header1`/`footer1` are the "even" page variants (present for
  double-sided printing support; leave as-is).
- Later sections (the document has 5 total, reflecting historical section
  breaks around the audit/digital-media/appendix content) reference
  `header4`, `header5`, `header6`, and `footer4`, `footer5` — leave their
  `headerReference`/`footerReference` wiring untouched; only edit the
  **text content** inside each header/footer XML part, never the section
  structure itself.

## Workflow for producing a new MAP as a branded .docx

Follow the **docx skill's "editing an existing document" path** (unzip → edit
`word/document.xml` → rezip), not the docx-js "create from scratch" path — creating
from scratch cannot reproduce these named styles, the header/footer images, the
section structure above, or the cover page layout.

1. Copy `assets/PG-MAP-reference-template.docx` to the new client's output filename.
2. Unpack it (`unzip`), strip symlink entries per the docx skill's guidance.
3. Replace the cover page client name/logo and title/date (`<Insert company name
   here>` placeholders).
4. Fill in body content section by section per `map-structure.md` and
   `section-playbook.md`, reusing existing `w:pStyle`/table styles only —
   never introduce new formatting. **Delete every section that doesn't apply
   to this client** (see the delete-if-not-applicable list in
   `map-structure.md`) rather than leaving it blank.
5. Update every `<Client name>`/`BUSINESS NAME`/`COMPANY NAME` placeholder
   occurrence across `document.xml` **and** every header part
   (`header1.xml`–`header6.xml`) — grep for the placeholder strings to find
   them all rather than assuming they're only in the running header.
6. **Strip every Word comment and its anchor markers** (`word/comments.xml`,
   `commentsExtended.xml`, `commentsIds.xml`, `commentsExtensible.xml`, and
   the `<w:commentRangeStart>`/`<w:commentRangeEnd>`/`<w:commentReference>`
   markers in `document.xml`) before delivery — this is Purple Giraffe's own
   hard rule (see `service-context.md`), not optional cleanup. A MAP with
   template comments still attached has not been finished.
7. Rezip, then verify with `soffice --headless --convert-to pdf` +
   `pdftoppm` and visually check a handful of pages against this guide before
   delivering. If PDF conversion isn't available in the runtime environment,
   validate via OOXML schema validation and a full read-back instead, and
   say so explicitly rather than claiming a visual check happened.
