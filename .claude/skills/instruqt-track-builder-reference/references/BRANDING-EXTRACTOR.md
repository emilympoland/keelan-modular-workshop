# BRANDING-EXTRACTOR — Customer Branding Extraction (companion procedure)

Authoritative procedure for the skill's customer branding extraction (Discovery,
opt-in, user-approved). The skill reads this file in full at the moment the feature
is approved — never from a cached summary.

Analyze the public-facing website for the company below and create a reusable
branding package that can be incorporated into an Instruqt track.

## Customer Information

**Company:** `<customer-name>`  
**Website:** `<public-url>`

> **If the fields above are unfilled placeholders:** derive the customer name and website from the context of the track being worked on (track.yml, MEMORY.md, README). If the customer cannot be determined unambiguously, ask the user before doing any research — a wrong-customer run is the most expensive failure possible.

---

# Objective

I am creating a custom Instruqt track for this prospect/customer.

The goal is **NOT** to clone their website.

The goal **IS** to capture the visual identity, styling patterns, colors, typography, and design language that can be safely incorporated into an Instruqt learning experience to create a more polished, customer-specific experience.

Use only publicly available information from the website.

Favor accuracy over assumptions.

Clearly identify any information that is inferred rather than directly observed.

---

# Research Workflow (do this first — fast path)

Work in this order. It is both the accuracy path and the speed path.

1. **Look for an official brand/press page first.** Check `<site>/brand`, `/brand-guidelines`, `/press`, `/media-kit`, and footer links labeled "Brand", "Press Kit", or "Media". Many companies publish exact palettes, typefaces, logo rules, and voice guidance.
2. **If an official brand page exists, treat it as the primary source.** Limit live-site inspection to ONE batched confirmation pass — collect button styling, border radii, spacing, label/eyebrow treatment, and font stacks in a single scripted probe. Do not crawl additional marketing pages.
3. **If no official brand page exists**, fall back to full live-site extraction: homepage plus one or two key product pages, computed styles for colors/type/components. Label semantic mappings as inferred.
4. **JS-rendered sites:** if a raw fetch returns a page shell, loading text, or obviously incomplete content, switch to rendered-browser extraction immediately — do not retry raw fetches.
5. **Fail fast on timeouts:** if a fetch hangs or times out once, go straight to rendered-browser extraction rather than re-waiting on long timeouts.

---

# Analyze

Review the website and identify the following.

## Brand Colors

Extract:

- Primary colors
- Secondary colors
- Accent colors
- Background colors
- Border colors
- CTA / button colors

Include:

- HEX values
- RGB values (if available)
- Recommended usage notes

---

## Typography

Identify:

- Primary font family
- Secondary font family
- Heading styles
- Body text styles
- Button text styles

If exact fonts cannot be determined, provide the closest likely match and clearly note that it is inferred.

**Licensed font policy:** when brand typefaces are licensed/proprietary, first check whether the vendor documents its own approved free alternatives (brand pages often do — e.g., a "Google alternatives" section). Prefer those over guessed lookalikes. The generated CSS must use web-loadable fonts (e.g., Google Fonts) with system-font fallbacks — Instruqt sandboxes and offline use cannot load licensed faces.

---

## Design Language

Document:

- Overall visual style
- Button styling patterns
- Card/container styling
- Border radius usage
- Shadow usage
- Spacing patterns
- Iconography style
- Common UI motifs

Describe the overall design personality (e.g., enterprise, modern SaaS, developer-focused, minimalist, security-focused, data-centric, etc.).

**Dark/light mode:** determine whether the brand runs a dual-mode system. If it does, capture both palettes, note any audience rule the brand publishes (e.g., dark for developers, light for enterprise), and ship a dark variant in the CSS.

---

## Logo Usage

Capture the brand's logo rules — these matter most for Instruqt notes slides and demo apps:

- Approved logo colors (many brands restrict to black/white only)
- Primary vs shorthand/icon-only variants and when each applies
- Clear-space and minimum-size rules
- Published misuse examples (no stretching, recoloring, gradients, busy backgrounds, etc.)
- URLs of official downloadable logo assets, if publicly provided

Do not download or redistribute logo files in the kit — record the URLs and rules only.

---

## Layout Patterns

Identify common patterns such as:

- Hero sections
- Callout boxes
- Alert banners
- Feature cards
- Navigation styling
- Section headers
- Promotional blocks
- Content containers

Focus on reusable elements that could reasonably appear inside an Instruqt challenge, demo application, or workshop guide.

---

## Brand Voice (Optional but Recommended)

Review website copy and marketing language.

Summarize:

- Tone
- Writing style
- Common messaging themes
- Preferred terminology
- Technical vs business-focused positioning

Provide guidance on how challenge instructions and demo content could align with the company's existing voice and messaging.

---

# Create File 1

## `<customer-name>-styles.css`

Generate a clean CSS file that:

