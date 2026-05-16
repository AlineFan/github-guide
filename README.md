# /github-guide

A Claude Code skill that explains any GitHub repository — in **two modes** depending on the reader.

<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-skill-teal?style=flat-square" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/Mode_A-HTML_beginner_guide-orange?style=flat-square" alt="Mode A">
  <img src="https://img.shields.io/badge/Mode_B-Markdown_deep--dive-blueviolet?style=flat-square" alt="Mode B">
  <img src="https://img.shields.io/badge/language-中文_/_EN-blue?style=flat-square" alt="Language">
</p>

## Two Modes

### Mode A — HTML Beginner Guide (default)

For non-technical readers or developers evaluating whether to adopt a tool. Output: a single self-contained HTML file with theme, SVG charts, quizzes, animations.

```
/github-guide https://github.com/user/repo
```

| Module | Content |
|--------|---------|
| **Hero** | Name, stats with count-up animation, **SVG sparkline** showing GitHub stars trend |
| **Why It Exists** | Problem explained in everyday language + chat animation |
| **How It Works** | Step-by-step flow animation + data journey diagram |
| **Architecture** | **Radial SVG mind map** with hover-highlighting spokes (6 branches) |
| **Comparison** | **5-dim SVG radar chart** + table + **clickable SVG decision tree** with path-highlighting |
| **Before → After** | User experience before vs after + "Who needs this?" |
| **Real Use Cases** | Verified cases from X/Twitter, GitHub, blogs with source links |
| **Quick Start** | Copy-paste setup commands |

Every section includes interactive elements — quizzes, animated conversations, flow diagrams. Guides can be **deployed to a live URL via Vercel** or **exported to PDF** with the included scripts.

### Mode B — Markdown Technical Deep-Dive (new)

For engineers studying a project's architecture / tech stack to learn from it. Output: a single Markdown file with Obsidian frontmatter, callouts, Mermaid diagrams. Saves directly into an Obsidian vault or any specified path.

```
/github-guide https://github.com/user/repo
分析这个项目，存到 obsidian://open?vault=...&file=...
```

Triggered when the user provides an `obsidian://` URL, asks for "技术路径分析 / 源码分析 / 深度分析 / save as Markdown", or specifies a `.md` output path. Structure: TL;DR → quick facts → problem mapping → **core technical decision** → code map → **2-4 commands traced end-to-end** → safety design → build & dist → dependency analysis → **lessons to borrow** → references → one-line takeaway.

Mode B skips quizzes, chat animations, and use-case storytelling (audience already knows what the project does). It uses jargon freely (the reader is an engineer). It digs into the source code, names file paths, and has opinions about which design choices are smart vs. debatable. Full spec in [`references/markdown-deep-dive.md`](references/markdown-deep-dive.md).

## Examples

**Mode A (HTML)**:

