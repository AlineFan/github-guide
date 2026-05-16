# Animation Effects

Hero backgrounds, count-up numbers, and motion patterns that elevate the guide from "static page" to "designed experience." All effects are CSS or vanilla JS — no libraries.

Every animation MUST respect `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 1. Hero Background — Warm Editorial

Subtle radial-gradient mesh + paper-grain SVG noise. Calm, editorial, doesn't fight with content.

### HTML

```html
<section id="hero" class="hero">
  <div class="hero-bg" aria-hidden="true">
    <div class="hero-mesh"></div>
    <svg class="hero-grain" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
      <filter id="grain">
        <feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves="2" stitchTiles="stitch"/>
        <feColorMatrix values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 0.06 0"/>
      </filter>
      <rect width="100%" height="100%" filter="url(#grain)"/>
    </svg>
  </div>
  <div class="section-inner hero-content">
    <h1 class="hero-title animate-in">Project Name</h1>
    <p class="hero-subtitle animate-in">One-line description</p>
    <!-- stats, sparkline, etc. -->
  </div>
</section>
```

### CSS

```css
.hero { position: relative; overflow: hidden; }
.hero-bg {
  position: absolute; inset: 0;
  pointer-events: none; z-index: 0;
}
.hero-content { position: relative; z-index: 1; }

.hero-mesh {
  position: absolute; inset: -10%;
  background:
    radial-gradient(circle at 20% 30%, var(--color-accent-light) 0%, transparent 40%),
    radial-gradient(circle at 80% 20%, rgba(212, 168, 67, 0.18) 0%, transparent 35%),
    radial-gradient(circle at 50% 80%, rgba(125, 109, 170, 0.12) 0%, transparent 40%);
  filter: blur(40px);
  animation: hero-drift 20s ease-in-out infinite;
}
.hero-grain {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
  opacity: 0.5;
  mix-blend-mode: multiply;
}

@keyframes hero-drift {
  0%, 100% { transform: translate(0, 0) scale(1); }
  33%      { transform: translate(2%, -1%) scale(1.05); }
  66%      { transform: translate(-1%, 2%) scale(0.98); }
}
```

---

## 2. Hero Background — Terminal IDE

Scan-line overlay + blinking cursor on title. Feels like an active terminal session.

### HTML

```html
<section id="hero" class="hero hero-terminal">
  <div class="hero-bg" aria-hidden="true">
    <div class="hero-scanlines"></div>
    <div class="hero-crt-glow"></div>
  </div>
  <div class="section-inner hero-content">
    <div class="terminal-prompt">
      <span class="prompt-user">user@github</span>
      <span class="prompt-sep">:</span>
      <span class="prompt-path">~/projects</span>
      <span class="prompt-sep">$</span>
    </div>
    <h1 class="hero-title animate-in">
      <span class="typed-text">project-name</span><span class="caret">▌</span>
    </h1>
    <p class="hero-subtitle animate-in">One-line description</p>
  </div>
</section>
```

### CSS

```css
.hero-scanlines {
  position: absolute; inset: 0;
  background: repeating-linear-gradient(
    to bottom,
    transparent 0px,
    transparent 2px,
    rgba(255, 255, 255, 0.015) 2px,
    rgba(255, 255, 255, 0.015) 4px
  );
  pointer-events: none;
}
.hero-crt-glow {
  position: absolute; inset: 0;
  background: radial-gradient(ellipse at center, transparent 0%, transparent 60%, rgba(0, 0, 0, 0.4) 100%);
  pointer-events: none;
}

.terminal-prompt {
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  margin-bottom: var(--space-4);
}
.prompt-user { color: var(--color-accent); }
.prompt-path { color: var(--color-actor-1); }
.prompt-sep  { color: var(--color-text-muted); }

.caret {
  display: inline-block;
  color: var(--color-accent);
  animation: blink 1s steps(1) infinite;
  margin-left: 2px;
}
@keyframes blink {
  50% { opacity: 0; }
}

/* Optional: type-in effect for hero title */
.typed-text {
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  border-right: none;
  animation: type 1.5s steps(20, end);
}
@keyframes type {
  from { max-width: 0; }
  to   { max-width: 30ch; }
}
```

---

## 3. Hero Background — Electric Studio

Diagonal two-panel split with blue gradient on the lower half. Title sits across both.

### HTML

```html
<section id="hero" class="hero hero-electric">
  <div class="hero-bg" aria-hidden="true">
    <div class="hero-split-top"></div>
    <div class="hero-split-bottom"></div>
    <div class="hero-accent-stripe"></div>
  </div>
  <div class="section-inner hero-content">
    <div class="hero-eyebrow">A NEW WAY TO BUILD</div>
    <h1 class="hero-title animate-in">Project Name</h1>
    <p class="hero-subtitle animate-in">One-line description</p>
  </div>