- Captures the customer's visual identity
- Uses CSS variables where appropriate
- Includes comments throughout
- Is organized into logical sections
- Is easy to maintain and customize later

Required sections:

- :root
- Typography
- Buttons
- Cards
- Alerts
- Tables
- Code Blocks
- Challenge Notes
- Callout Panels
- Utility Classes

The CSS should be production-quality and suitable for:
- Instruqt Challenge Notes
- Custom HTML pages
- Demo applications
- Landing pages
- Workshop content

Do NOT attempt to recreate the entire website.

Do NOT include unnecessary page layout code.

Focus on reusable branding elements.

---

# Create File 2

## `<customer-name>-guide.html`

Generate a self-contained HTML reference guide demonstrating the CSS.

Include examples for:

- Page title
- Section headers
- Paragraphs
- Hyperlinks
- Buttons
- Success messages
- Warning messages
- Information messages
- Feature cards
- Tables
- Code snippets
- Challenge notes
- Callout panels
- Multi-column content blocks
- Recommended styling examples

The guide should function as a visual style reference that can be opened locally in a browser.

**CSS linkage:** link the companion stylesheet relatively (`<link rel="stylesheet" href="<customer-name>-styles.css">`) rather than embedding a copy — the CSS file is the single source of truth and the two files always live together in the kit folder. Guide-only scaffolding styles (swatch grids, annotations) may be inline.

Include brief annotations describing the intended purpose of each component.

---

# Create File 3

## `<customer-name>-branding-summary.md`

Create a concise branding reference document.

### Sources & Retrieval Date

Open the document with the exact source URLs used (official brand page, live site pages inspected) and the retrieval date. Brand sites change — a kit must show how stale it is.

### Executive Summary

Provide a 2–3 paragraph summary describing:

- The company's visual identity
- Design approach
- Overall brand personality

### Color Palette

Provide a table containing:

| Color | HEX | Purpose |
|---------|---------|---------|

Include recommendations for:

- Primary usage
- Secondary usage
- Alerts
- Accents
- Backgrounds

### Typography

Document:

- Primary fonts
- Secondary fonts
- Heading recommendations
- Body text recommendations

### Design Language

Summarize:

- Button styles
- Card styles
- Border treatments
- Shadows
- Spacing
- Visual hierarchy

### Brand Voice

Summarize:

- Tone
- Messaging style
- Key themes
- Technical depth
- Customer-facing language patterns

### Recommended Instruqt Usage

Provide practical recommendations for:

- Challenge Notes
- Instructions
- Alerts
- Callouts
- Demo Applications
- Landing Pages
- Workshop Content
- Executive Demonstrations
- Technical Workshops

### Design Considerations

Describe:

- What should be emphasized
- What should be avoided
- Any branding elements that may not translate well to an Instruqt environment

---

# Output Requirements

- Be consistent and deterministic.
- Favor accuracy over assumptions.
- Clearly label anything inferred.
- Do not invent branding elements that cannot be observed.
- Do not copy large portions of copyrighted website code.
- Create clean, reusable assets suitable for:
  - Instruqt tracks
  - Technical workshops
  - Customer demos
  - POCs
  - POVs
  - Enablement experiences

---

# Verification (required final step)

After generating all three files, run a programmatic consistency check (a short script, not a manual read-through):

1. Every brand HEX value appears in all three files; no stray non-brand HEX values anywhere.
2. Every CSS class used in the HTML guide is defined in the stylesheet.
3. No NUL bytes in any file (Windows-filesystem artifact that breaks downstream tooling).

Fix any mismatch before delivering. This check takes seconds and replaces manual review.

---

# Deliverables

Generate exactly the following files:

1. `<customer-name>-styles.css`
2. `<customer-name>-guide.html`
3. `<customer-name>-branding-summary.md`

Ensure all three files are internally consistent and reflect the same branding analysis.

---

# Output Location

Write all three files to a `branding/` folder at the root of the track being worked on:

```
<track-folder>/branding/
```

Rules:

- Do NOT place deliverables in the track's `assets/` folder — `assets/` is published with the track on `instruqt track push`, and the guide/summary are authoring references that should never ship to learners.
- The kit lives with its track, alongside `MEMORY.md` and `README.md` — each track build is customer-specific, so the kit travels with the track it was made for.
- When branding is actually applied to the track, copy ONLY `<customer-name>-styles.css` into `assets/` (so it gets CDN-hosted on push), and only once something in the track references it. The `branding/` copy remains the canonical source.
- If building another track for the same customer later, copy the `branding/` folder from the earlier track and refresh it incrementally against the sources (check the kit's Sources & Retrieval Date header) rather than re-running the full extraction. Reuse existing assets whenever practical; recreate only what is stale or missing.

---

# Logging

Log to the track's `MEMORY.md` when the extraction completes:

- The sources used and the retrieval date.
- A design-decision entry recording that the kit exists and where branding will be
  applied (or that application is still to be decided).
- A session-history line noting the extraction ran (or was refreshed from an
  existing kit).
