---
name: brand-colors
description: >
  YouTubeViews / Mozzo Media brand design system with dark and light mode color tokens,
  typography, and component styling. Use when the user mentions "Ytubeviews Brand Colors",
  "brand colors", or requests dark/light mode styling for any YouTubeViews project.
  Applies to dashboards, web apps, landing pages, presentations, documents, and any
  UI or design work for YouTubeViews. Triggers on phrases like "Ytubeviews Brand Colors - Light Mode",
  "Ytubeviews Brand Colors - Dark Mode", "use my brand colors", or "apply brand styling".
---

# YouTubeViews Brand Design System

Apply these tokens whenever building UI, components, pages, or documents for YouTubeViews / Mozzo Media.
Default to **dark mode** unless the user specifies light mode.

## Color Tokens

### Dark Mode (Default)

| Token | Hex | Usage |
|---|---|---|
| `--bg-primary` | `#0A0A0B` | Page background |
| `--bg-secondary` | `#111113` | Cards, panels, sidebars |
| `--bg-tertiary` | `#1A1A1E` | Inputs, wells, recessed areas |
| `--bg-elevated` | `#222228` | Dropdowns, modals, popovers |
| `--text-primary` | `#F5F5F7` | Headlines, body text |
| `--text-secondary` | `#A1A1AA` | Descriptions, subtext |
| `--text-tertiary` | `#71717A` | Captions, placeholders, disabled |
| `--accent` | `#3DDBA9` | CTAs, active states, links |
| `--accent-hover` | `#5AE6BD` | Hover states |
| `--accent-muted` | `rgba(61,219,169,0.12)` | Badge fills, subtle highlights |
| `--border-default` | `rgba(255,255,255,0.08)` | Card borders, dividers |
| `--border-subtle` | `rgba(255,255,255,0.04)` | Inner separators |
| `--border-strong` | `rgba(255,255,255,0.15)` | Outlined buttons, active borders |
| `--card-bg` | `rgba(255,255,255,0.03)` | Glass-style card fill |
| `--card-border` | `rgba(255,255,255,0.06)` | Card stroke |

### Light Mode

| Token | Hex | Usage |
|---|---|---|
| `--bg-primary` | `#FAFAFA` | Page background |
| `--bg-secondary` | `#FFFFFF` | Cards, panels |
| `--bg-tertiary` | `#F4F4F5` | Inputs, wells |
| `--bg-elevated` | `#FFFFFF` | Dropdowns, modals |
| `--text-primary` | `#09090B` | Headlines, body |
| `--text-secondary` | `#52525B` | Descriptions |
| `--text-tertiary` | `#A1A1AA` | Captions, placeholders |
| `--accent` | `#0D9B6A` | CTAs (deeper mint for contrast) |
| `--accent-hover` | `#0B8A5E` | Hover states |
| `--accent-muted` | `rgba(13,155,106,0.08)` | Badge fills |
| `--border-default` | `rgba(0,0,0,0.08)` | Card borders |
| `--border-subtle` | `rgba(0,0,0,0.04)` | Inner separators |
| `--border-strong` | `rgba(0,0,0,0.15)` | Outlined buttons |
| `--card-bg` | `#FFFFFF` | Card fill |
| `--card-border` | `rgba(0,0,0,0.06)` | Card stroke |

### Semantic Colors (Both Modes)

| Token | Hex | Usage |
|---|---|---|
| `--success` | `#3DDBA9` | Positive states, growth |
| `--warning` | `#F5A623` | Attention, pending |
| `--error` | `#EF4444` | Destructive, overdue |
| `--info` | `#60A5FA` | Informational |

## Typography

- **Primary font**: `'Plus Jakarta Sans', -apple-system, sans-serif` — all headings and body
- **Mono font**: `'JetBrains Mono', monospace` — data, numbers, code, timestamps
- **H1**: 48px, weight 800, letter-spacing -0.04em, line-height 1.1
- **H2**: 32px, weight 700, letter-spacing -0.03em, line-height 1.2
- **H3**: 20px, weight 600, letter-spacing -0.01em
- **Body**: 15px, weight 400, line-height 1.7
- **Caption**: 12px, weight 500, letter-spacing 0.02em, uppercase
- **Mono data**: 13px, accent color

## Border Radius

| Token | Value | Usage |
|---|---|---|
| `--radius-sm` | `8px` | Small elements, checkboxes |
| `--radius-md` | `12px` | Inputs, small cards |
| `--radius-lg` | `16px` | Cards, panels |
| `--radius-xl` | `20px` | Large containers, modals |
| `--radius-full` | `9999px` | Buttons, badges, pills |

## Component Patterns

- **Buttons**: Pill-shaped (`border-radius: 9999px`), padding `10px 24px`, weight 600
  - Primary: accent bg, dark text (`#0A0A0B`)
  - Secondary: transparent, border `--border-strong`, primary text
  - Ghost: `--accent-muted` bg, accent text
- **Badges**: Pill-shaped, padding `4px 12px`, 12px weight 600
- **Cards**: `--radius-lg` or `--radius-xl`, `--card-bg` + `--card-border`, hover lift with shadow
- **Inputs**: `--radius-md`, `--bg-primary` fill (in dark), `--border-default` stroke, accent border on focus

## Design Principles

- Ultra-dark backgrounds with layered surface elevation for depth
- Mint accent is the signature — use sparingly for maximum impact on CTAs, active states, and key data
- Generous whitespace, clean alignment, no visual clutter
- Rounded corners everywhere — never use sharp corners
- Subtle borders using low-opacity white (dark) or black (light) rather than solid gray lines
- Monospace font for anything numerical or data-driven
