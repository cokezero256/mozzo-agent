---
name: miro-mind-map
description: >
  Generate beautiful, structured mind map boards in Miro from any content —
  scripts, frameworks, strategies, outlines, or documents. Produces clean
  vertical-flow layouts with color-coded section headers, typed nodes, dashed
  curved connectors, and proper spacing directly on a Miro board via the Miro
  MCP connector. Use this skill whenever the user says "make a mind map",
  "create a Miro board", "visualize this", "put this in Miro", "build a board",
  "generate the Miro", "map this out", "board this", or after completing a
  script review when the user wants to visualize the structure. Also trigger
  when the user pastes an outline or framework and asks to "see it visually"
  or "make it pretty". This skill handles ALL Miro board creation — even if the
  user just says "Miro" in the context of content they've been working on,
  check if this skill applies.
---

# Miro Mind Map Generator

You create structured, visually consistent mind map boards in Miro. You take any
structured content — a script, framework, strategy, outline, or document — parse
it into a node hierarchy, confirm with the user, then build it on a Miro board
using the Miro MCP tools.

The result looks like a professional presentation-quality mind map: title at top
center, sections branching horizontally into columns, each column reading
top-to-bottom with typed nodes (headers, sub-labels, bullet blocks) connected by
dashed curved arrows. Every board you produce follows the exact same visual
language so the user's boards are consistent over time.

---

## Prerequisites

This skill requires the **Miro MCP connector**. If it is not connected, tell the
user:

> "The Miro connector isn't connected yet. To set it up, go to your Connectors
> settings and connect the Miro integration. Once it's active, run this command
> again and I'll build the board."

If Miro tools are unavailable, do NOT attempt to build the board through any
other method. Stop and ask the user to connect Miro first.

---

## Step 1 — Get the Content

Ask: "What should I map? You can give me a script, outline, framework, or just
paste structured text."

Accept any of these input formats:

- **A completed longform script** → extract section headings, WHY/WHAT/HOW
  blocks, key examples, open loops, CTA
- **A short-form script** → extract hook, story/context, moral/value, CTA
- **A raw outline** with headers and bullets
- **A framework or strategy** with sections and sub-points
- **A document** with chapters/sections

If the content was just produced in this conversation (e.g., the user just
finished `/write-script` or `/review-script`), pull it from context — don't ask
them to paste it again.

---

## Step 2 — Parse into the Node Hierarchy

Map the content to this 6-type node system. Use your judgment to assign types
based on the content's structure:

| Node Type | What It Represents | When to Use |
|-----------|-------------------|-------------|
| **TITLE** | The main topic / video title | Always exactly one, top center |
| **SUBTITLE** | Supporting context line | Optional — date, client, video length, etc. |
| **SECTION HEADER** | Major top-level divisions | Hook, Body Section 1, Section 2, Outro, etc. |
| **COLUMN HEADER** | Category name within a section | WHY, WHAT, HOW, or named sub-topics |
| **SUB-LABEL** | Bold concept name | Key idea, framework name, story label |
| **BULLET BLOCK** | Detail points under a sub-label | Supporting facts, examples, proof points |

**Parsing rules:**

- The document title or video title → TITLE
- Any metadata line (date, client, length) → SUBTITLE
- H1/H2 headings or major script sections → SECTION HEADER
- H3 headings or named sub-topics within a section → COLUMN HEADER
- Bold terms, framework names, labeled concepts → SUB-LABEL (always ALL CAPS
  with colon: "CONCEPT NAME:")
- Bullet points, examples, details → BULLET BLOCK (use middle dot: "· item")
- If a section has multiple parallel topics, each becomes a COLUMN HEADER with
  its own stack of sub-labels and bullets below it

**Hierarchy depth:** Keep it to 3-4 levels max. If the content goes deeper,
flatten by combining lower levels into bullet blocks. Mind maps lose readability
past 4 levels.

---

## Step 3 — Confirm the Structure

Before building anything, present the parsed structure as a clean text outline:

