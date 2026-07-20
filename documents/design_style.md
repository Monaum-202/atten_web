# Design Style Guide — "Blueprint" Landing Page System

Source: derived from the Teksoi (GPS attendance) landing page. This is the
design system to reuse, unchanged, across every new vertical — Construction,
Education, Electricians, Events & Entertainment, Healthcare, Hotel
Management/Restaurants, Manufacturing, Security Agency, and any future one.

**Rule of thumb:** the CSS never changes. Only the copy in `{{PLACEHOLDER}}`
tokens, the 3 screenshot images, and (optionally) the 2 color variables
change per vertical.

---

## 1. Concept

A "technical blueprint" aesthetic — cobalt ink on paper, drafting-grid
backgrounds, monospace data labels, animated SVG diagrams that visualize
geofencing/tracking. It reads as precise, verifiable, engineering-grade —
which is why it works for any B2B workforce/operations product: the visual
language sells "we can prove this happened," not "we look nice."

Keep that thesis intact for every vertical: the hero visual should always
feel like live instrumentation (a tracking dot, a status readout, a
live-data sparkline), not a lifestyle photo.

---

## 2. Design tokens

### Color
```css
--brand:        #0047AB   /* Cobalt Blueprint Blue — primary actions, links, accents */
--brand-light:  #E0E7FF   /* tag backgrounds, avatar fills */
--ink:          #0B1730   /* headings, dark cards (CTA, status boxes) */
--slate:        #4B5563   /* body copy */
--bg:           #F9FAFB   /* page background */
--white:        #FFFFFF
--accent:       #10B981   /* Precision Green — "success/live" states, secondary icon color */
--line:         rgba(0,71,171,0.1)  /* hairlines, grid, card borders */
```
`--brand` and `--accent` are the only two values that should ever change
between verticals (see §8). Keep `--ink/--slate/--bg/--white` fixed so every
page in the family still feels like the same product suite.

### Type
| Role | Face | Usage |
|---|---|---|
| Display | **Outfit** (600/800) | h1/h2, logo, stat numbers, nav CTA labels |
| Body | **Manrope** (400/500/700) | paragraphs, list items, form labels |
| Mono | **IBM Plex Mono** | eyebrow tags, SVG data labels, footer meta, form field labels |

Headline letter-spacing runs tight/negative (`-2px` to `-3px`) at large
sizes — this, plus the outlined accent word (`-webkit-text-stroke`), is the
signature headline treatment. Keep it on every hero.

### Shape & elevation
- Radius scale: 8px (buttons/inputs) → 12–16px (small cards) → 24px (bento/review cards) → 32–40px (hero visual frame, CTA card, viz cards).
- Shadows are always cool-toned and brand-tinted (`rgba(0,71,171,...)` / `rgba(11,23,48,...)`), never neutral gray — this is part of what makes it feel "engineered" rather than generic SaaS.
- 1px hairline borders (`var(--line)`) everywhere; no heavy strokes.

### Background texture
Every section sits on the fixed blueprint grid (`body::before`) plus soft
radial color washes (`body::after`, and per-section inline radial-gradients
that alternate corners left/right down the page). Don't remove the grid —
it's the single most identifying visual signature of this system.

---

## 3. Layout components

- **Nav** — sticky, blurred glass background, logo mark (bordered square
  with solid inner square), 4 text links, dual CTA (filled primary +
  outline secondary), hamburger under 900px.
- **Hero** — 50/50 grid: headline + lead + CTAs + 2 mini-stats on one side;
  a "device/dashboard in a frame" visual on the other, with a floating
  annotated SVG diagram (top-right) and a floating credibility badge
  (bottom-left). This 50/50 + floating-badge pattern repeats for every
  major section, alternating sides via `direction: rtl` on the wrapping
  grid (content order flips, text stays LTR internally).
- **Stats band** — full-bleed brand-color strip, 4 big numbers.
- **Alternating showcase sections** (×3 in this build: "automation",
  "tracking", "mobile") — each is a 50/50 hero-grid alternating left/right,
  pairing narrative copy (numbered steps or feature cards) with a bespoke
  animated SVG or device-frame screenshot.
- **Bento grid** — `.bento-item.large/medium/small/full` CSS exists for a
  classic feature-grid section (`.features`) if a vertical needs one; the
  Teksoi build didn't use it. Use it for a page that needs 3–5 short
  feature blurbs instead of a 3-step narrative.
- **Reports/analytics section** — dashboard screenshot + floating
  "live data feed" sparkline card + 3 icon-cards below the copy.
- **Reviews** — auto-scrolling, infinitely-looping horizontal carousel,
  4 cards, star rating + quote + avatar-initials + name/role, dot nav.
- **CTA band** — dark full-bleed card, eyebrow + headline + 2 buttons.
- **Contact** — 2-column: value props with a connecting vertical line vs. a
  card form (name/email/phone/company + 2 selects).
