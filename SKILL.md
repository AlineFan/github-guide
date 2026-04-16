---
name: github-guide
description: "Turn any GitHub repository into a beginner-friendly interactive HTML guide page with flow diagrams, mind maps, quizzes, real use cases, and comparison charts. Use this skill whenever someone wants to understand a GitHub repo, create a guide for a repo, explain a project to beginners, analyze an open-source tool, or make a 'how it works' page. Also trigger when users say 'explain this repo', 'make a guide for this GitHub project', 'help me understand this tool', 'create an intro page for this repo', 'what does this repo do', or share a GitHub URL and ask for an explanation or guide. The output is a single self-contained HTML file with warm design, scroll-based navigation, animated diagrams, interactive quizzes, and verified real-world use cases."
---

# GitHub Guide

Transform any GitHub repository into a stunning, beginner-friendly interactive HTML guide. The output is a **single self-contained HTML file** — open it directly in the browser with no setup required (only external dependency: Google Fonts CDN). The guide explains what the project does, how it works, how it compares to alternatives, and how to get started — all through interactive visualizations, plain-language explanations, and verified real-world use cases.

## Language Confirmation

Before starting, ask the user:

> **Would you like this guide in Chinese (中文) or English?**

If the user's message is in English, default to English without asking.
If the user's message is in Chinese, default to Chinese without asking.

## First-Run Welcome

When the skill triggers and the user hasn't specified a repo yet, introduce yourself:

> **I can turn any GitHub repository into an interactive beginner's guide.**
>
> Just point me at a project:
> - **A GitHub link** — e.g., "make a guide for https://github.com/user/repo"
> - **A repo name** — e.g., "explain garrytan/gbrain"
> - **A local clone** — if you're already in a cloned repo
>
> I'll research the project, find real user feedback, and generate a beautiful single-page HTML guide with animated diagrams, comparison charts, interactive quizzes, and real use cases from X/Twitter and the community.

If the user provides a GitHub URL, browse the repo README and key files first. If they reference a local path, read it directly.

## Who This Is For

The target reader is a **non-technical person** or a developer **evaluating whether to use this tool**. They saw the repo trending on GitHub, heard about it on X/Twitter, or got a recommendation — and want to understand what it does, why it matters, and how to get started.

**Assume minimal technical background.** Every technical term needs a plain-language explanation. Use the glossary tooltip pattern (hover to see definition) for inline jargon. Architecture diagrams must have "what this means for you" subtitles, not just technical labels.

**Their goals:**
- Understand what problem this project solves and why it matters
- See how it compares to alternatives (and decide if it's right for them)
- Learn from real user experiences (not marketing copy)
- Get started with the simplest possible setup steps

## Core Principles

### 0. Professional Tone — Warm Expert, Not Cheerleader

Write like a patient mentor explaining to a smart friend. Avoid:
- Hype words: "amazing", "revolutionary", "game-changing", "powerful"
- Excessive emojis in body text (icons in UI elements like cards/badges are fine)
- Marketing copy tone — this is an educational guide, not a landing page
- Condescension — explain jargon, but don't simplify the ideas themselves

Good: "GBrain stores your conversations and notes so Claude can reference them next time."
Bad: "GBrain is an AMAZING tool that REVOLUTIONIZES how you interact with AI! 🚀🔥"

### 1. Every Technical Term Gets a Plain-Language Explanation

Don't just name things — explain what they mean for normal people. This applies everywhere:

**Bad (mind map label):**
```
MCP 协议
接入 Claude、Cursor、Windsurf 等
```

**Good (mind map label):**
```
接入 AI 工具
让 Claude、Cursor 直接用
通过一种叫 MCP 的标准接口，AI 工具可以直接读写你的知识库
```

Use the glossary tooltip pattern for inline terms:
```html
<span class="term" data-def="MCP (Model Context Protocol): a standard interface, like USB lets different devices connect, MCP lets different AI tools connect to your knowledge base.">MCP</span>
```

### 2. Use Cases Must Be Verified and Linked

This is critical. Every use case shown in the guide MUST be:
- **Actually about the specific repo being analyzed** — not about related/similar projects
- **Linked to the original source** with a clickable ↗ link
- **Labeled with a colored source badge** showing where it came from
- **Honest about limitations** — if few real cases exist, say so

Source badge colors:
- GitHub: `#24292f` (black)
- X/Twitter: `#0f1419` (black)
- Reddit: `#ff4500` (orange-red)
- Blog: `#e8590c` (orange)
- DEV/Medium: `#3b49df` (blue)
- HN: `#ff6600` (orange)

**Prioritize high-engagement posts.** When showing X/Twitter or Reddit cases, display view counts and likes in the source badge. A tweet with 985K views is more credible than one with 50 views.

### 3. Interactive Elements Teach Better Than Text

Every major concept should have at least one interactive element:
- **Chat animations** — simulate the problem the project solves (e.g., AI forgetting context)
- **Flow animations** — step-by-step walkthroughs of how data moves through the system
- **Data journey diagrams** — vertical flow showing a complete user scenario
- **Decision flow charts** — help users choose between alternatives
- **Quizzes** — test understanding after each major module (at least 3 total)
- **Mind maps** — show architecture with beginner-friendly labels

### 4. Compare Honestly — Direct Competitors Only

When selecting competitors for comparison:
- **Same-problem competitors only** — tools that solve the exact same problem, not built-in features or DIY workarounds
- **Not built-in features** — e.g., for a memory tool, don't compare with "Claude's built-in memory" (that's a platform feature, not a competing project)
- **Not DIY approaches** — e.g., don't compare with "manually copy-paste into Obsidian" (that's a workaround, not a product)
- **Must be installable/deployable projects** with their own GitHub repo or product page