```
TITLE: "How to Build a 7-Figure Agency"
SUBTITLE: "Client: Alex Hormozi | Target: 15 min | Date: Feb 2026"

├── ① HOOK (orange)
│   ├── BOLD CLAIM:
│   │   · "Most agencies fail within 18 months"
│   │   · Surprising stat setup
│   └── OPEN LOOP:
│       · "The one thing separating 6 from 7 figures..."
│
├── ② THE MINDSET SHIFT (yellow)
│   ├── WHY
│   │   ├── STAKES:
│   │   │   · 90% quit in year 1
│   │   │   · Owner vs operator trap
│   │   └── DREAM OUTCOME:
│   │       · Freedom, leverage, scalability
│   ├── WHAT
│   │   └── CORE CONCEPT:
│   │       · Identity shift from doer to builder
│   └── HOW
│       └── CLIENT STORY:
│           · "$0 to $40k/mo in 6 months"
│           · The exact moment it clicked
│
├── ③ THE OFFER STACK (mint)
│   └── ...
│
└── ④ OUTRO + CTA (purple)
    ├── TAKEAWAY:
    │   · "Start with one offer, one audience"
    └── CTA:
        · "Watch the pricing breakdown video next"
```

Show:
- Total sections found
- Total nodes at each level
- The color assigned to each section header

Ask: **"Does this look right? I'll build the Miro board exactly from this."**

Wait for confirmation. If the user wants changes, update the structure and
re-confirm before proceeding.

---

## Step 4 — Build the Miro Board

Use the Miro MCP `draft_diagram_new` tool to create the board. Provide a
detailed, structured description that follows the VISUAL SPECIFICATION below
exactly.

When calling `draft_diagram_new`, structure your request to include:

1. The board name: **"[Content Title] — Mind Map"**
2. The complete node hierarchy with positions, sizes, colors, and text
3. The connector definitions between nodes
4. The layout instructions

If `draft_diagram_new` accepts a structured format, use it. If it accepts
natural language, describe the board in precise detail following the spec. If it
accepts Mermaid or another diagram syntax, translate the hierarchy into that
format while preserving as many visual properties as possible.

**Important:** The Miro MCP tools may have limitations on styling control. Work
within the tool's capabilities but always aim for the closest possible match to
the visual specification. If certain properties (like exact corner radius or font
weight) can't be set through the tool, note this to the user and suggest they
fine-tune those details in Miro's editor.

---

## VISUAL SPECIFICATION

This is the canonical reference for every Miro board you produce. Follow it
exactly.

### Overall Layout

- **Flow: VERTICAL** — Title at top center, content flows downward
- At the section level, spread **horizontally** into columns (tree branching)
- Each column is self-contained and reads top-to-bottom
- NEVER use left-to-right flowchart style
- Minimum **300px horizontal gap** between columns
- Minimum **80px vertical gap** between elements

### Node Type 1: TITLE

| Property | Value |
|----------|-------|
| Widget type | Text (no shape/card) |
| Font size | 36px |
| Font weight | Bold |
| Color | #1A1A2E |
| Alignment | Center |
| Background | None |
| Border | None |
| Position | Top center of the board, (0, 0) |

### Node Type 2: SUBTITLE

| Property | Value |
|----------|-------|
| Widget type | Text (no shape/card) |
| Font size | 20px |
| Font weight | Regular |
| Color | #636E72 |
| Alignment | Center |
| Background | None |
| Border | None |
| Position | Directly below title, 40px gap |

### Node Type 3: SECTION HEADER

| Property | Value |
|----------|-------|
| Widget type | Shape — rectangle |
| Fill | Cycle: #FF6B35, #FDCB6E, #3DDBA9, #6C5CE7 (repeat) |
| Border | None |
| Font size | 16px |
| Font weight | Bold |
| Text color | #FFFFFF |
| Width | Auto-fit to text + 40px padding each side |
| Height | 48px |
| Corner radius | 4px |

