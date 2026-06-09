# Interactive Elements

Complete HTML/CSS/JS patterns for all interactive components. Copy and adapt these patterns — don't reinvent them.

**See also:**
- [svg-charts.md](svg-charts.md) — graphical upgrades: radar chart, sparkline, SVG decision tree, radial mind map. Use these as primary visuals on desktop; the DOM patterns below act as mobile fallbacks.
- [animation-effects.md](animation-effects.md) — hero backgrounds, count-up stats, path draw-in.
- [themes.md](themes.md) — 4 visual themes (Warm Editorial, Terminal IDE, Electric Studio, Paper & Ink).

---

## 1. Chat Animation

Simulates a conversation to demonstrate a problem (e.g., AI forgetting context).

### HTML Structure
```html
<div class="chat-window animate-in" id="chat-problem">
  <div class="chat-header">对话模拟：AI 的日常</div>
  <div class="chat-messages" id="chat-problem-messages"></div>
  <div class="chat-typing" id="chat-problem-typing" style="display:none">
    <div class="chat-avatar" style="background: var(--color-actor-1)">AI</div>
    <div class="typing-dots"><span></span><span></span><span></span></div>
  </div>
  <div class="chat-controls">
    <button class="btn btn-primary chat-next-btn" data-chat="chat-problem">下一条</button>
    <button class="btn chat-all-btn" data-chat="chat-problem">全部播放</button>
    <button class="btn chat-reset-btn" data-chat="chat-problem">重播</button>
    <span class="chat-progress" id="chat-problem-progress"></span>
  </div>
</div>
```

### JS Data & Logic
```js
const chatData = {
  'chat-problem': [
    { sender: '你', color: 'var(--color-actor-5)', avatar: '你', text: '帮我总结一下昨天的会议内容' },
    { sender: 'AI', color: 'var(--color-actor-1)', avatar: 'AI', text: '抱歉，我没有昨天会议的信息。你能发给我吗？' },
    { sender: '你', color: 'var(--color-actor-5)', avatar: '你', text: '……我昨天不是跟你讨论过了吗？' },
    { sender: 'AI', color: 'var(--color-actor-1)', avatar: 'AI', text: '很抱歉，每次新对话我都是从零开始的。😅' },
    { sender: '旁白', color: 'var(--color-actor-4)', avatar: '💡', text: '如果 AI 有了记忆，它会说：...' },
  ]
};

function showNextMessage(chatId) {
  const state = chatStates[chatId];
  const messages = chatData[chatId];
  if (!messages || state.current >= messages.length) return;
  const container = document.getElementById(chatId + '-messages');
  const typing = document.getElementById(chatId + '-typing');
  const msg = messages[state.current];
  typing.style.display = 'flex';
  setTimeout(() => {
    typing.style.display = 'none';
    const el = document.createElement('div');
    el.className = 'chat-message';
    el.innerHTML = `
      <div class="chat-avatar" style="background: ${msg.color}">${msg.avatar}</div>
      <div class="chat-bubble">
        <span class="chat-sender" style="color: ${msg.color}">${msg.sender}</span>
        <p>${msg.text}</p>
      </div>`;
    container.appendChild(el);
    container.scrollTop = container.scrollHeight;
    state.current++;
  }, 800);
}
```

---

## 2. Flow Animation (Step-by-Step)

Shows how data moves through a system with highlighted actors and step labels.

### HTML Structure
```html
<div class="flow-animation animate-in" id="flow-rw">
  <div class="flow-actors">
    <div class="flow-actor" id="flow-rw-step1">
      <div class="flow-actor-icon" style="background: var(--color-accent-light);">📨</div>
      <span>信号到达</span>
    </div>
    <div class="flow-actor" id="flow-rw-step2">
      <div class="flow-actor-icon" style="background: var(--color-info-light);">🔍</div>
      <span>读取记忆</span>
    </div>
    <!-- more actors... -->
  </div>
  <div class="flow-step-label-box" id="flow-rw-label">点击「下一步」开始</div>
  <div class="flow-controls">
    <button class="btn flow-next-btn" data-flow="flow-rw">下一步</button>
    <button class="btn flow-reset-btn" data-flow="flow-rw">重新开始</button>
    <span class="flow-progress" id="flow-rw-progress"></span>
  </div>
</div>
```

