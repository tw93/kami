# kami · Cheatsheet

One-page quick reference. Scan before filling a template or tweaking a detail. Full spec in `references/design.en.md`.

## Eight invariants

1. Page background `#f5f4ed` (parchment), never pure white
2. Single accent: ink-blue `#1B365D`
3. All grays **warm-toned** (yellow-brown undertone), no cool blue-gray
4. Serif for headlines, sans for body / UI
5. Serif weight locked at 500, no bold
6. Line-height: headlines 1.1-1.3 / dense 1.4-1.45 / reading 1.5-1.55. **Never 1.6+**
7. Tag backgrounds solid hex, no rgba (WeasyPrint double-rectangle bug)
8. Depth via ring / whisper shadow, no hard drop shadows

## Color

| Role | Hex | Use |
|---|---|---|
| Parchment | `#f5f4ed` | Page background |
| Ivory | `#faf9f5` | Card / lifted container |
| Warm Sand | `#e8e6dc` | Button / interactive surface |
| Dark Surface | `#30302e` | Dark container |
| Deep Dark | `#141413` | Dark page background |
| **Brand** | **`#1B365D`** | **Accent · CTA · title underline (≤ 5% of surface)** |
| Brand Coral | `#2D5A8A` | Links on dark surfaces |
| Near Black | `#141413` | Primary text |
| Dark Warm | `#3d3d3a` | Secondary dark / link |
| Charcoal | `#4d4c48` | Button text / dense body |
| Olive | `#5e5d59` | Subtext · descriptions |
| Stone | `#87867f` | Tertiary · metadata |
| Warm Silver | `#b0aea5` | Light text on dark surfaces |
| Border Cream | `#f0eee6` | Default card border |
| Border Warm | `#e8e6dc` | Section divider |
| Ring Warm | `#d1cfc5` | Button hover / focus ring |

**rgba -> solid** (parchment base + ink-blue):

| Alpha | Solid |
|---|---|
| 0.08 | `#EEF2F7` |
| 0.14 | `#E4ECF5` |
| **0.18** | **`#E4ECF5`** ← default tag |
| 0.22 | `#ead3c7` |
| 0.30 | `#D6E1EE` |

## Type (print pt)

| Role | Size | Weight | Line-height |
|---|---|---|---|
| Display | 36-48 | 500 | 1.10 |
| H1 | 18-22 | 500 | 1.20 |
| H2 | 14-16 | 500 | 1.25 |
| H3 | 12-13 | 500 | 1.30 |
| Body Lead | 11 | 400 | 1.55 |
| Body | 9.5-10 | 400 | 1.55 |
| Body Dense | 9-9.2 | 400 | 1.42 |
| Caption | 8.5-9 | 400 | 1.45 |
| Label | 7.5-8 | 600 | 1.35 |
| Tiny | 7 | 400 | 1.40 |

Screen (px) ≈ pt × 1.33.

## Font stack (English)

```css
--serif: "Newsreader", "Source Serif 4", "Charter",
         Georgia, "Times New Roman", serif;
--sans:  "Inter", -apple-system, BlinkMacSystemFont,
         "Helvetica Neue", Arial, sans-serif;
--mono:  "JetBrains Mono", "SF Mono", "Fira Code",
         Consolas, Monaco, monospace;
```

## Spacing (4pt base)

| Tier | Value | Use |
|---|---|---|
| xs | 2-3pt | Inline |
| sm | 4-5pt | Tag padding |
| md | 8-10pt | Component interior |
| lg | 16-20pt | Between components |
| xl | 24-32pt | Section-title margin |
| 2xl | 40-60pt | Between major sections |
| 3xl | 80-120pt | Between chapters |

**Page margins (A4)**

| Document | T · R · B · L |
|---|---|
| Resume | 11 · 13 · 11 · 13 mm |
| One-Pager | 15 · 18 · 15 · 18 mm |
| Long Doc | 20 · 22 · 22 · 22 mm |
| Letter | 25 mm all sides |
| Portfolio | 12 · 15 · 12 · 15 mm |

## Radius scale

`4pt -> 6pt -> 8pt (default) -> 12pt -> 16pt -> 24pt -> 32pt (hero)`

## Common CSS snippets

### Card

```css
.card {
  background: var(--ivory);
  border: 0.5pt solid var(--border-cream);
  border-radius: 8pt;
  padding: 16pt 20pt;
}
```

### Tag (default lightest solid)

```css
.tag {
  background: #EEF2F7;            /* 0.08 equivalent */
  color: var(--brand);
  font-size: 8pt; font-weight: 600;
  padding: 1pt 5pt;
  border-radius: 2pt;
  letter-spacing: 0.4pt;
  text-transform: uppercase;
}
```

### Section title (brand underline is the signature move)

```css
.section-title {
  font-family: var(--serif);
  font-size: 14pt; font-weight: 500;
  color: var(--near-black);
  margin: 24pt 0 10pt 0;
}
```

### Metric (data card)

```css
.metric { display: flex; align-items: baseline; gap: 6pt; }
.metric-value {
  font-family: var(--serif); font-size: 16pt; font-weight: 500;
  color: var(--brand);
  font-variant-numeric: tabular-nums;
}
.metric-label { font-size: 9pt; color: var(--olive); }
```

### Quote

```css
.quote {
  border-left: 2pt solid var(--brand);
  padding: 4pt 0 4pt 14pt;
  color: var(--olive);
  line-height: 1.55;
}
```

## Quick decisions

| Need | Use |
|---|---|
| Headline | serif 500, line-height 1.10-1.30 |
| Reading body | sans 400, 9.5-10pt, 1.55 |
| Emphasize a number | `color: var(--brand)`, no bold |
| Divide two sections | 1pt brand border-bottom, or 0.5pt warm dotted |
| Quote | 2pt brand left border + olive color |
| Code | ivory bg + 0.5pt border + 6pt radius + mono |
| Primary button | brand fill + ivory text |
| Secondary button | warm-sand + charcoal |
| Chapter start | serif heading + 1pt brand line below |
| Cover | Display heading + right-aligned author/date + heavy whitespace |

Not on the table -> first principles: **serif carries authority, sans carries utility, warm gray carries rhythm, ink-blue carries focus**.
