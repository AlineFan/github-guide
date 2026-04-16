# Interactive Elements

Complete HTML/CSS/JS patterns for all interactive components. Copy and adapt these patterns — don't reinvent them.

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

## 12. Footer (Inside Last Section)

**CRITICAL**: Place footer inside the last `<section>`, not after `</main>`. Prevents scroll-snap bounce.

### HTML Structure
```html
<!-- Inside the last <section>, at the bottom -->
<div class="footer-inner">
  <p>Interactive guide generated with ⚡</p>
  <div class="footer-links">
    <span>Skill Author: <a href="https://github.com/AlineFan" target="_blank">@AlineFan</a></span>
    <span class="footer-sep">·</span>
    <span>Source: <a href="REPO_URL" target="_blank">GitHub</a></span>
  </div>
</div>
```

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