### JS Data & Logic
```js
const flowData = {
  'flow-rw': {
    steps: [
      { label: '📨 Step 1 description', highlight: 'flow-rw-step1' },
      { label: '🔍 Step 2 description', highlight: 'flow-rw-step2' },
    ],
    current: 0
  }
};

function flowNext(flowId) {
  const flow = flowData[flowId];
  if (!flow || flow.current >= flow.steps.length) return;
  document.querySelectorAll(`#${flowId} .flow-actor`).forEach(a => a.classList.remove('highlighted'));
  const step = flow.steps[flow.current];
  document.getElementById(step.highlight)?.classList.add('highlighted');
  document.getElementById(flowId + '-label').textContent = step.label;
  flow.current++;
}
```

---

## 3. Data Journey (Vertical Flow)

A vertical diagram showing a complete scenario from start to finish.

### HTML Structure
```html
<div class="journey animate-in" id="journey-data">
  <div class="journey-title">一个问题的完整旅程</div>
  <div class="journey-steps">
    <div class="journey-step active">
      <div class="journey-step-icon" style="border-color: var(--color-actor-1); background: var(--color-accent-light);">💬</div>
      <div class="journey-step-text">
        <div class="journey-step-label">你提了个问题</div>
        <div class="journey-step-desc">比如："上次和客户聊了什么？"</div>
      </div>
    </div>
    <div class="journey-arrow active">↓</div>
    <div class="journey-step active">
      <div class="journey-step-icon" style="border-color: var(--color-info);">🔍</div>
      <div class="journey-step-text">
        <div class="journey-step-label">系统去找线索</div>
        <div class="journey-step-desc">同时用两种方式搜索</div>
      </div>
    </div>
    <div class="journey-arrow active">↓</div>
    <!-- more steps... -->
  </div>
</div>
```

### CSS
```css
.journey-step {
  display: flex; align-items: center; gap: var(--space-4);
  width: 100%; max-width: 500px;
}
.journey-step-icon {
  width: 48px; height: 48px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.3rem; flex-shrink: 0;
  border: 2px solid var(--color-border); background: var(--color-surface);
}
.journey-step.active .journey-step-icon {
  border-color: var(--color-accent); background: var(--color-accent-light);
  box-shadow: 0 0 0 4px rgba(42,123,155,0.12); transform: scale(1.1);
}
.journey-arrow { text-align: center; color: var(--color-border); font-size: 1.2rem; }
.journey-arrow.active { color: var(--color-accent); }
```

---

## 4. Quiz (Multiple Choice)

Interactive quiz with instant feedback. Shows correct answer + explanation.

### HTML Structure
```html
<div class="quiz animate-in" id="quiz-1">
  <div class="quiz-header">
    <div class="quiz-icon">?</div>
    <span class="quiz-label">检查一下你的理解</span>
  </div>
  <div class="quiz-question">问题文本？</div>
  <div class="quiz-options" data-quiz="quiz-1" data-answer="1">
    <button class="quiz-option" data-idx="0">
      <span class="quiz-option-letter">A</span> 选项 A
    </button>
    <button class="quiz-option" data-idx="1">
      <span class="quiz-option-letter">B</span> 选项 B（正确答案）
    </button>
    <button class="quiz-option" data-idx="2">
      <span class="quiz-option-letter">C</span> 选项 C
    </button>
    <button class="quiz-option" data-idx="3">
      <span class="quiz-option-letter">D</span> 选项 D
    </button>
  </div>
  <div class="quiz-explain" id="quiz-1-explain">
    <strong>正确！</strong> 解释为什么这是正确答案...
  </div>
