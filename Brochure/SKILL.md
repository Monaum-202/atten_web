---
name: brochure-design
description: Design print-ready or digital brochures -- tri-fold, bi-fold, or single-page flyers -- as polished PPTX/PDF. Use for any brochure, flyer, pamphlet, leaflet, or folded handout request.
---

# Brochure Design

A brochure is a print or print-style layout problem, not a slide deck problem. The biggest risk
isn't the visual design (the `frontend-design` and `pptx` skills already cover that) -- it's
getting the physical folding logic wrong, which silently produces a brochure that reads out of
order once printed and folded. This skill exists mainly to prevent that mistake and to give you
correct panel dimensions to start from.

## Step 1: Clarify format and medium before building anything

Two quick questions decide everything else. If the user hasn't answered them, pick sensible
defaults and say so rather than blocking:

1. **Medium** -- physical print (will be folded and handed out) or digital-only (viewed as a PDF /
   emailed / posted online)? Default: print-ready, since it also works fine as a digital PDF.
2. **Format** -- tri-fold, bi-fold, or single-page flyer (no folds)? Default: tri-fold, the
   standard for a features/services brochure like a product one-pager.

Digital-only brochures don't need fold-safety margins or the panel-order logic below -- treat
them like a short, well-designed multi-section PDF instead.

## Step 2: Panel dimensions

All brochures are built as a **flat sheet**, landscape orientation, divided into equal vertical
panels. Design two full-bleed pages: **Side A (Outside)** and **Side B (Inside)**.

| Format | Flat sheet size | Panels | Panel size (each) |
|---|---|---|---|
| Tri-fold, US Letter | 11in x 8.5in | 3 | 3.667in x 8.5in |
| Tri-fold, A4 | 297mm x 210mm | 3 | 99mm x 210mm |
| Bi-fold, US Letter | 11in x 8.5in | 2 | 5.5in x 8.5in |
| Bi-fold, A4 | 297mm x 210mm | 2 | 148.5mm x 210mm |
| Flyer, US Letter / A4 | 8.5in x 11in / A4 portrait | 1 (no fold) | full page |

For a more polished tri-fold, the panel that folds inward first can be made ~0.125in narrower
than the other two so it tucks cleanly without the cover popping open. Equal thirds is fine for a
first draft or a digital-only piece.

## Step 3: Panel roles and fold order (read this before laying out content)

This is the part that's easy to get backwards. For a standard tri-fold letter brochure:

**Side A (Outside)** -- left to right on the flat sheet:
`[ Back Panel ]  [ Inside Flap / Teaser ]  [ Front Cover ]`
The Front Cover is what people see first. The Flap folds in first and tucks behind it.

**Side B (Inside)** -- left to right on the flat sheet:
`[ Inside Left ]  [ Inside Center ]  [ Inside Right ]`
This is the main spread the reader sees when they fully unfold it -- design it as one continuous
layout that happens to have two fold lines running through it, not three unrelated panels.

For landscape tri-folds, standard duplex printing flips on the **short edge**, which keeps the
left-to-right panel order identical between Side A and Side B -- i.e. **do not manually mirror or
reverse the panel order** when laying out Side B. This is the convention every mainstream trifold
template (Word, Canva, Avery, print shops) uses.

**Always verify before finalizing:** print Side A and Side B on plain paper, single-sided, lay one
on top of the other, and fold. Confirm the Front Cover is the panel someone would see holding it
in their hand, and that the inside spread reads left-to-right in order. This five-minute check
catches the one mistake that's expensive to catch after professional printing.

Bi-fold is simpler and lower-risk: two panels per side, `[Back][Front Cover]` outside and
`[Inside Left][Inside Right]` inside, no flap to worry about.

## Step 4: Content architecture (what goes where)

A reasonable default for a tri-fold marketing brochure:

- **Front Cover:** product/organization name, one strong tagline, one hero visual. Minimal text.
- **Inside Flap:** a single compelling stat, quote, or "why it matters" hook -- this is the teaser
  someone reads right before opening the whole thing.
- **Inside Left / Center / Right:** the substance -- 3-6 key features or services, each with a
  short (1-2 sentence) description. Treat this like the feature-card slides in the pptx skill.
- **Back Panel:** contact info, website, QR code if useful, and a short closing CTA line.

## Step 5: Build it

Use the same toolkit as the `pptx` skill (pptxgenjs + react-icons, per that skill's setup), but
with a **custom page size** matching the flat sheet instead of the default slide size:

```javascript
const pres = new pptxgen();
pres.defineLayout({ name: 'TRIFOLD', width: 11, height: 8.5 }); // inches, flat sheet
pres.layout = 'TRIFOLD';

// Side A (Outside) — one slide, three panels drawn at x = 0, 3.667, 7.333
const sideA = pres.addSlide();
// Side B (Inside) — a second slide, same three x-positions, same left-to-right order
const sideB = pres.addSlide();
```

Read `/mnt/skills/public/pptx/SKILL.md` for the icon-generation setup, safe fonts, and card/badge
patterns (ring icons, filled icons, colored cards) -- reuse those directly, just laid out within
panel-width columns instead of full-slide columns. Read
`/mnt/skills/public/frontend-design/SKILL.md` before picking colors and type so the result doesn't
look templated.

**Print-specific rules on top of the usual design guidance:**
- Keep all text and important graphics **at least 0.25in away from every fold line and every trim
  edge** -- content right on a fold or edge gets visually lost or cut.
- Body text no smaller than ~9pt; panels are narrow, so short sentences beat long paragraphs.
- If this is going to a commercial print shop rather than a home/office printer, mention to the
  user that they may want the shop to convert the final PDF to CMYK and add bleed -- LibreOffice
  and PowerPoint both export RGB, which is fine for digital/office printing but not always for
  professional offset printing.

## Step 6: QA and deliver

Follow the same validate → render → visually inspect loop as the pptx skill:
1. `validate.py` on the .pptx for structural correctness.
2. Convert to PDF with `soffice.py --convert-to pdf`, then `pdftoppm` to images.
3. View each panel and specifically check: nothing sits on a fold line, the front cover reads
   correctly on its own, and the inside spread flows left to right without gaps.
4. Export **both** the editable `.pptx` and the print-ready `.pdf` to outputs -- the PDF is what
   actually gets printed or emailed; the PPTX is what the user edits later.