For each competitor:
- Include clickable links to their GitHub repos
- Show both strengths AND weaknesses
- Use a decision flow chart ("not sure which to choose? follow this flow")
- Don't be a marketing page — be a fair comparison

### 5. Quizzes Test Application, Not Recall

Every quiz must test whether the reader can **apply** the concept, not just repeat it.

**Bad (recall):** "What does MCP stand for?"
**Good (application):** "Your AI assistant forgets everything between sessions. Which approach would help it remember your project context?"

- Wrong answers should be **plausible misconceptions**, not obvious jokes
- Feedback text must explain **why** the answer is correct/incorrect
- At least 3 quizzes total, spread across modules

## Research Phase

Before writing any HTML, gather information in this order:

### Step 1: Understand the Repository
1. Read the README thoroughly
2. Scan the repo structure (key directories, package.json/Cargo.toml/etc.)
3. Check GitHub stats: stars, forks, age, contributors, open issues
4. Read 5-10 recent issues to understand real user pain points
5. Identify the core problem it solves and its unique approach

### Step 2: Find Real Use Cases (Critical Step)

Search for verified, high-engagement user testimonials. Use subagents for parallel research:

**X/Twitter search queries:**
- `"<repo-name>"` — exact match
- `"@<author>" <repo-name>` — author's own posts
- `"<repo-name>" setup OR "set up" OR using OR tried` — user experiences

**Reddit search:**
- `"<repo-name>" site:reddit.com`
- Check relevant subreddits (r/selfhosted, r/LocalLLaMA, r/programming, etc.)

**Other sources:**
- Hacker News: `"<repo-name>" site:news.ycombinator.com`
- DEV.to: `"<repo-name>" site:dev.to`
- Medium: `"<repo-name>" site:medium.com`
- YouTube: search for demo/tutorial videos

**Verification rules:**
- Each use case must specifically mention the repo (not a similar project)
- Prioritize tweets/posts with >10K views, >100 likes
- Save the original URL for every case
- If Reddit/HN have no results, honestly report that

### Step 3: Find Alternatives
Search for competing/similar projects. For each, find:
- GitHub repo URL
- Stars count
- Core differentiator
- Trade-offs vs. the main project

## Page Structure

The HTML guide follows this exact module structure:

### Module 0: Hero Section
- Project name, one-line description, key stats (stars, forks, tools count)
- Scroll hint

### Module 1: Why This Project Exists
- Explain the problem in everyday language
- **Chat animation** simulating the problem (e.g., AI forgetting, manual workflow pain)
- What this project does about it (3 feature cards with icons)
- **Quiz #1** — test understanding of the core problem

### Module 2: How It Works
- Core mechanism explained with a **step-by-step flow animation** (actors + labels)
- **Data journey diagram** — complete vertical flow showing a real scenario
- Key technical insight explained simply (e.g., "smart search" vs "dumb search")
- **Quiz #2** — test understanding of the mechanism

### Module 3: Architecture Overview
- **Mind map** with center node + 6 branches
- Every branch has beginner-friendly title + "what this means for you" description
- Callout box highlighting a key benefit
- **Quiz #3** — test understanding of architecture