| Repo | Guide |
|------|-------|
| [garrytan/gbrain](https://github.com/garrytan/gbrain) | [gbrain-guide-v2.html](examples/gbrain-guide-v2.html) |
| [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) | [karpathy-skills-guide.html](examples/karpathy-skills-guide.html) |

> Download the HTML file and open it in your browser — everything is self-contained.

**Mode B (Markdown deep-dive)**: produced live into the user's Obsidian vault, not bundled here. The canonical structure is documented in [`references/markdown-deep-dive.md`](references/markdown-deep-dive.md), with a worked example analyzing [tw93/Mole](https://github.com/tw93/Mole) (Mac cleanup tool — Shell+Go hybrid architecture, 614 lines, 11 sections covering core technical decision, code map, command-level path tracing, safety design, dependency analysis, and reusable patterns).

## Install

### Option 1: Claude Code Plugin (recommended)

```bash
claude /install-skill https://github.com/AlineFan/github-guide
```

### Option 2: Manual

Clone this repo into your Claude Code skills directory:

```bash
git clone https://github.com/AlineFan/github-guide.git ~/.claude/skills/github-guide
```

## Usage

### Default — Mode A (HTML for beginners)

```
/github-guide https://github.com/user/repo
```

Or describe what you want:

```
/github-guide help me understand this repo: https://github.com/anthropics/claude-code
```

The skill will: **research** (README + structure + GitHub stats) → **find use cases** (X/Twitter/Reddit/HN/blogs with verified engagement) → **find alternatives** for the comparison module → **generate** a single HTML file.

### Mode B (Markdown for engineers)

Include an Obsidian URL, a `.md` path, or a phrase like "技术路径分析 / 深度分析 / save as Markdown":

```
/github-guide https://github.com/user/repo
分析这个项目，存到 obsidian://open?vault=Notes&file=tech%2Frepo-analysis
```

The skill will: **clone the repo** (`gh repo clone <repo> /tmp/...`) → **read the entry point + build config + key source files** → **trace 2-4 commands end-to-end** → **catalog dependencies with why-chosen reasoning** → **write the Markdown file** to your Obsidian vault path (or wherever you specified). No use-case search, no theme picking — focus on the code.

## Design Principles

### Professional tone

Clear, professional Chinese. No colloquial or joking expressions. All interactive element names translated to Chinese descriptions.

### Beginner-first

Every technical term gets a plain-language explanation. Architecture diagrams have "what this means for you" subtitles. The target reader saw the repo trending and wants to understand it — not read the source code.

### Verified use cases only

Every use case shown in the guide must:
- Actually mention the specific repo (not a similar project)
- Link to the original source with a clickable ↗ link
- Show engagement metrics (views, likes) for social media posts
- Be honest when feedback is limited ("this project is 3 days old")

### Interactive > text

Concepts are taught through chat animations, flow diagrams, data journeys, mind maps, decision trees, and quizzes — not paragraphs.

### Honest comparisons with direct competitors

The comparison section only includes projects that solve **the same problem directly** — standalone open-source tools a user would choose between. Does not include built-in features of other products or DIY workaround combinations. Links to competitor repos, shows both strengths and weaknesses, includes a decision flow chart.

## Design System

The skill ships **4 visual themes** — the generator picks one based on what the repo *is*, not personal preference:

| Theme | When to use | Fonts | Accent |
|---|---|---|---|
| **Warm Editorial** (default) | Mixed audience, friendly tone | Bricolage Grotesque + DM Sans | Teal `#2A7B9B` |
| **Terminal IDE** | Dev tool, CLI, hacker aesthetic | JetBrains Mono + Inter | Terminal green `#39D353` |
| **Electric Studio** | AI/ML/SaaS, enterprise feel | Manrope + IBM Plex Sans | Electric blue `#4361EE` |
| **Paper & Ink** | Docs, knowledge base, literary | Cormorant + Source Serif 4 | Crimson `#C41E3A` |

All themes share the same component CSS — only the design tokens differ. Full specs in [`references/themes.md`](references/themes.md).

## Interactive Elements

### DOM primitives ([`references/interactive-elements.md`](references/interactive-elements.md))

| Element | Purpose |
|---------|---------|
| Chat Animation | Simulate the problem the project solves |
| Flow Animation | Step-by-step data flow walkthrough |
| Data Journey | Vertical diagram of a complete scenario |
| Quiz | Multiple choice with instant feedback |
| Decision Flow | Nested branching chart (mobile fallback) |
| Glossary Tooltip | Hover to reveal term definitions |
| Mind Map Grid | 3×2 cards (mobile fallback) |
| Before/After Cards | User experience change comparison |
| Workflow Steps | Numbered steps with tool badges + mock outputs |
| Concept Comparison | Analogy-based callouts for abstract distinctions |
| Source Badges | Colored labels linking to original posts |
| Navigation | Progress bar + dot navigation + keyboard arrows |

### SVG charts ([`references/svg-charts.md`](references/svg-charts.md))

| Chart | Where it appears |
|---|---|
| **Radar chart** | Module 4 — multi-dimensional competitor comparison |
| **Sparkline** | Hero — GitHub stars growth with milestone markers |
| **Decision Tree** | Module 4 — clickable, path-highlighting |
| **Radial Mind Map** | Module 3 — center + 6 spokes, hover-highlights |

Each SVG chart has a mobile fallback to its DOM equivalent (≤600px).

### Animation effects ([`references/animation-effects.md`](references/animation-effects.md))

Per-theme hero backgrounds (mesh gradient / scan lines / two-panel / paper grain), count-up number animation, SVG path draw-in, parallax-lite, scroll-linked module number indicator, `prefers-reduced-motion` enforcement.

## Share & Export

After generation, the skill offers two optional deliveries:

```bash
# Deploy to a live URL (Vercel)
bash scripts/deploy.sh path/to/guide.html

# Export to PDF (Playwright)
bash scripts/export-pdf.sh path/to/guide.html
```

The export script auto-detects github-guide format (looks for `<section>` elements) and disables scroll-snap before screenshotting each module at 1920×1080.

## File Structure

```
github-guide/
├── SKILL.md                          # Skill definition + Mode A page structure + Mode B spec
├── references/
│   ├── design-system.md              # Shared design tokens (Mode A)
│   ├── themes.md                     # 4 visual theme presets (Mode A)
│   ├── interactive-elements.md       # DOM-based primitives (Mode A: chat / flow / quiz)
│   ├── svg-charts.md                 # SVG charts (Mode A: radar / sparkline / tree / mindmap)
│   ├── animation-effects.md          # Hero bg + count-up + parallax + path draw-in (Mode A)
│   └── markdown-deep-dive.md         # Mode B template — Obsidian frontmatter, callouts, Mermaid,
│                                     # section-by-section template, tone calibration
├── scripts/
│   ├── deploy.sh                     # Vercel deployment (Mode A only)
│   └── export-pdf.sh                 # Multi-page PDF export (Mode A only)
└── examples/
    ├── gbrain-guide-v2.html          # Mode A example: garrytan/gbrain
    └── karpathy-skills-guide.html    # Mode A example: andrej-karpathy-skills
```

## Requirements

- [Claude Code](https://claude.com/claude-code) with skills support
- The generated HTML works in any modern browser (only external dependency: Google Fonts CDN)

## License

MIT