- **Footer** — logo/tagline, 2 link columns, copyright bar.

---

## 4. Motion

- **Scroll reveal**: `.reveal`, `.reveal-left`, `.reveal-right`,
  `.reveal-zoom`, `.reveal-fade` + `IntersectionObserver` toggling
  `.visible`. Apply to one wrapper per side of every alternating section.
- **SVG diagrams**: a dot/pin moves along a fixed path via
  `animateMotion`; a status box cross-fades between states (searching →
  matched, outside → within boundary) on a shared timeline
  (`keyTimes`). This "live instrument" motion is the signature element —
  reuse the pattern (grid + boundary shape + moving node + status box)
  rather than inventing a new diagram style per vertical.
- **Carousel**: CSS `scroll-snap` + JS auto-advance every 4s, pauses on
  hover/touch, infinite loop via cloned cards.

Respect the brief's existing restraint: one signature animated diagram per
section, nothing more layered on top.

---

## 5. Icons

[Iconify "Solar"](https://icon-sets.iconify.design/solar/) icon set,
loaded via the `<iconify-icon>` web component. Use the `-bold-duotone` or
`-bold` variants for feature/value-prop icons (richer, sits well at 28–40px)
and `-linear` for small inline UI icons (checkmarks, nav toggle, arrows).

---

## 6. Responsive rules

| Breakpoint | Behavior |
|---|---|
| 1024px | Hero grids collapse to 1 column, centered text, bento becomes 1 column |
| 900px | Nav links collapse into hamburger menu |
| 768px | Section padding shrinks, headline sizes step down, contact form goes 1 column |
| 480px | Stats grid becomes 2×2, hero visual shrinks further |

Don't change these breakpoints — the whole component set (nav, hero,
bento, contact form) is tuned against them together.

---

## 7. Placeholder glossary (`template.html`)

All tokens use the form `{{TOKEN_NAME}}`. Find-and-replace every one before
shipping — run `grep -o '{{[A-Z0-9_]*}}' template.html | sort -u` to confirm
none remain.

| Section | Tokens |
|---|---|
| Global / Nav | `BRAND_NAME`, `TAGLINE`, `NAV_LINK_1..4`, `CTA_PRIMARY_LABEL`, `CTA_SECONDARY_LABEL` |
| Hero | `HERO_HEADLINE_ACCENT`, `HERO_HEADLINE_REST`, `HERO_LEAD`, `HERO_MINISTAT_1/2_LABEL`/`DESC`, `HERO_SVG_ZONE_LABEL`, `HERO_SVG_BADGE_TITLE`, `HERO_SVG_BADGE_STATUS`, `HERO_SVG_BADGE_ID_LABEL`, `HERO_SVG_CHECKIN_LABEL`, `IMG_HERO_APP`, `IMG_HERO_APP_ALT`, `TECH_BADGE_LABEL`, `TECH_BADGE_VALUE` |
| Stats band | `STAT_1..4_VALUE`, `STAT_1..4_LABEL` |
| Showcase 1 (process) | `SEC1_HEADLINE`, `SEC1_LEAD`, `SEC1_STEP_1..3_TITLE`/`DESC`, `SEC1_SVG_ZONE_LABEL` |
| Showcase 2 (live tracking) | `SEC2_HEADLINE`, `SEC2_LEAD`, `SEC2_CARD_1/2_ICON`/`TITLE`/`DESC`, `SEC2_SVG_SITE_LABEL`, `SEC2_LOC1..3_NAME`/`TIME` |
| Showcase 3 (mobile) | `SEC3_TAG`, `SEC3_HEADLINE`, `SEC3_LEAD`, `SEC3_FEATURE_1..5_ICON`/`LABEL`, `IMG_MOBILE_APP`, `IMG_MOBILE_APP_ALT` |
| Showcase 4 (reports) | `SEC4_TAG`, `SEC4_HEADLINE`, `SEC4_LEAD`, `SEC4_CARD_1..3_ICON`/`TITLE`/`DESC`, `IMG_DASHBOARD`, `IMG_DASHBOARD_ALT` |
| Reviews | `REVIEWS_HEADLINE`, `REVIEWS_SUBHEAD`, `REVIEW_1..4_QUOTE`/`INITIALS`/`NAME`/`ROLE` |
| CTA band | `CTA_EYEBROW`, `CTA_HEADLINE`, `CTA_SUBTEXT`, `CTA_BUTTON_1_LABEL`, `CTA_BUTTON_2_LABEL` |
| Contact | `CONTACT_HEADLINE`, `CONTACT_LEAD`, `CONTACT_VALUE_1/2_ICON`/`TITLE`/`DESC`/`TAG`, `CONTACT_FIELD5_LABEL` (default: company-size select), `CONTACT_FIELD6_LABEL` + `CONTACT_INDUSTRY_OPTION_1..4` (industry/segment select), `CONTACT_SUBMIT_LABEL` |
| Footer | `FOOTER_TAGLINE`, `FOOTER_PRODUCT_LINK_1..3`, `FOOTER_RESOURCE_LINK_1..3`, `FOOTER_YEAR` |

