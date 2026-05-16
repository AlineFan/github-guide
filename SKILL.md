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

### 6. Chat Animations Must Be Factually Accurate

Chat animations simulate real scenarios, so every claim about tool capabilities must be verified. Don't claim an AI tool "can't" do something it actually can — instead show the **trade-off** (cost, speed, reliability).

**Bad:**
```
AI: "抱歉，我无法直接访问 B 站网页。"
```
(Wrong — Claude has browser tools and CAN access B站)

**Good:**
```
AI: "好的，让我用浏览器工具打开 B 站……正在截图分析页面……"
旁白: "30 秒后……消耗了约 50K token（约 $0.15），才拿到 5 条结果。而且格式不固定，下次跑可能输出完全不同。"
```
(Accurate — shows the real trade-off instead of a false limitation)

Before writing any chat animation, verify: does the AI tool actually have this capability? If yes, show why the project's approach is better (cheaper, faster, more reliable), not that the alternative is impossible.

#### 6a. Token Accounting Must Be Honest (applies everywhere, not just chat)

A frequent failure mode: claiming a CLI / local tool "uses zero tokens" or "极低 token 消耗". This is **wrong**. The CLI binary itself doesn't burn AI tokens, but:

- The AI **reads the CLI's output** back into its context → input tokens.
- The AI **generates a summary / response** → output tokens.

What a CLI **actually saves** vs. a browser/MCP alternative:

| Cost category | Browser / Heavy MCP | CLI tool |
|---|---|---|
| **Tool schema常驻上下文** | Yes — every tool's description loaded upfront | Cheap or none — Skills loaded on demand |
| **AI deciding *how* to extract data** | Lots — screenshots, DOM parsing, retry loops | Zero — command knows where data lives |
| **Reading the returned data** | Costs tokens (often large HTML/screenshot) | Costs tokens (structured JSON, usually smaller) |
| **Writing the summary** | Costs tokens | Costs tokens (same) |

**Honest framing**: "省的是工具暴露 + 现场决策那部分 token；读 JSON 和写总结的 token 还是要花。"

**Dishonest framing to avoid**: "零 token", "几乎不消耗 token", "free", or anything implying the entire roundtrip is free.

The same rule applies to feature cards, quiz explanations, Before/After items, callouts — anywhere token economics is mentioned, name **which** part is saved.

### 7. Use Everyday Analogies for Abstract Concepts

When the project involves technical concepts that readers might confuse (e.g., CLI vs MCP, REST vs GraphQL, SSR vs CSR), add a **concept comparison callout** with an everyday analogy near the comparison module.

**Example (CLI vs MCP distinction):**
```
CLI（命令行接口）= 打电话点外卖 — 你说菜名，服务员帮你下单
MCP（模型上下文协议）= 餐厅的标准菜单 — AI 看到菜单就知道能点什么
```

Place this callout at the top of Module 4 (Comparison), before the comparison cards. The analogy helps readers understand the fundamental difference before diving into feature-by-feature comparison.

### 8. All Interactive Element Text Must Be Jargon-Free

Principle #1 applies everywhere, but interactive elements deserve extra scrutiny because readers engage with them step by step. Every label in flow animations, data journeys, and mind maps must pass the "would my non-technical friend understand this?" test.

**Bad flow label:** `"找到对应的 bilibili/hot 适配器，确定数据获取策略"`
**Good flow label:** `"找到一个已经写好的「小脚本」来处理这件事"`

**Bad journey description:** `"内置适配器知道 B 站热门数据的 API 端点和字段格式"`
**Good journey description:** `"这个脚本已经知道去哪里拿数据、怎么拿——有人帮你写好了"`

**Bad mind map branch:** `"CLI Hub（整合本地工具）"`
**Good mind map branch:** `"工具统一入口 — 一个入口管所有工具"`

Common jargon to replace (expand this table for each new repo — every CLI / API / auth term must have a plain-language equivalent before the term first appears):

