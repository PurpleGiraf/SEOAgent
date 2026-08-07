<!-- Updated: 2026-08-07 -->
# Purple Giraffe brand style guide (extracted from source .docx)

Extracted directly from the OOXML of a real, delivered Marketing Action Plan. These
are exact values, not visual approximations — use them verbatim when generating or
editing a MAP.

`assets/PG-MAP-reference-template.docx` is a **stripped skeleton**, not the original
worked example — all client-specific content (business detail, audit findings, SWOT,
competitor data, KPI targets) has been removed and replaced with generic placeholder
text, since the source document was a real client's confidential MAP. What survives
intact: every named paragraph/table style, the header/footer (including the contact
bar images), the cover-page structure, and one example of each content pattern
(heading hierarchy, Audit/Strategy/Actions unit, KPI table, calendar table). Use it as
a formatting reference and a starting point to duplicate — never as source content.

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

## Workflow for producing a new MAP as a branded .docx

Follow the **docx skill's "editing an existing document" path** (unzip → edit
`word/document.xml` → rezip), not the docx-js "create from scratch" path — creating
from scratch cannot reproduce these named styles, the header/footer images, or the
cover page layout.

1. Copy `assets/PG-MAP-reference-template.docx` to the new client's output filename.
2. Unpack it (`unzip`), strip symlink entries per the docx skill's guidance (untrusted
   external party file handling doesn't apply here since this is our own asset, but
   still follow the unzip/edit/rezip mechanics exactly).
3. Replace the cover page client name/logo and title/date.
4. Replace the TOC and all body content in `word/document.xml`, but reuse every
   `w:pStyle`/table style referenced above — do not introduce new formatting.
5. Update `header2.xml`'s running client name.
6. Rezip, then verify with `soffice --headless --convert-to pdf` +
   `pdftoppm` and visually check a handful of pages against this guide before
   delivering.
