# Prompt Template — Generate a New Vertical Landing Page

Use this with `template.html` + `design_style.md` attached (or pasted) in a
new chat. Fill in the bracketed fields, delete this instruction line, and
send. Works with Claude or any capable coding assistant.

---

## Copy-paste prompt

```
I'm reusing my "Blueprint" landing page design system for a new vertical.
Attached: template.html (the structural/CSS reference — DO NOT change any
CSS, layout, class names, animations, or script) and design_style.md (the
design system rules, per-industry starting points, and QA checklist).

Task: produce a complete, standalone HTML file for the vertical below by
replacing every {{PLACEHOLDER}} token in template.html with real content.
Do not restructure sections, do not add or remove sections, do not touch
the <style> block or the <script> block. Keep copy lengths within the
guardrails in design_style.md §7 so nothing wraps or overflows. When done,
self-check with the QA list in design_style.md §10 and confirm no
{{PLACEHOLDER}} tokens remain.

VERTICAL: [e.g. Security Agency]
BRAND NAME: [e.g. Sentinel Ops]
ONE-LINE POSITIONING: [what this product does, for this vertical, in plain words]
PRIMARY AUDIENCE / BUYER: [e.g. security agency owners managing 50-500 guards across client sites in Dhaka]
CORE VALUE PROP (for the hero): [the single biggest promise — e.g. "prove every guard was at their post, on time, every shift"]
KEY PROOF POINTS / STATS (4, for the stats band): [real or reasonable placeholder numbers]
THE 3-STEP PROCESS (for showcase 1 — arrival/verification/departure style flow specific to this vertical):
  1. [step]
  2. [step]
  3. [step]
LIVE-TRACKING ANGLE (for showcase 2 — what "real-time visibility" means for this buyer): [e.g. patrol route + checkpoint scans across a client site]
MOBILE APP FEATURES (5, for showcase 3): [bullet list]
REPORTING/ADMIN FEATURES (3, for showcase 4): [bullet list]
TESTIMONIAL VOICES (4, roles only — I'll write quotes, or write them yourself): [e.g. Security Agency Owner, Client Facility Manager, Ops Supervisor, Guard Team Lead]
CONTACT FORM SEGMENT OPTIONS (4, for the industry/segment dropdown): [e.g. Residential / Commercial / Event / Industrial Security]
COLOR: [keep default cobalt blue --brand #0047AB + green --accent #10B981 for suite consistency] OR [use this palette instead: ...]
IMAGES: [I'll supply hero/mobile/dashboard screenshots after — use descriptive alt text and placeholder filenames hero-app.png / mobile-app.png / dashboard.png for now]

Output the finished file as {{vertical-slug}}.html.
```

---

## Notes on filling it in

- **Keep the metaphor literal.** The design system's whole visual language
  (geofence circle, moving dot, check-in/check-out status box, live-data
  sparkline) assumes a *location + time verification* story. Every listed
  vertical (construction, education, electricians, events, healthcare,
  hotel/restaurant, manufacturing, security) fits this naturally because
  they're all shift/attendance/workforce-presence problems — lean into
  that rather than forcing an unrelated product into this shell.
- **Testimonials**: write 1–2 sentences per quote, specific to a concrete
  outcome (a number, a saved hour, a resolved dispute) rather than generic
  praise — see design_style.md §7 for length guardrails.
- **If a vertical genuinely needs an extra short-feature-grid section**
  instead of a 3-step narrative, say so in the prompt — `template.html`
  already carries unused `.bento-grid` / `.bento-item` CSS for exactly that
  (see design_style.md §3).
- **Batch generation**: to produce all 8 verticals in one sitting, run this
  prompt once per vertical rather than asking for all 8 in a single
  response — content quality (and adherence to length guardrails) holds up
  much better one page at a time.