Text-length guardrails so nothing wraps oddly:
- `HERO_LEAD`: 1–2 sentences (~140–180 characters)
- `SEC1/2/3/4 _LEAD`: 1–2 sentences (~120–160 characters)
- Step/feature `_DESC`: 1 short sentence (~90–130 characters)
- `REVIEW_*_QUOTE`: 1–2 sentences (~100–160 characters)

Untouched intentionally: a handful of SVG micro-labels (`SEARCHING...`,
`Check-In: 08:52`, `Check-Out: 17:10`, `ASSIGNED WORK AREA`,
`ATTENDANCE_STATUS`, `LIVE DATA FEED`, `WITHIN GEOFENCE`, `4 CHECK-INS · 0
EXITS TODAY`, `AUTO VERIFY ACTIVE`) are illustrative sample telemetry that
reads correctly for every vertical (a hospital, a hotel, and a construction
site all have check-ins, zones, and live status). Leave them as-is unless a
specific vertical needs different wording.

---

## 8. Per-industry starting points

Same layout and interaction pattern throughout; these are just seed ideas
for tone, the `--accent` swap (optional — keep `--brand` cobalt for suite
consistency, or shift it if each vertical is a distinct product line), the
industry-select field, and icon choices. Treat as a starting point, not a
rulebook — write real copy for the real product each time.

| Industry | Angle | Suggested icon set | `CONTACT_FIELD6` options idea |
|---|---|---|---|
| Construction | Site attendance, sub-contractor headcount, multi-site geofencing | `solar:buildings-3-bold-duotone`, `solar:helmet-bold`, `solar:ruler-cross-pen-bold` | Residential / Commercial / Infrastructure / Industrial |
| Education | Staff & faculty attendance, campus geofencing, substitute tracking | `solar:square-academic-cap-bold-duotone`, `solar:notebook-bold`, `solar:users-group-rounded-bold` | School / College / University / Coaching Center |
| Electricians | Job-site check-in/out, multi-client route verification, job timestamps | `solar:bolt-bold-duotone`, `solar:cable-bold`, `solar:widget-bold` | Residential / Commercial / Industrial / Emergency Callout |
| Events & Entertainment | Crew/vendor call-time verification, venue geofencing, shift wrap-up | `solar:ticket-bold-duotone`, `solar:confetti-bold`, `solar:calendar-mark-bold` | Corporate Events / Weddings / Concerts / Exhibitions |
| Healthcare | Shift attendance, ward/floor zones, credential-checked check-in | `solar:health-bold-duotone`, `solar:stethoscope-bold`, `solar:calendar-mark-bold` | Hospital / Clinic / Diagnostic Center / Home Care |
| Hotel Management / Restaurants | Floor/outlet attendance, housekeeping route tracking, shift handover | `solar:bed-bold-duotone`, `solar:chef-hat-bold-duotone`, `solar:cup-hot-bold` | Hotel / Resort / Restaurant / Cafe Chain |
| Manufacturing | Shop-floor attendance, shift/line zones, overtime accuracy | `solar:factory-bold-duotone`, `solar:conveyor-belt-bold`, `solar:settings-bold` | Garments / Electronics / Food Processing / Heavy Industry |
| Security Agency | Guard patrol/checkpoint verification, post geofencing, incident timestamping | `solar:shield-check-bold-duotone`, `solar:siren-bold`, `solar:widget-5-bold` | Residential / Commercial / Event / Industrial Security |

---

## 9. Assets checklist per new page

Replace 3 images (keep same aspect ratios: hero ≈ portrait app screen,
mobile-showcase ≈ tall phone screen, reports ≈ 16:10 dashboard):
1. `{{IMG_HERO_APP}}` — main app screen shown in the hero frame
2. `{{IMG_MOBILE_APP}}` — phone-framed screen in the mobile showcase
3. `{{IMG_DASHBOARD}}` — admin/web dashboard screen in the reports section

## 10. Pre-ship QA

- [ ] `grep -o '{{[A-Z0-9_]*}}' file.html` returns nothing
- [ ] All 3 images present and reasonably sized (compress before shipping)
- [ ] Nav anchors (`#automation`, `#reports`, `#contact`) still match section IDs
- [ ] Test at 1024px, 900px, 768px, 480px
- [ ] Contact form field 6 options match the vertical
- [ ] Review quotes/names read as plausible for the vertical (not copy-pasted attendance language into an unrelated field)