</div>
```

### JS Logic
```js
document.querySelectorAll('.quiz-options').forEach(optionsEl => {
  const quizId = optionsEl.dataset.quiz;
  const correctIdx = parseInt(optionsEl.dataset.answer, 10);
  const buttons = optionsEl.querySelectorAll('.quiz-option');
  const explain = document.getElementById(quizId + '-explain');
  buttons.forEach(btn => {
    btn.addEventListener('click', () => {
      const idx = parseInt(btn.dataset.idx, 10);
      buttons.forEach(b => b.classList.add('disabled'));
      if (idx === correctIdx) btn.classList.add('correct');
      else { btn.classList.add('wrong'); buttons[correctIdx].classList.add('correct'); }
      if (explain) explain.classList.add('show');
    });
  });
});
```

### CSS
```css
.quiz { background: var(--color-surface); border: 2px solid var(--color-accent); border-radius: var(--radius-md); padding: var(--space-6); }
.quiz-option { display: flex; align-items: center; gap: var(--space-3); padding: var(--space-3) var(--space-4); border: 1.5px solid var(--color-border); border-radius: var(--radius-sm); cursor: pointer; width: 100%; text-align: left; background: var(--color-surface); }
.quiz-option.correct { border-color: var(--color-success); background: var(--color-success-light); color: var(--color-success); font-weight: 600; }
.quiz-option.wrong { border-color: var(--color-error); background: var(--color-error-light); color: var(--color-error); opacity: 0.7; }
.quiz-option.disabled { pointer-events: none; }
.quiz-explain { display: none; margin-top: var(--space-4); padding: var(--space-4); background: var(--color-success-light); border-left: 3px solid var(--color-success); }
.quiz-explain.show { display: block; }
```

---

## 5. Decision Flow Chart

> **Desktop:** prefer the SVG version in [svg-charts.md §3](svg-charts.md) — clickable, path-highlighted, more visually engaging. Use the pattern below as the mobile fallback (≤600px).

Nested branching diagram for "which tool should I choose?"

### HTML Structure
```html
<div class="decision-flow animate-in">
  <div class="decision-flow-title">不知道选哪个？跟着这个流程走</div>
  <div class="decision-node decision-q">你会用命令行吗？</div>
  <div class="decision-branches">
    <div class="decision-branch">
      <div class="decision-branch-label">不会</div>
      <div class="decision-arrow-down">↓</div>
      <div class="decision-node decision-a">推荐工具 A</div>
      <div style="font-size: var(--text-xs); color: var(--color-text-muted);">零门槛</div>
    </div>
    <div class="decision-branch">
      <div class="decision-branch-label">会</div>
      <div class="decision-arrow-down">↓</div>
      <div class="decision-node decision-q" style="font-size: var(--text-xs);">你在意隐私吗？</div>
      <div class="decision-branches" style="margin-top: var(--space-2);">
        <!-- nested branches -->
      </div>
    </div>
  </div>
</div>
```

### CSS
```css
.decision-flow { background: var(--color-surface); border: 1px solid var(--color-border); border-radius: var(--radius-md); padding: var(--space-6); text-align: center; }
.decision-node { display: inline-block; padding: var(--space-3) var(--space-5); border-radius: var(--radius-sm); font-weight: 600; }
.decision-q { background: var(--color-info-light); border: 2px solid var(--color-info); border-radius: 999px; }
.decision-a { background: var(--color-success-light); border: 1.5px solid var(--color-success); color: var(--color-success); }
.decision-branches { display: grid; grid-template-columns: 1fr 1fr; gap: var(--space-4); margin-top: var(--space-3); }
```

---

## 6. Glossary Tooltips

Hover to reveal definition of technical terms.

### HTML
```html
<span class="term" data-def="MCP: 一种标准接口协议，让不同 AI 工具能连接你的知识库。">MCP</span>
```

### CSS
```css
.term { border-bottom: 1.5px dashed var(--color-accent-muted); cursor: help; position: relative; }
.term:hover::after {
  content: attr(data-def);
  position: absolute; bottom: calc(100% + 8px); left: 50%; transform: translateX(-50%);
  background: var(--color-text); color: white;
  padding: var(--space-2) var(--space-3); border-radius: var(--radius-sm);
  font-size: var(--text-xs); white-space: normal; width: max-content; max-width: 260px;
  z-index: 100; box-shadow: var(--shadow-lg); line-height: 1.4;
}
```

---

## 7. Mind Map

> **Desktop:** prefer the radial SVG mind map in [svg-charts.md §4](svg-charts.md) — true center-and-spokes structure with hover highlighting. Use the grid pattern below as the mobile fallback (≤600px).

Center node with surrounding branches.

### HTML Structure
```html
<div class="mindmap">
  <div class="mindmap-center">
    <div class="mindmap-connector"></div>
    <div class="mindmap-core"><span class="emoji">🧠</span> Project Name</div>
  </div>
  <div class="mindmap-branches stagger-children">
    <div class="mindmap-branch animate-in">
      <div class="mindmap-branch-icon">💾</div>
      <div class="mindmap-branch-title">Feature Name</div>
      <div class="mindmap-branch-desc">
        <strong>What this means for you</strong><br>
        Plain language explanation
      </div>
    </div>
    <!-- 5 more branches (6 total, 3x2 grid) -->
  </div>
