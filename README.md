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
| **Hero** | Project name, stats (stars/forks), one-line description |
| **Why It Exists** | Problem explained in everyday language + chat animation |
| **How It Works** | Step-by-step flow animation + data journey diagram |
| **Architecture** | Mind map with beginner-friendly labels |
| **Comparison** | Table + decision flow chart ("which tool should I pick?") |
| **Before → After** | User experience before vs after using the project + "Who needs this?" |
| **Real Use Cases** | Verified cases from X/Twitter, GitHub, blogs with source links |
| **Quick Start** | Copy-paste setup commands |

Every section includes interactive elements — quizzes, animated conversations, flow diagrams — not just walls of text.

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

The generated pages use a warm, readable design:

- **Fonts**: Bricolage Grotesque (headings) · DM Sans (body) · JetBrains Mono (code)
- **Colors**: Teal accent `#2A7B9B` · Warm off-white `#FAF7F2` · Dark text `#2C2A28`
- **Layout**: 800px content width, scroll-snap sections, responsive
- **Animations**: Scroll-triggered fade-in via IntersectionObserver

Full design tokens in [`references/design-system.md`](references/design-system.md).

## Interactive Elements

9 reusable patterns with complete HTML/CSS/JS:

| Element | Purpose |
|---------|---------|
| Chat Animation | Simulate the problem the project solves |
| Flow Animation | Step-by-step data flow walkthrough |
| Data Journey | Vertical diagram of a complete scenario |
| Quiz | Multiple choice with instant feedback |
| Decision Flow | Nested branching "which tool?" chart |
| Glossary Tooltip | Hover to reveal term definitions |
| Mind Map | Architecture overview with 6 branches |
| Source Badges | Colored labels linking to original posts |
| Navigation | Progress bar + dot navigation + keyboard arrows |

Full patterns in [`references/interactive-elements.md`](references/interactive-elements.md).

## File Structure

```
github-guide/
├── SKILL.md                          # Skill definition
├── references/
│   ├── design-system.md              # Design tokens & CSS variables
│   └── interactive-elements.md       # 9 interactive component patterns
└── examples/
    ├── gbrain-guide-v2.html          # Example: garrytan/gbrain
    └── karpathy-skills-guide.html    # Example: andrej-karpathy-skills
```

## Requirements

- [Claude Code](https://claude.com/claude-code) with skills support
- The generated HTML works in any modern browser (only external dependency: Google Fonts CDN)

## License

MIT
