---
name: github-guide
description: "Turn any GitHub repository into a beginner-friendly interactive HTML guide page with flow diagrams, mind maps, quizzes, real use cases, and comparison charts. Use this skill whenever someone wants to understand a GitHub repo, create a guide for a repo, explain a project to beginners, analyze an open-source tool, or make a 'how it works' page. Also trigger when users say 'explain this repo', 'make a guide for this GitHub project', 'help me understand this tool', 'create an intro page for this repo', 'what does this repo do', or share a GitHub URL and ask for an explanation or guide. The output is a single self-contained HTML file with warm design, scroll-based navigation, animated diagrams, interactive quizzes, and verified real-world use cases."
---

# GitHub Guide

Transform any GitHub repository into a stunning, beginner-friendly interactive HTML guide. The output is a **single self-contained HTML file** — open it directly in the browser with no setup required (only external dependency: Google Fonts CDN). The guide explains what the project does, how it works, how it compares to alternatives, and how to get started — all through interactive visualizations, plain-language explanations, and verified real-world use cases.

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

## Language Confirmation

After receiving a repo URL and before starting research, ask the user which language to use for the guide:

> **请问指南使用中文还是 English？**
> - 中文（默认）
> - English

Use the selected language for ALL content in the generated HTML (module titles, descriptions, quiz questions, callouts, footer, etc.). If the user's initial message is in English, default to English without asking.

## Who This Is For

The target reader is a **non-technical person** or a developer **evaluating whether to use this tool**. They saw the repo trending on GitHub, heard about it on X/Twitter, or got a recommendation — and want to understand what it does, why it matters, and how to get started.

**Assume minimal technical background.** Every technical term needs a plain-language explanation. Use the glossary tooltip pattern (hover to see definition) for inline jargon. Architecture diagrams must have "what this means for you" subtitles, not just technical labels.

**Their goals:**
- Understand what problem this project solves and why it matters
- See how it compares to alternatives (and decide if it's right for them)
- Learn from real user experiences (not marketing copy)
- Get started with the simplest possible setup steps

## Core Principles

### 0. Professional Tone — No Colloquial or Joking Expressions

Write in clear, professional Chinese. Avoid aggressive/colloquial phrasing:
- "说人话" → "所有术语都有白话解释"
- "只用真实案例" → "每条用户评价都经过验证"
- "互动学习" → "用互动元素代替大段文字"
- "随便用，不用付费" → "免费使用，可自由修改和分发"

Translate interactive element names to Chinese descriptions (e.g., "Chat Animation" → "对话动画：用模拟对话演示问题").

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

### 4. Compare Honestly

When comparing with alternatives:
- Include clickable links to competitor GitHub repos
- Show both strengths AND weaknesses
- Use a decision flow chart ("not sure which to choose? follow this flow")
- Don't be a marketing page — be a fair comparison

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

First identify the **core problem** the repo solves (e.g., "AI has no long-term memory"). Then find other projects that solve **the same problem directly**.

**Selection criteria — choose competitors that are:**
- **Same problem, same category**: standalone tools/libraries a user would choose between (e.g., for AI memory: Mem0, Khoj, Letta — all open-source AI memory projects)
- **Open-source with their own GitHub repo** so you can link and compare stars
- **Actively maintained** with meaningful adoption (1K+ stars preferred)

**Do NOT include as competitors:**
- Built-in features of other products (e.g., "Claude Memory" is a feature, not a competing project)
- DIY workarounds or tool combinations (e.g., "Obsidian + RAG plugin" is a workaround, not a purpose-built solution)
- Projects that only tangentially relate to the problem

**For each selected competitor, find:**
- GitHub repo URL
- Stars count
- License type
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
- 3 approach cards showing different ways to solve the problem
- **Comparison table** with the project highlighted (include GitHub Stars row, license type)
- Table headers link to competitor GitHub repos (all competitors must have their own repo)
- **Decision flow chart** — nested questions leading to recommendations

### Before → After + 谁需要这个 (Between Module 4 and Module 5)

This section serves as a summary/bridge after the reader has understood the project and seen alternatives. It contains:

**Before → After comparison:**
- Left card (red tint): 6 pain points of life WITHOUT the project
- Right card (green tint): 6 improvements AFTER using the project
- This shows **real user experience changes**, NOT terminology translation or glossary
- Use concrete, relatable scenarios (e.g., "每次开新对话都要花 10 分钟给 AI 解释背景" → "AI 自动关联上下文")

**谁需要这个？ (Who needs this?):**
- 5 scenario tags describing target users in specific, relatable terms
- Use pill-shaped tags with hover effect

**Do NOT include in this section:**
- Module overview tables or any meta-content about the guide's own structure
- Design principle badges

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

### Footer
- **Must be placed INSIDE the last section** (not as a standalone element after `</main>`), to avoid scroll-snap bounce
- Line 1: "{Project} 小白指南 · 基于 {author}/{repo} · {License}"
- Line 2: "Made by github-guide · skill作者: 林锵锵 · 小红书 · X" with clickable links
- Use muted text color, not black

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
- Quiz (multiple choice with instant feedback)
- Decision flow (nested branching chart)
- Glossary tooltips (hover-to-reveal definitions)
- Mind map (center node + branches)
- Navigation dots + progress bar

## Output Format

Generate a **single self-contained HTML file** with all CSS and JS inline. The file should:
- Work offline (except Google Fonts CDN)
- Be responsive (mobile-friendly)
- Support keyboard navigation (arrow keys)
- Have scroll-snap for section-by-section navigation
- Include a progress bar and navigation dots
- Use the language confirmed with the user (Chinese or English) for all content

Save the file next to the source material, or in the user's specified location.

## Quality Checklist

Before delivering the HTML file, verify:

- [ ] Every technical term has either a tooltip or inline explanation
- [ ] At least 3 interactive quizzes with correct answers and explanations
- [ ] All use cases are verified as being about THIS specific repo
- [ ] All use cases have clickable source links
- [ ] X/Twitter cases show engagement metrics (views, likes)
- [ ] Comparison table links to competitor GitHub repos
- [ ] Decision flow chart is included in comparison section
- [ ] Data journey diagram is included in "how it works" section
- [ ] Mind map branches have beginner-friendly descriptions
- [ ] Chat animation demonstrates the core problem
- [ ] Page is responsive and works on mobile
- [ ] All interactive elements (quizzes, flow, chat) work correctly
