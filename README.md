# /github-guide

A Claude Code skill that turns any GitHub repository into a beautiful, interactive beginner-friendly HTML guide.

Point it at any repo and get a single self-contained HTML page with animated diagrams, comparison charts, interactive quizzes, and verified real-world use cases.

<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-skill-teal?style=flat-square" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/output-single_HTML-orange?style=flat-square" alt="Single HTML Output">
  <img src="https://img.shields.io/badge/language-中文_/_EN-blue?style=flat-square" alt="Language">
</p>

## What It Does

Give it a GitHub URL:

```
/github-guide https://github.com/user/repo
```

Get back a **single HTML file** (no build step, no dependencies) containing:

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

Every section includes interactive elements — quizzes, animated conversations, flow diagrams — not just walls of text. Generated guides can be **deployed to a live URL via Vercel** or **exported to PDF** with the included scripts.

## Examples

| Repo | Guide |
|------|-------|
| [garrytan/gbrain](https://github.com/garrytan/gbrain) | [gbrain-guide-v2.html](examples/gbrain-guide-v2.html) |
| [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) | [karpathy-skills-guide.html](examples/karpathy-skills-guide.html) |

> Download the HTML file and open it in your browser — everything is self-contained.

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

```
/github-guide https://github.com/user/repo
```

Or just describe what you want:

```
/github-guide help me understand this repo: https://github.com/anthropics/claude-code
```

The skill will:

1. **Research** — read the README, scan repo structure, check GitHub stats
2. **Find use cases** — search X/Twitter, Reddit, HN, blogs for verified real-world usage
3. **Find alternatives** — identify competing projects for the comparison section
4. **Generate** — output a single HTML file with all interactive elements

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
├── SKILL.md                          # Skill definition + Page Structure
├── references/
│   ├── design-system.md              # Shared design tokens (Warm Editorial defaults)
│   ├── themes.md                     # 4 visual theme presets
│   ├── interactive-elements.md       # DOM-based primitives (chat / flow / quiz / ...)
│   ├── svg-charts.md                 # SVG charts (radar / sparkline / tree / mindmap)
│   └── animation-effects.md          # Hero bg + count-up + parallax + path draw-in
├── scripts/
│   ├── deploy.sh                     # Vercel deployment
│   └── export-pdf.sh                 # Multi-page PDF export
└── examples/
    ├── gbrain-guide-v2.html          # Example: garrytan/gbrain
    └── karpathy-skills-guide.html    # Example: andrej-karpathy-skills
```

## Requirements

- [Claude Code](https://claude.com/claude-code) with skills support
- The generated HTML works in any modern browser (only external dependency: Google Fonts CDN)

## License

MIT