</section>
```

### CSS

```css
.hero-electric { color: var(--color-text); }
.hero-electric .hero-bg { z-index: 0; }
.hero-split-top {
  position: absolute; top: 0; left: 0; right: 0;
  height: 60%; background: var(--color-bg);
}
.hero-split-bottom {
  position: absolute; bottom: 0; left: 0; right: 0;
  height: 40%;
  background: linear-gradient(135deg, var(--color-accent) 0%, var(--color-accent-hover) 100%);
}
.hero-accent-stripe {
  position: absolute; bottom: 40%; left: 0; right: 0;
  height: 3px; background: var(--color-text);
  transform-origin: left; transform: scaleX(0);
  animation: stripe-grow 1.2s var(--ease-out) 0.3s forwards;
}
@keyframes stripe-grow {
  to { transform: scaleX(1); }
}

.hero-eyebrow {
  font-family: var(--font-mono); font-size: var(--text-xs);
  font-weight: 600; letter-spacing: 0.2em;
  color: var(--color-accent); margin-bottom: var(--space-4);
}

.hero-electric .hero-title {
  font-weight: 800; letter-spacing: -0.03em;
  /* Outlined oversized number for module nums */
}
.hero-electric .module-num {
  font-size: 6rem; font-weight: 800;
  -webkit-text-stroke: 1.5px var(--color-accent);
  color: transparent;
}
```

---

## 4. Hero Background — Paper & Ink

Cream paper + single horizontal rule with center ornament. Quiet, literary, no animation on hero.

### HTML

```html
<section id="hero" class="hero hero-paper">
  <div class="hero-bg" aria-hidden="true">
    <svg class="hero-grain hero-grain-paper" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
      <filter id="paper-grain">
        <feTurbulence type="fractalNoise" baseFrequency="0.75" numOctaves="3"/>
        <feColorMatrix values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 0.04 0"/>
      </filter>
      <rect width="100%" height="100%" filter="url(#paper-grain)"/>
    </svg>
  </div>
  <div class="section-inner hero-content">
    <div class="hero-eyebrow-paper">— An Interactive Guide —</div>
    <h1 class="hero-title animate-in">Project Name</h1>
    <hr class="ornament">
    <p class="hero-subtitle animate-in"><em>One-line description in italic</em></p>
  </div>
</section>
```

### CSS

```css
.hero-grain-paper {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
  opacity: 0.8;
  mix-blend-mode: multiply;
}

.hero-eyebrow-paper {
  font-family: var(--font-display); font-style: italic;
  font-size: var(--text-base); color: var(--color-text-muted);
  letter-spacing: 0.1em; margin-bottom: var(--space-4);
}

.hero-paper .hero-title {
  font-family: var(--font-display); font-weight: 600;
  font-size: clamp(3rem, 8vw, 5rem);
  line-height: 1.05; letter-spacing: -0.01em;
}
.hero-paper .hero-subtitle {
  font-family: var(--font-display); font-style: italic;
  color: var(--color-text-secondary);
}

/* .ornament defined in themes.md §Paper & Ink */
```

---

## 5. Count-Up Animation for Stats

Numbers in Hero (stars, forks, contributors) animate from 0 to target value on scroll-into-view. Pairs naturally with sparklines.

### HTML

Mark any element with `data-count-to="<number>"`:

```html
<div class="stat-block">
  <div class="stat-value" data-count-to="12847" data-count-duration="1500">0</div>
  <div class="stat-label">⭐ Stars</div>
</div>

<div class="stat-block">
  <div class="stat-value" data-count-to="423" data-count-suffix="K">0</div>
  <div class="stat-label">👁 Monthly views</div>
</div>
```

### JS

```js
function animateCount(el) {
  const target = parseFloat(el.dataset.countTo);
  const duration = parseInt(el.dataset.countDuration, 10) || 1200;
  const prefix = el.dataset.countPrefix || '';
  const suffix = el.dataset.countSuffix || '';
  const decimals = parseInt(el.dataset.countDecimals, 10) || 0;
  const start = performance.now();

  function frame(now) {
    const t = Math.min((now - start) / duration, 1);
    // Ease out cubic — matches --ease-out (0.16, 1, 0.3, 1) feel
    const eased = 1 - Math.pow(1 - t, 3);
    const value = target * eased;
    el.textContent = prefix + value.toLocaleString('en-US', {
      minimumFractionDigits: decimals,
      maximumFractionDigits: decimals,
    }) + suffix;
    if (t < 1) requestAnimationFrame(frame);
  }
  requestAnimationFrame(frame);
}

const countObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting && !entry.target.dataset.counted) {
      entry.target.dataset.counted = '1';
      animateCount(entry.target);
    }
  });
}, { threshold: 0.5 });