**Color rotation:** First section gets orange (#FF6B35), second gets yellow-amber
(#FDCB6E), third gets mint (#3DDBA9), fourth gets purple (#6C5CE7). If there are
more than 4 sections, cycle back to orange.

### Node Type 4: COLUMN HEADER

| Property | Value |
|----------|-------|
| Widget type | Text (no shape/card) |
| Font size | 18px |
| Font weight | Bold |
| Text decoration | Underline |
| Color | #1A1A2E |
| Background | None |
| Border | None |

### Node Type 5: SUB-LABEL

| Property | Value |
|----------|-------|
| Widget type | Text (no shape/card) |
| Font size | 14px |
| Font weight | Bold |
| Color | #1A1A2E |
| Format | ALL CAPS, always ends with colon — "LABEL NAME:" |
| Background | None |
| Border | None |

### Node Type 6: BULLET CONTENT BLOCK

| Property | Value |
|----------|-------|
| Widget type | Text (no shape/card) |
| Font size | 13px |
| Font weight | Regular |
| Color | #2D3436 |
| Bullet style | Middle dot + space: "· " before each item |
| Line breaks | Each bullet on its own line |
| Background | None |
| Border | None |
| Indent | 20px right of its parent sub-label |

### Connectors

| Property | Value |
|----------|-------|
| Style | Dashed line |
| Color | #636E72 (medium gray) |
| End cap | Arrow pointing DOWN toward child |
| Curvature | Curved/arc (smooth arc, not straight, not elbow) |

**Connect these pairs:**
- Title → each Section Header
- Section Header → each Column Header underneath it

**Do NOT connect:**
- Sub-labels to anything (they float below their parent with spacing only)
- Bullet blocks to anything (they float below their parent sub-label)

### Spacing Rules

| Gap | Size |
|-----|------|
| Title → Subtitle | 40px |
| Subtitle → first Section Header row | 120px |
| Between Section Header columns | 300px minimum |
| Section Header → Column Header | 80px |
| Column Header → first Sub-label | 30px |
| Sub-label → Bullet block | 10px |
| Between Sub-label groups | 40px |
| Between Column Header stacks | 80px |

### Construction Sequence

Follow this order when building the board:

1. **Title** at position (0, 0), centered
2. **Subtitle** at (0, 60), centered
3. **Calculate column positions** — count top-level sections, space them evenly
   across the X axis with 300px gaps, centered under the title
4. **Section Headers** — place each at (columnX, 200)
5. **Connectors** — dashed curved arrows from title to each section header
6. **Column content** — for each section, stack vertically below the header:
   - Column Header (if the section has sub-divisions)
   - Sub-labels with their bullet blocks
   - Respect all spacing rules
7. **Sub-section connectors** — if a section has multiple columns, connect the
   section header to each column header with dashed curved arrows
8. **Board name** — set to "[Content Title] — Mind Map"
9. **Zoom to fit** all content

### When the Content Is a Script

For longform YouTube scripts specifically, use this mapping:

| Script Element | Node Type |
|----------------|-----------|
| Video title | TITLE |
| Client / length / date | SUBTITLE |
| HOOK section | SECTION HEADER (always first, orange) |
| Each body section heading | SECTION HEADER |
| OUTRO + CTA | SECTION HEADER (always last) |
| WHY / WHAT / HOW | COLUMN HEADER |
| Named concepts, frameworks, story labels | SUB-LABEL |
| Examples, stats, bullet points, proof | BULLET BLOCK |
| Open loops | SUB-LABEL with "OPEN LOOP:" prefix |
| Production notes [B-ROLL], [TEXT], etc. | Omit from mind map |

---

## Step 5 — Deliver

After the board is created, share the Miro board link and tell the user:

> "Your mind map is live on Miro: [link]
>
> It has [X] sections across [Y] total nodes. You can drag things around, add
> sticky notes, or adjust colors directly in Miro."

If the user wants changes:
- Update the node hierarchy
- Re-generate the affected sections
- Add or remove levels of detail

---

## Tips for Different Content Sizes

**Small content (3-4 sections, <20 nodes):**
- Include all levels of detail down to bullet blocks
- Add generous spacing — let it breathe

**Medium content (5-6 sections, 20-40 nodes):**
- Include section headers, column headers, and key sub-labels
- Be selective with bullet blocks — only the most important details

**Large content (7+ sections, 40+ nodes):**
- Keep to section headers and column headers only
- Collapse details into summarized sub-labels
- Consider splitting into 2 boards if the content has a natural midpoint
- Tell the user: "This is a big one — I've kept it to the top 2 levels for
  readability. Want me to drill into any specific section?"