### Module 4: Comparison with Alternatives
- 3 approach cards showing different ways to solve the problem (direct competitors only — see Principle #4)
- **Comparison table** with the project highlighted
- Table headers link to competitor GitHub repos where applicable
- **Decision flow chart** — nested questions leading to recommendations

### Module 4.5: Before → After
Show the reader's **experience change** — what life is like without vs. with this tool.

- **Before/After cards** (`.ba-grid` > `.ba-card--before` + `.ba-card--after`)
  - Before: pain points, frustrations, inefficiencies the user faces today
  - After: how the experience improves with this project
- **NOT terminology translation** — don't explain what terms mean. Show what changes for the user.
- **Scenario tags** (`.scenario-tags`) — who specifically benefits: roles, situations, use cases
- Place this between Comparison and Use Cases — the reader now knows what the tool does, how it compares, and this module answers "what would actually change for me?"

```html
<div class="ba-grid">
  <div class="ba-card ba-card--before animate-in">
    <div class="ba-label">❌ Without [Project]</div>
    <div class="ba-item">Pain point 1 the user experiences</div>
    <div class="ba-item">Pain point 2 the user experiences</div>
    <div class="ba-item">Pain point 3 the user experiences</div>
  </div>
  <div class="ba-card ba-card--after animate-in">
    <div class="ba-label">✅ With [Project]</div>
    <div class="ba-item">Improved experience 1</div>
    <div class="ba-item">Improved experience 2</div>
    <div class="ba-item">Improved experience 3</div>
  </div>
</div>
<div class="scenario-tags">
  <span class="scenario-tag">👩‍💻 Role/persona 1</span>
  <span class="scenario-tag">📊 Role/persona 2</span>
</div>
```

### Module 5: Real Use Cases
Organized into 3 groups, each with a labeled header:

**Group 1: Repository examples** (from README/docs)
- 2-3 official use cases with `GitHub` badge

**Group 2: Author's own usage** (from X/Twitter)
- 2-3 tweets from the author, with engagement metrics
- Must include real tweet text and link

**Group 3: Community feedback** (from X/Twitter, Reddit, blogs, code reviews)
- Verified independent user cases with engagement metrics
- GitHub issues that reveal real usage patterns
- Independent code reviews or analyses
- If project is very new, include a callout explaining limited feedback

### Module 6: Quick Start Guide
- Prerequisites (with install commands)
- 3-step getting started flow
- Code blocks with syntax highlighting
- Advanced integration example (if applicable)
- Success callout

### Footer (Inside Last Section)
- Links to repo, license, skill author
- **CRITICAL**: Footer HTML must be placed **inside** the last `<section>` element, not after `</main>`. If placed outside, scroll-snap causes a bounce effect where users can't scroll past the last module.
- Use the `.footer-inner` class for the footer content block

## Design System

Read `references/design-system.md` for the complete design tokens. Key points:

- **Fonts**: Bricolage Grotesque (display), DM Sans (body), JetBrains Mono (code)
- **Colors**: Teal accent `#2A7B9B`, warm off-white `#FAF7F2`, dark text `#2C2A28`
- **Layout**: `max-width: 800px`, scroll-snap modules, `100dvh` sections
- **Animations**: Intersection Observer for scroll-triggered fade-in, stagger-children pattern

## Interactive Elements

Read `references/interactive-elements.md` for complete HTML/CSS/JS patterns for:
- Chat animation (message queue with typing indicator)
- Flow animation (actors + step labels + highlight)
- Data journey (vertical flow with icons)
- Quiz (multiple choice with instant feedback — application, not recall)
- Decision flow (nested branching chart)
- Glossary tooltips (hover-to-reveal definitions)
- Mind map (center node + branches)
- Before/After cards + Scenario tags
- Navigation dots + progress bar

## Output Format

Generate a **single self-contained HTML file** with all CSS and JS inline. The file should:
- Work offline (except Google Fonts CDN)
- Be responsive (mobile-friendly)
- Support keyboard navigation (arrow keys)
- Have scroll-snap for section-by-section navigation
- Include a progress bar and navigation dots
- Use Chinese (简体中文) for all content by default, or match the user's language

Save the file next to the source material, or in the user's specified location.

## Quality Checklist

Before delivering the HTML file, verify:

- [ ] Language consistent (all Chinese or all English, matching user's choice)
- [ ] Professional tone — no hype words, no excessive emojis in body text
- [ ] Every technical term has either a tooltip or inline explanation
- [ ] At least 3 interactive quizzes — testing **application**, not recall
- [ ] All use cases are verified as being about THIS specific repo
- [ ] All use cases have clickable source links
- [ ] X/Twitter cases show engagement metrics (views, likes)
- [ ] Competitors are **direct same-problem competitors** (not built-in features or DIY workarounds)
- [ ] Comparison table links to competitor GitHub repos
- [ ] Decision flow chart is included in comparison section
- [ ] Before→After module shows **user experience change** (not terminology explanation)
- [ ] Scenario tags identify who benefits
- [ ] Data journey diagram is included in "how it works" section
- [ ] Mind map branches have beginner-friendly descriptions
- [ ] Chat animation demonstrates the core problem
- [ ] **Footer is inside the last `<section>`** (not after `</main>`)
- [ ] **Nav dots count matches section count**
- [ ] Page is responsive and works on mobile
- [ ] All interactive elements (quizzes, flow, chat) work correctly