document.querySelectorAll('[data-count-to]').forEach(el => countObserver.observe(el));
```

### CSS

```css
.stat-block {
  display: flex; flex-direction: column; align-items: center;
  padding: var(--space-4) var(--space-5);
}
.stat-value {
  font-family: var(--font-display);
  font-weight: 800;
  font-size: clamp(2rem, 5vw, 3.5rem);
  color: var(--color-accent);
  line-height: 1;
  font-variant-numeric: tabular-nums;
  letter-spacing: -0.02em;
}
.stat-label {
  font-family: var(--font-mono); font-size: var(--text-xs);
  text-transform: uppercase; letter-spacing: 0.1em;
  color: var(--color-text-muted); margin-top: var(--space-2);
}
```

### Notes

- **`tabular-nums`** is critical — without it, numbers jitter horizontally as digits change width.
- **Reduced motion:** if user prefers reduced motion, skip the animation and set the value directly. The CSS rule above handles `transition-duration`, but `requestAnimationFrame` doesn't respect it — add explicit check:

  ```js
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    el.textContent = prefix + target.toLocaleString() + suffix;
    return;
  }
  ```

- **Real numbers only.** Don't fake "1.2M downloads" if the repo has 200. Honesty is core to github-guide's tone.

---

## 6. SVG Path Draw-In (Flow Lines)

When a flow animation step advances, draw the connector line from previous step to current step.

### CSS

```css
.flow-connector {
  stroke: var(--color-accent);
  stroke-width: 2;
  fill: none;
  stroke-dasharray: 200;
  stroke-dashoffset: 200;
  transition: stroke-dashoffset 600ms var(--ease-out);
}
.flow-connector.drawn {
  stroke-dashoffset: 0;
}
```

### JS

```js
// When advancing a flow step, draw the connector to the new highlight
function drawConnectorTo(connectorEl) {
  // Set length first (must match actual SVG path length for accurate animation)
  const length = connectorEl.getTotalLength();
  connectorEl.style.strokeDasharray = length;
  connectorEl.style.strokeDashoffset = length;
  // Force reflow before animating
  void connectorEl.getBoundingClientRect();
  connectorEl.classList.add('drawn');
  connectorEl.style.strokeDashoffset = 0;
}
```

---

## 7. Floating Module Cards (Parallax-Lite)

Subtle parallax on module cards as user scrolls — cards drift slightly faster than scroll.

### JS (Performance-conscious — uses `transform` only)

```js
const parallaxEls = document.querySelectorAll('[data-parallax]');
let ticking = false;

function updateParallax() {
  const vh = window.innerHeight;
  parallaxEls.forEach(el => {
    const rect = el.getBoundingClientRect();
    const center = rect.top + rect.height / 2;
    const offsetFromCenter = (center - vh / 2) / vh;
    const speed = parseFloat(el.dataset.parallax) || 0.1;
    el.style.transform = `translateY(${offsetFromCenter * speed * 50}px)`;
  });
  ticking = false;
}

window.addEventListener('scroll', () => {
  if (!ticking) {
    requestAnimationFrame(updateParallax);
    ticking = true;
  }
}, { passive: true });
```

### Usage

```html
<div class="hero-decoration" data-parallax="0.3">...</div>
```

Use sparingly — only on hero decorations and background elements. Never on text or main content.

---

## 8. Scroll-Linked Module Number Indicator

Big translucent module number that fills in as the user scrolls through the section. Visual progress signal.

### HTML

```html
<section id="module-2" data-progress-num="02">
  <!-- module content -->
</section>
```

### CSS

```css
section[data-progress-num]::before {
  content: attr(data-progress-num);
  position: fixed;
  top: 50%; right: 40px;
  transform: translateY(-50%);
  font-family: var(--font-display);
  font-size: 12rem; font-weight: 800;
  color: var(--color-accent);
  opacity: 0;
  background: linear-gradient(to top, var(--color-accent) var(--scroll-progress, 0%), transparent var(--scroll-progress, 0%));
  -webkit-background-clip: text; background-clip: text;
  -webkit-text-fill-color: transparent;
  pointer-events: none;
  transition: opacity 300ms;
}
section[data-progress-num].in-view::before {
  opacity: 0.08;
}

@media (max-width: 800px) {
  section[data-progress-num]::before { display: none; }
}
```

### JS

```js
const numberObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    entry.target.classList.toggle('in-view', entry.isIntersecting);
  });
}, { threshold: [0, 0.3, 0.7, 1] });

document.querySelectorAll('section[data-progress-num]').forEach(s => numberObserver.observe(s));

window.addEventListener('scroll', () => {
  document.querySelectorAll('section[data-progress-num].in-view').forEach(s => {
    const rect = s.getBoundingClientRect();
    const progress = Math.max(0, Math.min(1, -rect.top / (rect.height - window.innerHeight)));
    s.style.setProperty('--scroll-progress', `${progress * 100}%`);
  });
}, { passive: true });
```

---

## When to use what

| Effect | Where | Skip when |
|---|---|---|
| Hero mesh / gradient (§1-4) | Module 0 only | — |
| Count-up (§5) | Hero stats + use case engagement | Reduced motion |
| Path draw-in (§6) | Flow / journey diagrams | Mobile (looks janky) |
| Parallax (§7) | Hero decorations only | Reduced motion, mobile |
| Module number indicator (§8) | Each major module | Width < 800px |

**Don't pile on.** Two well-orchestrated effects (a hero entrance + count-up stats) outperform six scattered micro-animations. Frontend-slides calls this principle out explicitly — apply it here too.