| Jargon | Plain replacement |
|--------|-------------------|
| 守护进程 / daemon | 后台小程序 |
| 适配器 / adapter | 小脚本 / 现成命令 |
| API / API 端点 | 去哪里拿数据 / 后台接口 |
| 管道 / pipe | 交给其他程序处理 |
| CLI Hub | 工具统一入口 |
| CDP 协议 | (删除，不需要暴露给用户) |
| OAuth | 扫码登录（首次出现配 tooltip：「让你授权 AI 用你账号的标准方式」） |
| scope / 权限 scope | 权限范围（举例：「只能看日程」「只能发消息」） |
| smart defaults | 自动填好参数 / 不用记一堆选项 |
| `--as user` / `--as bot` | 用你的身份做 / 用「机器人小号」做 |
| keychain / 钥匙串 | 系统密码保险箱 |
| dry-run | 假装跑一遍（先预览结果） |
| Shortcut（命令） | 一键按钮 / 快捷命令 |
| Skill（工具体系） | 工具 / 功能模块（首次出现说明） |
| MCP / MCP server | 一种把工具暴露给 AI 的标准协议（首次出现配 tooltip） |
| 一对一映射 / map | 一一对应 |
| 兜底 / fallback | 用来兜住没覆盖的情况 |
| token（AI 上下文意义） | 对话里塞着的字数（首次出现解释） |
| token（认证意义） | 通行证 / 登录凭证（与 AI token 区别开） |
| 上下文 / context | 对话里塞着的内容（必要时解释） |
| 调用 | 让 ... 干活 / 发指令 |
| 结构化 (输出) | 规整清单（不是网页 / 不是乱码） |
| 解析（错误消息） | (重写句子：「错误消息直接写下一步怎么改」) |
| 跑生产 / production | 正式运行 / 真实任务 |
| Raw API | 兜底按钮（如果已建立"按钮"比喻） |

**Failure mode to actively avoid**: feature cards / callouts that read like a backend engineer's README. Every card title and first sentence must work standalone for a non-tech reader who skipped earlier modules.

**Real regression example (do NOT repeat)**:

> Bad feature card: "原生权限与身份 — OAuth 登录、按 scope 授权、用户/机器人身份切换 (`--as user` / `--as bot`)、敏感数据走系统钥匙串。机器人跑生产更安全。"

> Good rewrite: "登录一次，权限你说了算 — 扫码登录飞书后，你挑只给 AI 哪些权限（比如只让它看日程、不能发消息）。可以让 AI 用「你的身份」做事，也可以建一个「机器人小号」给它跑——重要的活让机器人做更安全，不会占用你的个人账号。"

Same content, but every term is either explained or replaced with what the reader would say at dinner.

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
Search for competing/similar projects. Cast a wide net — search GitHub, Reddit, HN, and the repo's own issues (users often mention alternatives). Don't rely solely on the README's "Related Projects" section.

For each competitor, find:
- GitHub repo URL and **stars count** (for credibility context)
- **Language/framework** and **team background** (individual vs. university vs. company)
- Core differentiator — what does it do better or worse?
- Trade-offs vs. the main project

### Step 4: Find Complementary Tools (If Available)

Look for tools that work WITH the project — but only include what you can actually verify. Don't invent integrations.

**Where to find them (by reliability):**
1. **README / docs** — explicit "works with", "integrations", "ecosystem" sections (most reliable)
2. **GitHub Issues** — search for issue titles mentioning other tools: `"works with" OR "integrate" OR "combine" OR "together with"`
3. **Author's X/Twitter** — authors often showcase their tool alongside complementary ones
4. **User explicitly mentions one** — if the user provides a complementary tool URL, research it

**When to skip:** If the README doesn't mention integrations and no issues/posts show multi-tool usage, skip Module 4.7 or use a **single-tool workflow** instead (just the project itself, no cross-tool integration). A realistic single-tool scenario is better than a forced multi-tool one.