</div>
```

Key rule: every branch MUST have a bold "what this means for you" line + plain description. Never use technical jargon alone.

---

## 8. Use Case Cards with Source Badges

### HTML Structure
```html
<div class="use-case-card animate-in">
  <div class="use-case-emoji" style="background: var(--color-accent-light);">🏢</div>
  <div class="use-case-body">
    <div class="use-case-role">Author · @handle (Platform · 31K Views)</div>
    <div class="use-case-title">Title of the use case</div>
    <p class="use-case-desc">Description with <strong>highlights</strong>.</p>
    <div class="use-case-quote">"Original quote text"
      <div class="use-case-source">
        <a href="https://..." target="_blank" rel="noopener">
          <span class="source-badge x">X</span> @handle · 31K Views ↗
        </a>
      </div>
    </div>
  </div>
</div>
```

### CSS for source links
```css
.use-case-source a {
  color: var(--color-text-muted); text-decoration: none;
  display: inline-flex; align-items: center; gap: 6px;
  transition: color var(--duration-fast);
}
.use-case-source a:hover { color: var(--color-accent); }
```

---

## 9. Navigation & Progress

### HTML
```html
<nav class="nav">
  <div class="progress-bar" id="progress-bar"></div>
  <div class="nav-inner">
    <span class="nav-title">Project 小白指南</span>
    <div class="nav-dots">
      <button class="nav-dot active" data-target="hero" data-tooltip="开始"></button>
      <button class="nav-dot" data-target="module-1" data-tooltip="Module 1"></button>
      <!-- more dots -->
    </div>
  </div>
</nav>
```

### JS
```js
// Progress bar
window.addEventListener('scroll', () => {
  const progress = (window.scrollY / (document.documentElement.scrollHeight - window.innerHeight)) * 100;
  document.getElementById('progress-bar').style.width = progress + '%';
}, { passive: true });

// Nav dots - scroll tracking
const sectionObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      document.querySelectorAll('.nav-dot').forEach(d => d.classList.remove('active'));
      // find and activate matching dot
    }
  });
}, { threshold: 0.3 });

// Keyboard navigation (arrow keys)
document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowDown' || e.key === 'ArrowRight') { /* next section */ }
  if (e.key === 'ArrowUp' || e.key === 'ArrowLeft') { /* prev section */ }
});
```

---

## 10. Before / After Cards

Shows user experience change — what life is like without vs. with the project. **NOT terminology translation** — show pain points and improvements.

### HTML Structure
```html
<div class="ba-grid">
  <div class="ba-card ba-card--before animate-in">
    <div class="ba-label">❌ Without [Project]</div>
    <div class="ba-item">Pain point 1 the user experiences today</div>
    <div class="ba-item">Pain point 2 the user experiences today</div>
    <div class="ba-item">Pain point 3 the user experiences today</div>
  </div>
  <div class="ba-card ba-card--after animate-in">
    <div class="ba-label">✅ With [Project]</div>
    <div class="ba-item">Improved experience 1</div>
    <div class="ba-item">Improved experience 2</div>
    <div class="ba-item">Improved experience 3</div>
  </div>