This feeds the Module 4.7 (Real Workflow Scenario).

### Step 5: Pick a Visual Theme

Before generating HTML, decide which of the 4 themes fits the repo. Read [references/themes.md](references/themes.md) for the full palette specs. Quick decision matrix:

| Repo character | Theme |
|---|---|
| Dev tool, CLI, terminal-oriented, hacker aesthetic | **Terminal IDE** (dark, JetBrains Mono, terminal green) |
| AI / ML / data infra, modern SaaS | **Electric Studio** (electric blue, Manrope, two-panel hero) |
| Documentation, knowledge base, content/CMS | **Paper & Ink** (Cormorant serif, crimson, literary) |
| Default / mixed audience / unsure | **Warm Editorial** (the original — teal + cream + Bricolage) |

**Pick by what the repo *is*, not personal preference.** A doc tool in Terminal IDE feels wrong; a CLI in Paper & Ink feels precious. If genuinely unclear, ask the user via `AskUserQuestion` before generating.

## Page Structure

The HTML guide follows this exact module structure:

### Module 0: Hero Section
- Project name, one-line description, key stats (stars, forks, tools count)
- **Theme-specific hero background** — read [references/animation-effects.md](references/animation-effects.md) §1-4 for the chosen theme's pattern (mesh gradient / scan lines / two-panel split / paper grain)
- **Count-up animation** on stats — read [references/animation-effects.md](references/animation-effects.md) §5
- **Optional sparkline** of GitHub stars trend (12 months) — read [references/svg-charts.md](references/svg-charts.md) §2. Skip if the repo is too new for meaningful data
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
- **Radial mind map** with center node + 6 spokes — read [references/svg-charts.md](references/svg-charts.md) §4. Hover highlights spoke and dims other branches. Falls back to the 3x2 grid mindmap (interactive-elements.md §7) on mobile (≤600px)
- Every branch has beginner-friendly title + "what this means for you" description
- Callout box highlighting a key benefit
- **Quiz #3** — test understanding of architecture

### Module 4: Comparison with Alternatives
- If comparing tools with fundamentally different approaches (e.g., CLI vs MCP, local vs cloud), add a **concept comparison callout** at the top with everyday analogy (see Principle #7)
- 3 approach cards showing different ways to solve the problem (direct competitors only — see Principle #4)
- **Radar chart** comparing this project vs 2 competitors across 5-6 dimensions (ease of use, performance, community, docs, stability, ecosystem) — read [references/svg-charts.md](references/svg-charts.md) §1. Use real, defensible scores; reserve 0.95+ only for genuine standout strengths
- **Comparison table** below the radar — include ecosystem context (language, team background, stars)
- Table headers link to competitor GitHub repos where applicable
- **SVG decision tree** — interactive branching diagram, clicking an answer highlights the path. Read [references/svg-charts.md](references/svg-charts.md) §3. Falls back to the DOM-based `.decision-flow` (interactive-elements.md §5) on mobile

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

### Module 4.7: Real Workflow Scenario (Optional but Recommended)

Show a **concrete, end-to-end use case** with a specific persona, actual commands, and mock outputs. This answers the reader's question: "OK I get what it does, but what does a real workflow actually look like?"

**Two modes depending on research results:**

**Mode A: Cross-tool workflow** (when Step 4 found verified complementary tools)
Show how the project works alongside other tools in a real pipeline. Example: "OpenCLI searches data → last30days does research → Claude drafts → OpenCLI publishes." Each step gets a **tool badge** showing which tool is active.

**Mode B: Single-tool workflow** (when no complementary tools were found)
Show the project's own capabilities in a realistic multi-step scenario. Example: "User searches → filters results → exports → uses output." Still uses numbered steps, commands, and mock outputs — just all within the same tool.

Structure for both modes:
- **Persona intro** — who they are, what they're trying to do (e.g., "小 A 是小红书 AI 领域创作者")
- **Numbered workflow steps** (3-5 steps), each with:
  - **Tool badge** — which tool is used (Mode A: different badges per tool; Mode B: same badge)
  - **Step title** — what this step accomplishes
  - **Command/prompt** — exact command or natural language prompt
  - **Mock output** — realistic terminal/AI output (styled as dark code block)
  - **Insight callout** — what the user learned or discovered at this step
- **Time comparison** — "Traditional: X hours → This workflow: Y minutes"
- **Cross-tool summary** (Mode A only) — a callout explaining each tool's role

Read `references/interactive-elements.md` § "Workflow Steps" for the CSS/HTML pattern.

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

**CRITICAL**: Footer HTML must be placed **inside** the last `<section>` element, not after `</main>`. Outside placement triggers scroll-snap bounce.

Use this exact structure — every guide gets the same skill-author attribution + repo link + license + version:

```html
<div class="footer-inner">
  <p>Interactive guide generated with ⚡</p>
  <div class="footer-links">
    <span>Skill Author: <a href="https://github.com/AlineFan" target="_blank">@AlineFan</a></span>
    <span class="footer-sep">·</span>
    <span>Source: <a href="REPO_URL" target="_blank">GitHub</a></span>
    <span class="footer-sep">·</span>
    <span>License: LICENSE_NAME</span>
    <span class="footer-sep">·</span>
    <span>VERSION_TAG（RELEASE_DATE）</span>
  </div>
</div>
```

Fill in:
- `REPO_URL` — the repo being guided (e.g. `https://github.com/larksuite/cli`)
- `LICENSE_NAME` — from `gh repo view ... --json licenseInfo` (e.g. `MIT`, `Apache 2.0`)
- `VERSION_TAG` — latest release tag from `gh repo view ... --json latestRelease` (e.g. `v1.0.32`)
- `RELEASE_DATE` — `publishedAt` from same query (e.g. `2026-05-15`)

The Skill Author link to **@AlineFan** is the github-guide skill's own author attribution — keep it on every generated guide.

## Design System & Visual Theme

The default tokens (Warm Editorial) live in `references/design-system.md`. The **4 theme variants** (Warm Editorial, Terminal IDE, Electric Studio, Paper & Ink) live in `references/themes.md` — each theme overrides the design tokens with its own fonts and color palette.

**Per-generation flow:**
1. Pick a theme via the matrix in Research Step 5
2. Load the theme block from `references/themes.md` into your `:root { ... }`
3. Use the shared component CSS from `references/design-system.md` for spacing, typography scale, radii, etc.

Key shared properties (all themes share these):
- **Layout**: `max-width: 800px` (1000px wide), scroll-snap modules, `100dvh` sections
- **Spacing/Type scales**: same across themes
- **Animations**: Intersection Observer for scroll-triggered fade-in, stagger-children pattern

## Interactive Elements

Read these three references — they cover all visual components:

### `references/interactive-elements.md` (DOM-based primitives)
- Chat animation (message queue with typing indicator)
- Flow animation (actors + step labels + highlight)
- Data journey (vertical flow with icons)
- Quiz (multiple choice with instant feedback — application, not recall)
- Decision flow (nested branching chart — fallback for mobile)
- Glossary tooltips (hover-to-reveal definitions)
- Mind map grid (3x2 cards — fallback for mobile)
- Before/After cards + Scenario tags
- **Workflow steps** (numbered steps with tool badges, commands, mock outputs)
- **Concept comparison callout** (analogy-based explanation of abstract distinctions)
- Navigation dots + progress bar

### `references/svg-charts.md` (graphical charts — upgrade from DOM primitives)
- **Radar chart** — multi-dimensional comparison (Module 4)
- **Sparkline** — GitHub stars / activity trend with milestones (Module 0 Hero)
- **SVG decision tree** — clickable, path-highlighted branching diagram (Module 4)
- **Radial mind map** — center + 6 spokes with hover highlighting (Module 3)

Each chart has a mobile fallback to its DOM equivalent in interactive-elements.md.

### `references/animation-effects.md` (motion patterns)
- Per-theme hero backgrounds (mesh gradient / scan lines / two-panel / paper grain)
- Count-up number animation for stats
- SVG path draw-in for flow connectors
- Parallax-lite for hero decorations
- Scroll-linked module number indicator
- `prefers-reduced-motion` enforcement

## Output Format

Generate a **single self-contained HTML file** with all CSS and JS inline. The file should:
- Work offline (except Google Fonts CDN)
- Be responsive (mobile-friendly)
- Support keyboard navigation (arrow keys)
- Have scroll-snap for section-by-section navigation
- Include a progress bar and navigation dots
- Use Chinese (简体中文) for all content by default, or match the user's language

Save the file next to the source material, or in the user's specified location.

## Share & Export (Optional)

After generating and saving the HTML, **ask the user:**

> "Want to share this guide? I can deploy it to a live URL (works on any device) or export it as a PDF."

Options: **Deploy to URL** / **Export PDF** / **Both** / **No thanks**. If they decline, stop here.

### Deploy to a Live URL (Vercel)

```bash
bash scripts/deploy.sh <path-to-guide.html>
```

The script (`scripts/deploy.sh`) handles installing Vercel CLI, login prompts, and asset bundling. If the user has never deployed before, walk them through the Vercel signup at https://vercel.com/signup. Redeploying the same file overwrites the previous deploy — same URL stays live.

**Gotchas**:
- Local images referenced via `src="..."` are auto-bundled. Images via CSS `background-image` may be missed — prefer `<img>` tags or inline SVG (charts are already inline, so this rarely matters)
- Spaces in filenames work but become `%20` in URLs — rename to hyphens for cleanliness

### Export to PDF

```bash
bash scripts/export-pdf.sh <path-to-guide.html> [output.pdf]
```

The script auto-detects github-guide vs frontend-slides format (looks for `<section>` vs `.slide`). For github-guide mode it:
1. Disables scroll-snap to allow free positioning
2. Forces all `.animate-in` to `.visible` so animations don't capture mid-frame
3. Screenshots each `<section>` at 1920×1080 in scroll order
4. Combines into a multi-page PDF

**Note on dynamic charts**: SVG charts (radar, sparkline, mindmap) render JS-computed paths. The script waits 400ms per section to let polygons/paths settle before screenshotting. If a chart appears blank in the PDF, increase the wait in the script's `waitForTimeout(400)` call.

**Compact mode** for large guides:
```bash
bash scripts/export-pdf.sh <path-to-guide.html> --compact   # 1280×720 instead of 1920×1080
```

## Plain-Language Pass (MANDATORY before delivery)

After generating the HTML, **before declaring the guide complete**, do a deliberate jargon sweep:

1. **Read every feature-card title + first sentence as if you've skipped earlier sections.** Each must work standalone for a non-tech reader. If the first sentence has more than one untranslated term (MCP, OAuth, scope, Skill, token, API, dry-run, etc.), rewrite it.

2. **Read every callout body out loud.** Imagine your smart-but-non-technical cousin at family dinner. If they would stop you to ask "what's an X?", either tooltip the term or rewrite the sentence.

3. **List every English word and acronym appearing in body text** (excluding code blocks). For each first appearance: is there a tooltip OR an inline plain-language explanation in the same sentence? If neither — fix it.

4. **Read every flow / data-journey / mind-map label.** These are step-by-step UI text — readers engage with them slowly. They MUST pass the "non-technical friend" test (Principle 8). Common offenders: "调用 +shortcut", "OAuth 验证", "结构化返回", "AI 选 Skill", "Raw API 兜底" — all of these need rewriting unless the reader already met the term with explanation.

5. **Quizzes especially.** Quiz options often inherit jargon from the explanation prose. Make sure each quiz option AND the explanation reuse the plain-language framing you established earlier (e.g., "一键按钮" not "Shortcut" if the analogy was introduced in Module 2).

If you skip this pass and ship a guide that uses raw terms like "smart defaults / OAuth / scope / 钥匙串 / dry-run / Raw API" without explanation, you've violated Principles 1 and 8 — the user will rightfully reject the work.

## Quality Checklist

Before delivering the HTML file, verify:

**Content accuracy:**
- [ ] Language consistent (all Chinese or all English, matching user's choice)
- [ ] Professional tone — no hype words, no excessive emojis in body text
- [ ] **Chat animation facts verified** — no false claims about AI tool capabilities; show trade-offs, not fake limitations
- [ ] **Token claims are honest** (Principle 6a): no "零 token" / "几乎不消耗 token" / "free" anywhere. Always name *which* part of the token cost is saved (tool-schema overhead, decision-making) vs. *which* part still has to be paid (reading output, writing summary). Applies to chat, feature cards, quiz explanations, callouts, Before/After items.
- [ ] Every technical term has either a tooltip or inline explanation
- [ ] **No unexplained jargon in flow/journey/mindmap text** — pass the "non-technical friend" test

**Interactive elements:**
- [ ] At least 3 interactive quizzes — testing **application**, not recall
- [ ] Data journey diagram is included in "how it works" section
- [ ] Mind map branches have beginner-friendly descriptions (not just technical labels)
- [ ] Chat animation demonstrates the core problem
- [ ] All interactive elements (quizzes, flow, chat) work correctly

**Comparison & use cases:**
- [ ] All use cases are verified as being about THIS specific repo
- [ ] All use cases have clickable source links
- [ ] X/Twitter cases show engagement metrics (views, likes)
- [ ] Competitors are **direct same-problem competitors** (not built-in features or DIY workarounds)
- [ ] Comparison table links to competitor GitHub repos, shows stars + ecosystem context
- [ ] Decision flow chart branches on user context (language, use case) not just features
- [ ] **Concept comparison callout** with analogy if comparing different technical approaches
- [ ] Before→After module shows **user experience change** (not terminology explanation)
- [ ] Scenario tags identify who benefits

**Workflow scenario (if included):**
- [ ] Concrete persona with a specific goal
- [ ] Each step has tool badge, command, and mock output
- [ ] **Cross-tool integration** shown if complementary tools exist
- [ ] Time comparison (traditional vs AI workflow)

**Theme & visuals:**
- [ ] **Theme chosen by repo character**, not personal preference (see Research Step 5)
- [ ] All `:root` tokens come from one theme block — no mixing across themes
- [ ] Hero background matches the chosen theme (mesh / scan lines / two-panel / paper)
- [ ] Fonts loaded from a single Google Fonts `<link>` matching the theme

**Charts (when included):**
- [ ] **Radar scores are defensible** — no project gets 1.0 across all axes; reserve 0.95+ for genuine standouts
- [ ] **Sparkline data is real** — pulled from GitHub API or star-history.com, not fabricated
- [ ] **Milestone markers** on the sparkline link to verifiable events (release tags, HN posts)
- [ ] **Decision tree paths** highlight correctly on click; mobile fallback works
- [ ] **Radial mindmap** has 4-8 branches; hover dims other branches correctly

**Animation & accessibility:**
- [ ] `prefers-reduced-motion` CSS block is present
- [ ] Count-up stats use `font-variant-numeric: tabular-nums` (prevents jitter)
- [ ] No more than 2 high-impact animations on the same section
- [ ] All SVG charts have `role="img"` and `aria-label`

**Technical:**
- [ ] **Footer is inside the last `<section>`** (not after `</main>`)
- [ ] **Footer has all 4 spans**: Skill Author (`@AlineFan` link) / Source (repo link) / License / Version (tag + date)
- [ ] **Nav dots count matches section count**
- [ ] Page is responsive and works on mobile (chart fallbacks kick in ≤600px)
- [ ] Single self-contained HTML file (only external dep: Google Fonts CDN)