</div>
```

### CSS
```css
.ba-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: var(--space-4); margin-bottom: var(--space-10);
}
.ba-card {
  border-radius: var(--radius-md); padding: var(--space-5);
  border: 1px solid var(--color-border);
}
.ba-card--before { background: #FEF2F2; border-color: #FECACA; }
.ba-card--after  { background: #F0FDF4; border-color: #BBF7D0; }
.ba-label {
  font-family: var(--font-mono); font-size: var(--text-xs); font-weight: 600;
  text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: var(--space-3);
}
.ba-card--before .ba-label { color: #DC2626; }
.ba-card--after  .ba-label { color: #16A34A; }
.ba-item {
  font-size: var(--text-sm); color: var(--color-text-secondary);
  line-height: 1.8; padding: var(--space-1) 0;
}
@media (max-width: 600px) { .ba-grid { grid-template-columns: 1fr; } }
```

---

## 11. Scenario Tags

Show who specifically benefits from this project. Place below Before/After cards.

### HTML Structure
```html
<div class="scenario-tags">
  <span class="scenario-tag">👩‍💻 Software developers</span>
  <span class="scenario-tag">📊 Product managers</span>
  <span class="scenario-tag">🎓 CS students</span>
  <span class="scenario-tag">🏗️ System architects</span>
</div>
```

### CSS
```css
.scenario-tags {
  display: flex; flex-wrap: wrap; gap: var(--space-2); justify-content: center;
}
.scenario-tag {
  display: inline-flex; align-items: center; gap: 6px;
  padding: var(--space-2) var(--space-4);
  background: var(--color-surface); border: 1px solid var(--color-border);
  border-radius: var(--radius-full); font-size: var(--text-sm);
  color: var(--color-text-secondary);
  transition: all var(--duration-fast);
}
.scenario-tag:hover {
  border-color: var(--color-accent); color: var(--color-accent);
  background: var(--color-accent-light);
}
```

---

## 12. Workflow Steps (Real Scenario)

Shows a concrete end-to-end workflow with numbered steps, tool badges, commands, and mock terminal outputs. Used in Module 4.7 (Real Workflow Scenario).

### HTML Structure
```html
<section id="module-scenario" style="justify-content:flex-start;padding-top:100px">
  <div class="section-inner">
    <div class="module-num animate-in">实战案例</div>
    <h2 class="module-title animate-in">自媒体创作者的完整工作流</h2>
    <p class="module-subtitle animate-in">小 A 是小红书 AI 领域创作者。从选题到发布，全程用 AI 完成。</p>

    <!-- Each step -->
    <div class="workflow-step animate-in">
      <div class="workflow-num">1</div>
      <div class="workflow-connector"></div>
      <div class="workflow-content">
        <span class="workflow-tool-badge last30days">last30days-skill</span>
        <div class="workflow-title">全网调研：最近 30 天大家在聊什么？</div>
        <div class="workflow-desc">描述这一步做什么、用什么工具。</div>
        <div class="code-block" style="margin-bottom:0">
          <span class="comment"># 命令或提示</span>
          /last30days AI Agent 工具
        </div>
        <div class="workflow-output">
          <span class="output-label">↓ 返回结果（节选）</span>
          📊 Reddit r/ClaudeCode · 1,200 upvotes
             "OpenCLI changed my workflow completely"
        </div>
        <div class="callout callout-info" style="margin-top:var(--space-3);margin-bottom:0">
          <div class="callout-icon">💡</div>
          <div class="callout-text"><strong>洞察</strong>：这一步的关键发现。</div>
        </div>
      </div>
    </div>

    <!-- Last step: no .workflow-connector -->
    <div class="workflow-step animate-in">
      <div class="workflow-num">4</div>
      <div class="workflow-content">
        <span class="workflow-tool-badge opencli">OpenCLI</span>
        <div class="workflow-title">一键发布</div>
        <div class="workflow-desc">描述最后一步。</div>
        <!-- command + output blocks -->
      </div>
    </div>

    <!-- Time comparison -->
    <div class="callout callout-success animate-in" style="margin-top:var(--space-4)">
      <div class="callout-icon">⚡</div>
      <div class="callout-text"><strong>全流程耗时对比</strong><br>
        传统方式：90 分钟调研 + 30 分钟写作<br>
        AI 工作流：3 分钟调研 + 2 分钟起草 ≈ <strong>5 分钟</strong></div>
    </div>

    <!-- Cross-tool summary -->
    <div class="callout callout-accent animate-in">
      <div class="callout-icon">🔗</div>
      <div class="callout-text"><strong>工具组合</strong><br>
        Tool A = 调研引擎（全网搜热门讨论）<br>
        Tool B = 执行引擎（搜索 + 发布 + 操作）<br>
        两者由 AI 统一调度。</div>
    </div>
  </div>
</section>
```

### CSS
```css
.workflow-step {
  display: flex; gap: var(--space-5);
  margin-bottom: var(--space-8); position: relative;
}
.workflow-num {
  width: 44px; height: 44px; border-radius: 50%;
  background: var(--color-accent); color: #fff;
  display: flex; align-items: center; justify-content: center;
  font-family: var(--font-display); font-weight: 800;
  font-size: var(--text-xl); flex-shrink: 0; margin-top: 2px;
}
.workflow-content { flex: 1; min-width: 0; }
.workflow-title {
  font-family: var(--font-display); font-weight: 700;
  font-size: var(--text-lg); margin-bottom: var(--space-2);
}
.workflow-desc {
  font-size: var(--text-sm); color: var(--color-text-secondary);
  margin-bottom: var(--space-3); line-height: 1.7;
}
.workflow-output {
  background: var(--color-bg-code); border-radius: var(--radius-md);
  padding: var(--space-4) var(--space-5); margin-top: var(--space-3);
  font-family: var(--font-mono); font-size: var(--text-xs);
  color: #CDD6F4; line-height: 1.8; overflow-x: auto; white-space: pre;
}
.workflow-output .output-label {
  color: rgba(255,255,255,.35); font-size: 10px;
  text-transform: uppercase; letter-spacing: .12em;
  display: block; margin-bottom: var(--space-2);
}
/* Tool badges — add colors per tool */
.workflow-tool-badge {
  display: inline-block; padding: 3px 10px;
  border-radius: var(--radius-full);
  font-family: var(--font-mono); font-size: 10px; font-weight: 600;
  margin-bottom: var(--space-2);
}
.workflow-tool-badge.opencli {
  background: var(--color-accent-light); color: var(--color-accent);
}
.workflow-tool-badge.last30days {
  background: #F3E8FF; color: #7B6DAA;
}
.workflow-tool-badge.claude {
  background: var(--color-info-light); color: #B8860B;
}
/* Connector line between steps */
.workflow-connector {
  position: absolute; left: 21px; top: 48px; bottom: -16px;
  width: 2px; background: var(--color-border);
}
.workflow-step:last-of-type .workflow-connector { display: none; }
```

### Responsive
```css
@media (max-width: 480px) {
  .workflow-step { gap: var(--space-3); }
  .workflow-num { width: 36px; height: 36px; font-size: var(--text-base); }
  .workflow-output { font-size: 11px; padding: var(--space-3); }
}
```

### Notes
- The scenario section uses `justify-content: flex-start` (overriding the default `center`) because content is too long to center vertically
- Tool badge colors should be customized per project — add new `.workflow-tool-badge.<name>` classes as needed
- The `.workflow-connector` line visually connects step numbers; omit it on the last step
- Mock outputs should be realistic but brief — show 3-5 lines max, not full terminal dumps

---

## 13. Concept Comparison Callout

Explains abstract technical distinctions using everyday analogies. Place at the top of Module 4 when comparing tools with fundamentally different approaches.

### HTML Structure
```html
<div class="callout callout-accent animate-in" style="margin-bottom:var(--space-8)">
  <div class="callout-icon">💡</div>
  <div class="callout-text">
    <strong>先搞懂两个概念：CLI 和 MCP</strong><br>
    <strong>CLI</strong>（命令行接口）= 像<strong>打电话点外卖</strong>——你说菜名，服务员帮你下单。<br>
    <strong>MCP</strong>（模型上下文协议）= 像<strong>餐厅的标准菜单</strong>——AI 看到菜单就知道能点什么。<br>
    ProjectA 走的是 <strong>CLI 路线</strong>，ProjectB 走的是 <strong>MCP 路线</strong>。
  </div>
</div>
```

### Notes
- Reuses existing `.callout.callout-accent` styles — no new CSS needed
- The analogy should be relatable and non-technical (food, transportation, daily life)
- Keep it to 3-4 lines — just enough to build the mental model
- After the callout, the comparison cards and table go into details

---

## 14. Footer (Inside Last Section)

**CRITICAL**: Place footer inside the last `<section>`, not after `</main>`. Prevents scroll-snap bounce.

### HTML Structure
```html
<!-- Inside the last <section>, at the bottom -->
<div class="footer-inner">
  <p>Interactive guide generated with ⚡</p>
  <div class="footer-links">
    <span>Created by <strong style="color:var(--color-accent-muted)">林锵锵</strong> · <a href="https://www.xiaohongshu.com/user/profile/66e3e0f8000000000b031363" target="_blank" rel="noopener">小红书 ↗</a> · <a href="https://x.com/ylf1797721" target="_blank" rel="noopener">X ↗</a></span>
    <span class="footer-sep">·</span>
    <span>Built with <a href="https://github.com/AlineFan/github-guide" target="_blank" rel="noopener">github-guide ↗</a></span>
    <span class="footer-sep">·</span>
    <span>讲解对象 <a href="REPO_URL" target="_blank" rel="noopener">REPO_NAME</a> · LICENSE_NAME</span>
  </div>
</div>
```

**Always include all three spans**: Created by 林锵锵 (小红书 + X) / Built with github-guide / 讲解对象 {project name} · {license}. Pull the guided project's license from `gh repo view <repo> --json licenseInfo`.

The **Created by 林锵锵** (小红书 + X) and **Built with github-guide** spans are the skill's fixed attribution — keep them identical, with these exact links, on every generated guide. Only the `讲解对象` span changes per repo.

### CSS
```css
.footer-inner {
  text-align: center; padding: var(--space-10) var(--space-6);
  background: var(--color-bg-warm); border-top: 1px solid var(--color-border-light);
  margin-top: var(--space-10);
}
.footer-inner p { font-size: var(--text-sm); color: var(--color-text-muted); }
.footer-inner a { color: var(--color-accent); text-decoration: none; }
.footer-inner a:hover { text-decoration: underline; }
.footer-links {
  display: inline-flex; align-items: center; gap: 12px;
  flex-wrap: wrap; justify-content: center; margin-top: var(--space-2);
}
```

---

## 15. Approach-Archetype Cards (Solution Landscape)

For **Module 3.5** — show the 2-4 *paradigms* the field uses to solve this problem (NOT specific competing products; those go in Module 4's radar/table). One card per approach; always mark the card this project uses.

### HTML Structure
```html
<div class="approach-grid stagger-children">
  <!-- One card per paradigm. Add class "approach-card--this" to the approach THIS project uses. -->
  <div class="approach-card animate-in">
    <div class="approach-head">
      <span class="approach-icon">📄</span>
      <h4 class="approach-name">存成文档</h4>
    </div>
    <p class="approach-essence">像把笔记记在一个本子上——简单直接，需要时自己翻。</p>
    <div class="approach-tools">代表：<a href="https://github.com/owner/repo" target="_blank" rel="noopener">某工具 ↗</a></div>
    <div class="approach-tradeoff">
      <div class="approach-good"><span>✓ 好在</span>零门槛、完全可控、看得见</div>
      <div class="approach-cost"><span>✗ 代价</span>本子一厚就难找，全靠人记得去翻</div>
    </div>
  </div>

  <div class="approach-card approach-card--this animate-in">
    <div class="approach-badge">这个项目走的就是这条路</div>
    <div class="approach-head">
      <span class="approach-icon">🕸️</span>
      <h4 class="approach-name">知识图谱</h4>
    </div>
    <p class="approach-essence">像给笔记之间拉上线——问一件事，相关的会自己浮出来。</p>
    <div class="approach-tools">代表：<a href="REPO_URL" target="_blank" rel="noopener">本项目 ↗</a></div>
    <div class="approach-tradeoff">
      <div class="approach-good"><span>✓ 好在</span>能顺藤摸瓜找到相关的内容</div>
      <div class="approach-cost"><span>✗ 代价</span>前期得先把关系建起来</div>
    </div>
  </div>
</div>

<!-- Positioning line: the bridge into Module 4 -->
<p class="approach-position animate-in">
  本项目选了 <strong>知识图谱</strong> 这一派——因为它要解决的核心痛点是「记得多但找不到」。下面看看，同样走这条路的几个工具里，它表现如何。
</p>
```

### CSS
```css
.approach-grid {
  display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: var(--space-5); margin: var(--space-8) 0;
}
.approach-card {
  position: relative; display: flex; flex-direction: column; gap: var(--space-3);
  padding: var(--space-6); background: var(--color-surface);
  border: 1px solid var(--color-border); border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  transition: transform var(--duration-normal) var(--ease-out), box-shadow var(--duration-normal) var(--ease-out);
}
.approach-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); }
/* The approach THIS project uses — highlighted, never hidden */
.approach-card--this { border-color: var(--color-accent); box-shadow: 0 0 0 2px var(--color-accent-light), var(--shadow-md); }
.approach-badge {
  position: absolute; top: calc(-1 * var(--space-3)); left: var(--space-5);
  padding: 2px 10px; background: var(--color-accent); color: #fff;
  font-family: var(--font-mono); font-size: var(--text-xs); font-weight: 600; border-radius: var(--radius-full);
}
.approach-head { display: flex; align-items: center; gap: var(--space-2); }
.approach-icon { font-size: var(--text-2xl); }
.approach-name { margin: 0; font-size: var(--text-lg); font-weight: 700; }
.approach-essence { margin: 0; color: var(--color-text-secondary); font-size: var(--text-sm); line-height: 1.6; }
.approach-tools { font-size: var(--text-xs); color: var(--color-text-muted); }
.approach-tools a { color: var(--color-accent); text-decoration: none; }
.approach-tradeoff { margin-top: auto; display: flex; flex-direction: column; gap: var(--space-2); font-size: var(--text-sm); }
.approach-good span, .approach-cost span { font-weight: 600; margin-right: 6px; }
.approach-good { color: var(--color-success); }
.approach-cost { color: var(--color-error); }
.approach-position { max-width: 60ch; margin: var(--space-6) auto 0; text-align: center; font-size: var(--text-base); line-height: 1.7; color: var(--color-text); }
@media (max-width: 600px) { .approach-grid { grid-template-columns: 1fr; } }
```

### Notes
- **Paradigms, not products.** Don't list 3 competing apps here — that's the radar chart in Module 4. List 2-4 *ways of thinking* (document / vector DB / knowledge graph / semi-automatic), each possibly represented by several tools.
- **Always mark the project's own approach** with `--this` + the badge. The reader's takeaway: "this project belongs to *that* school — here's the trade-off it accepted."
- Trade-offs describe the **approach**, not a single tool — e.g. "knowledge graphs need setup" is true of the paradigm, not just this repo.
- A DIY/lightweight approach (e.g. "just a text file") is fair game here as a paradigm, even though Principle 4 bars it from the Module 4 *product* comparison.
- Scale the essence/trade-off wording to reader level (Principle 9): ELI5 leans on the analogy, intern reads a tighter phrasing — but **keep all cards** either way.

---

## 16. Layered Concept Explainer (ELI5 / ELI14 / Intern)

Optional enhancement for a genuinely hard core concept. Lets the reader flip the *same* idea between three depths — the on-page embodiment of "ask for ELI5 / ELI14 / intern" (see SKILL.md § Reader Calibration). The page's overall default depth is still set at generation time; this widget just lets a curious reader go deeper or simpler on one specific term.

### HTML Structure
```html
<div class="depth-card" data-depth="14">
  <div class="depth-card-head">
    <span class="depth-term">什么是「向量数据库」？</span>
    <div class="depth-tabs" role="tablist" aria-label="解释深度">
      <button class="depth-tab" role="tab" data-level="5">🧒 小白</button>
      <button class="depth-tab is-active" role="tab" data-level="14" aria-selected="true">🎓 有基础</button>
      <button class="depth-tab" role="tab" data-level="intern">💼 实习生</button>
    </div>
  </div>
  <div class="depth-body">
    <p class="depth-pane" data-level="5">就像一个「按意思找」的盒子：你丢一句话进去，它能挑出"意思最接近"的其他句子——不用一字一字对上。</p>
    <p class="depth-pane is-active" data-level="14">普通数据库按关键词精确匹配；向量数据库先把文字变成一串数字（坐标），再找坐标上"离得最近"的内容，所以能按语义找相似的东西。</p>
    <p class="depth-pane" data-level="intern">文本经 embedding 模型编码成高维向量，按余弦相似度做近邻检索（ANN，如 HNSW）。换来语义召回，代价是要维护索引、调相似度阈值，且结果不是精确匹配。</p>
  </div>
</div>
```

### JS
```js
document.querySelectorAll('.depth-card').forEach(card => {
  const tabs  = card.querySelectorAll('.depth-tab');
  const panes = card.querySelectorAll('.depth-pane');
  tabs.forEach(tab => tab.addEventListener('click', () => {
    const level = tab.dataset.level;
    tabs.forEach(t => { const on = t === tab; t.classList.toggle('is-active', on); t.setAttribute('aria-selected', on); });
    panes.forEach(p => p.classList.toggle('is-active', p.dataset.level === level));
  }));
});
```

### CSS
```css
.depth-card {
  border: 1px solid var(--color-border); border-radius: var(--radius-lg);
  background: var(--color-surface-warm); padding: var(--space-5) var(--space-6); margin: var(--space-6) 0;
}
.depth-card-head {
  display: flex; flex-wrap: wrap; gap: var(--space-3);
  align-items: center; justify-content: space-between; margin-bottom: var(--space-3);
}
.depth-term { font-weight: 700; font-size: var(--text-lg); }
.depth-tabs { display: inline-flex; gap: 4px; background: var(--color-bg-warm); padding: 3px; border-radius: var(--radius-full); }
.depth-tab {
  border: 0; cursor: pointer; padding: 4px 12px; border-radius: var(--radius-full);
  font-family: var(--font-mono); font-size: var(--text-xs); color: var(--color-text-secondary);
  background: transparent; transition: background var(--duration-fast) var(--ease-out);
}
.depth-tab.is-active { background: var(--color-accent); color: #fff; }
.depth-pane { display: none; margin: 0; line-height: 1.7; color: var(--color-text); }
.depth-pane.is-active { display: block; }
```

### Notes
- **All three depths describe the same fact** — only the wording changes. The ELI5 pane must not be "wrong but simple"; it's the honest core idea minus the jargon (mirrors Principle 9: depth changes wording, never substance).
- Keep it honest (Principle 6a): if the intern pane mentions a cost/token trade-off, the ELI5 pane must not pretend the cost vanishes.
- Default the visible pane to the page's overall level (`data-depth` + matching `is-active`) so the widget opens at the depth the reader picked.
- **Don't overuse.** If every term becomes a tabbed widget, the page turns into a control panel. Reserve for the 1-3 genuinely load-bearing concepts.
- Tab switching is an instant display swap (no motion), so it's `prefers-reduced-motion`-safe by default.
