# SVG Charts

Production-ready SVG chart patterns for github-guide. These replace or supplement the existing DOM-based diagrams in `interactive-elements.md` when you need true graphical comparison. All charts:

- Use inline SVG (no library dependency)
- Inherit colors from `--color-*` CSS variables
- Support reduced-motion via `@media (prefers-reduced-motion: reduce)`
- Animate on scroll via the existing `.animate-in` / IntersectionObserver pipeline
- Are responsive (`viewBox` + `width: 100%`)

---

## 1. Radar Chart — Multi-Dimensional Comparison

Compare 2-3 tools across 5-6 dimensions (e.g., ease of use, performance, community, docs, stability, ecosystem). Replaces or supplements the comparison table in Module 4.

### When to use

- You have 3+ direct competitors
- The dimensions of comparison are roughly equivalent in importance
- You want readers to see relative strength at a glance

### HTML / SVG Structure

```html
<div class="radar-chart animate-in">
  <div class="radar-title">五维对比：本项目 vs 竞品</div>
  <svg viewBox="0 0 400 400" class="radar-svg" role="img" aria-label="多维度对比雷达图">
    <!-- Axis grid: 5 concentric pentagons (or hexagons for 6 dims) -->
    <g class="radar-grid">
      <polygon points="200,40 354,150 295,330 105,330 46,150" />   <!-- 100% -->
      <polygon points="200,72 323,160 276,304 124,304 77,160" />   <!-- 80% -->
      <polygon points="200,104 292,170 257,278 143,278 108,170" /> <!-- 60% -->
      <polygon points="200,136 261,180 238,252 162,252 139,180" /> <!-- 40% -->
      <polygon points="200,168 230,190 219,226 181,226 170,190" /> <!-- 20% -->
    </g>
    <!-- Axis lines from center to vertices -->
    <g class="radar-axes">
      <line x1="200" y1="200" x2="200" y2="40" />
      <line x1="200" y1="200" x2="354" y2="150" />
      <line x1="200" y1="200" x2="295" y2="330" />
      <line x1="200" y1="200" x2="105" y2="330" />
      <line x1="200" y1="200" x2="46" y2="150" />
    </g>
    <!-- Tool A polygon (this project — highlight) -->
    <polygon class="radar-poly radar-poly-self" data-values="0.9,0.85,0.7,0.95,0.8" />
    <!-- Tool B polygon -->
    <polygon class="radar-poly radar-poly-b" data-values="0.6,0.9,0.5,0.7,0.85" />
    <!-- Tool C polygon -->
    <polygon class="radar-poly radar-poly-c" data-values="0.7,0.5,0.85,0.6,0.6" />
    <!-- Axis labels (positioned just past each vertex) -->
    <g class="radar-labels">
      <text x="200" y="28" text-anchor="middle">易用性</text>
      <text x="368" y="153" text-anchor="start">性能</text>
      <text x="308" y="350" text-anchor="middle">社区</text>
      <text x="92" y="350" text-anchor="middle">文档</text>
      <text x="32" y="153" text-anchor="end">稳定</text>
    </g>
  </svg>
  <!-- Legend -->
  <div class="radar-legend">
    <span class="radar-legend-item"><span class="radar-swatch radar-swatch-self"></span>本项目</span>
    <span class="radar-legend-item"><span class="radar-swatch radar-swatch-b"></span>竞品 B</span>
    <span class="radar-legend-item"><span class="radar-swatch radar-swatch-c"></span>竞品 C</span>
  </div>
</div>
```

### CSS

```css
.radar-chart {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-6);
}
.radar-title {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: var(--text-lg);
  margin-bottom: var(--space-4);
  text-align: center;
}
.radar-svg { width: 100%; max-width: 480px; display: block; margin: 0 auto; }

.radar-grid polygon {
  fill: none;
  stroke: var(--color-border);
  stroke-width: 1;
}
.radar-grid polygon:nth-child(1) { fill: var(--color-bg-warm); }
.radar-axes line { stroke: var(--color-border-light); stroke-width: 1; }

.radar-poly {
  stroke-width: 2;
  fill-opacity: 0.18;
  transition: fill-opacity var(--duration-fast);
}
.radar-poly-self { fill: var(--color-accent); stroke: var(--color-accent); }
.radar-poly-b    { fill: var(--color-actor-2); stroke: var(--color-actor-2); }
.radar-poly-c    { fill: var(--color-actor-3); stroke: var(--color-actor-3); }
.radar-poly:hover { fill-opacity: 0.45; }

.radar-labels text {
  font-family: var(--font-mono);
  font-size: 11px;
  fill: var(--color-text-secondary);
  font-weight: 600;
}

.radar-legend {
  display: flex; flex-wrap: wrap; gap: var(--space-4);
  justify-content: center; margin-top: var(--space-4);
  font-size: var(--text-sm);
}
.radar-legend-item { display: inline-flex; align-items: center; gap: 6px; }
.radar-swatch { width: 14px; height: 14px; border-radius: 3px; }
.radar-swatch-self { background: var(--color-accent); }
.radar-swatch-b    { background: var(--color-actor-2); }
.radar-swatch-c    { background: var(--color-actor-3); }
```

### JS — Compute polygon points from `data-values`

```js
function renderRadarPolygons() {
  const cx = 200, cy = 200, radius = 160;
  document.querySelectorAll('.radar-poly').forEach(poly => {
    const values = poly.dataset.values.split(',').map(Number);
    const n = values.length;
    const pts = values.map((v, i) => {
      const angle = -Math.PI / 2 + (2 * Math.PI * i) / n;
      const x = cx + radius * v * Math.cos(angle);
      const y = cy + radius * v * Math.sin(angle);
      return `${x.toFixed(1)},${y.toFixed(1)}`;
    }).join(' ');
    poly.setAttribute('points', pts);
  });
}
renderRadarPolygons();

// Animate on scroll: scale from center
document.querySelectorAll('.radar-poly').forEach(poly => {
  poly.style.transformOrigin = '200px 200px';
  poly.style.transform = 'scale(0)';
  poly.style.transition = 'transform 800ms var(--ease-out)';
});
new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.querySelectorAll('.radar-poly').forEach((p, i) => {
        setTimeout(() => p.style.transform = 'scale(1)', i * 200);
      });
    }
  });
}, { threshold: 0.3 }).observe(document.querySelector('.radar-chart'));
```

### Notes

- **5 dimensions** is the sweet spot (pentagon). 6 also works (hexagon) — adjust grid polygon `points` accordingly. Below 4 looks weird, above 7 gets cramped.
- **Values must be 0-1.** Convert your raw ratings (e.g., 1-10) by dividing by 10.
- **Honest scoring rule:** Never give your project 1.0 across all axes — that signals marketing, not analysis. Reserve 0.95+ for genuine standout strengths.

---

## 2. Sparkline — GitHub Stars / Activity Trend

A compact line chart showing growth over time. Use in Module 0 (Hero) next to the stars stat, or in Module 5 (Use Cases) showing community growth.

### HTML / SVG Structure

```html
<div class="sparkline-block animate-in">
  <div class="sparkline-meta">
    <span class="sparkline-label">⭐ GitHub Stars · 12 个月</span>
    <span class="sparkline-value" data-count-to="12847">0</span>
  </div>
  <svg viewBox="0 0 320 80" class="sparkline-svg" preserveAspectRatio="none">
    <!-- Fill gradient -->
    <defs>
      <linearGradient id="spark-fill" x1="0" x2="0" y1="0" y2="1">
        <stop offset="0%" stop-color="var(--color-accent)" stop-opacity="0.3"/>
        <stop offset="100%" stop-color="var(--color-accent)" stop-opacity="0"/>
      </linearGradient>
    </defs>
    <!-- Filled area under line -->
    <path class="sparkline-area" data-values="120,180,210,250,320,480,650,890,1450,2300,4100,8200,12847" />
    <!-- Line on top -->
    <path class="sparkline-line" data-values="120,180,210,250,320,480,650,890,1450,2300,4100,8200,12847" />
    <!-- Milestone markers -->
    <g class="sparkline-markers">
      <circle cx="0" cy="0" r="4" data-x-pct="0.46" data-y-from="650" data-label="v1.0 发布"/>
      <circle cx="0" cy="0" r="4" data-x-pct="0.84" data-y-from="8200" data-label="HN 首页"/>
    </g>
  </svg>
  <!-- Milestone labels (rendered as flag-style tags) -->
  <div class="sparkline-milestones">
    <span class="milestone-tag" style="left:46%">🚀 v1.0 发布</span>
    <span class="milestone-tag" style="left:84%">📰 HN 首页</span>
  </div>
</div>
```

### CSS

```css
.sparkline-block {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-5);
  position: relative;
}
.sparkline-meta {
  display: flex; justify-content: space-between; align-items: baseline;
  margin-bottom: var(--space-3);
}
.sparkline-label {
  font-family: var(--font-mono); font-size: var(--text-xs);
  color: var(--color-text-muted); text-transform: uppercase; letter-spacing: 0.08em;
}
.sparkline-value {
  font-family: var(--font-display); font-weight: 800;
  font-size: var(--text-3xl); color: var(--color-accent);
}
.sparkline-svg { width: 100%; height: 80px; display: block; }
.sparkline-area { fill: url(#spark-fill); }
.sparkline-line { fill: none; stroke: var(--color-accent); stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }
.sparkline-markers circle {
  fill: var(--color-surface); stroke: var(--color-accent); stroke-width: 2;
  cursor: pointer;
  transition: r var(--duration-fast);
}
.sparkline-markers circle:hover { r: 6; }
.sparkline-milestones {
  position: relative; height: 24px; margin-top: var(--space-2);
}
.milestone-tag {
  position: absolute; transform: translateX(-50%);
  font-size: 11px; color: var(--color-text-muted);
  white-space: nowrap;
}
```

### JS — Compute path from `data-values`

```js
function renderSparklines() {
  const W = 320, H = 80, PAD = 4;
  document.querySelectorAll('.sparkline-area, .sparkline-line').forEach(path => {
    const values = path.dataset.values.split(',').map(Number);
    const max = Math.max(...values), min = Math.min(...values);
    const range = max - min || 1;
    const stepX = (W - PAD * 2) / (values.length - 1);
    const pts = values.map((v, i) => {
      const x = PAD + i * stepX;
      const y = H - PAD - ((v - min) / range) * (H - PAD * 2);
      return [x, y];
    });
    // Smooth curve via simple quadratic interpolation
    let d = `M ${pts[0][0]} ${pts[0][1]}`;
    for (let i = 1; i < pts.length; i++) {
      const [px, py] = pts[i - 1], [cx, cy] = pts[i];
      const mx = (px + cx) / 2;
      d += ` Q ${px} ${py}, ${mx} ${(py + cy) / 2} T ${cx} ${cy}`;
    }
    if (path.classList.contains('sparkline-area')) {
      d += ` L ${W - PAD} ${H - PAD} L ${PAD} ${H - PAD} Z`;
    }
    path.setAttribute('d', d);
  });

  // Position milestone markers
  document.querySelectorAll('.sparkline-markers circle').forEach(c => {
    const xPct = parseFloat(c.dataset.xPct);
    const yFrom = parseFloat(c.dataset.yFrom);
    const sibling = c.closest('svg').querySelector('.sparkline-line');
    const values = sibling.dataset.values.split(',').map(Number);
    const max = Math.max(...values), min = Math.min(...values);
    c.setAttribute('cx', PAD + xPct * (W - PAD * 2));
    c.setAttribute('cy', H - PAD - ((yFrom - min) / (max - min)) * (H - PAD * 2));
  });
}
renderSparklines();
```

### Notes

- **Use real data** when possible — pull star history from `https://api.github.com/repos/<owner>/<repo>/stargazers` (paginated) or the `star-history.com` API.
- **Milestones** should be verifiable (release tags from `git log`, HN/Reddit posts with date + link).
- **Smoothing:** the quadratic interpolation gives a clean curve. For raw step-line look, just use `L` instead of `Q...T`.

---

## 3. SVG Decision Tree — Branching Visual

Replaces the nested `<div>` decision-flow with a real tree: nodes connected by SVG lines, clicking an answer highlights the path.

### HTML Structure

```html
<div class="svg-tree animate-in">
  <div class="svg-tree-title">不知道选哪个？跟着流程走</div>
  <svg viewBox="0 0 640 480" class="svg-tree-svg" role="img" aria-label="决策流程图">
    <!-- Connectors (drawn first so they sit behind nodes) -->
    <g class="tree-edges">
      <path class="tree-edge" data-from="q1" data-to="q2"/>
      <path class="tree-edge" data-from="q1" data-to="a1"/>
      <path class="tree-edge" data-from="q2" data-to="a2"/>
      <path class="tree-edge" data-from="q2" data-to="a3"/>
    </g>
    <!-- Nodes as foreignObject so text wraps naturally -->
    <foreignObject id="q1" class="tree-node tree-node-q" x="240" y="20" width="160" height="60">
      <div xmlns="http://www.w3.org/1999/xhtml" class="tree-node-inner">
        你会用命令行吗？
      </div>
    </foreignObject>
    <foreignObject id="a1" class="tree-node tree-node-a" x="40" y="200" width="160" height="60" data-path="q1">
      <div xmlns="http://www.w3.org/1999/xhtml" class="tree-node-inner">
        <strong>不会 → 推荐工具 A</strong>
        <span class="tree-sub">零门槛</span>
      </div>
    </foreignObject>
    <foreignObject id="q2" class="tree-node tree-node-q" x="440" y="200" width="160" height="60" data-path="q1">
      <div xmlns="http://www.w3.org/1999/xhtml" class="tree-node-inner">
        会 → 在意隐私吗？
      </div>
    </foreignObject>
    <foreignObject id="a2" class="tree-node tree-node-a" x="340" y="400" width="140" height="60" data-path="q1,q2">
      <div xmlns="http://www.w3.org/1999/xhtml" class="tree-node-inner">
        <strong>在意 → 本项目</strong>
        <span class="tree-sub">本地优先</span>
      </div>
    </foreignObject>
    <foreignObject id="a3" class="tree-node tree-node-a" x="500" y="400" width="140" height="60" data-path="q1,q2">
      <div xmlns="http://www.w3.org/1999/xhtml" class="tree-node-inner">
        <strong>不在意 → 工具 C</strong>
        <span class="tree-sub">云端方案</span>
      </div>
    </foreignObject>
  </svg>
</div>
```

### CSS

```css
.svg-tree {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-6);
}
.svg-tree-title {
  font-family: var(--font-display); font-weight: 700;
  font-size: var(--text-lg); text-align: center;
  margin-bottom: var(--space-4);
}
.svg-tree-svg { width: 100%; max-width: 720px; display: block; margin: 0 auto; }

.tree-edge {
  fill: none; stroke: var(--color-border);
  stroke-width: 2; stroke-linecap: round;
  transition: stroke var(--duration-normal), stroke-width var(--duration-normal);
}
.tree-edge.highlighted {
  stroke: var(--color-accent); stroke-width: 3;
}

.tree-node-inner {
  width: 100%; height: 100%;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  padding: var(--space-2) var(--space-3);
  border-radius: var(--radius-sm);
  font-family: var(--font-display); font-weight: 600;
  font-size: var(--text-sm); text-align: center;
  line-height: 1.3;
  transition: transform var(--duration-fast), box-shadow var(--duration-fast);
  cursor: pointer;
  box-sizing: border-box;
}
.tree-node-q .tree-node-inner {
  background: var(--color-info-light);
  border: 2px solid var(--color-info);
  color: var(--color-text);
}
.tree-node-a .tree-node-inner {
  background: var(--color-success-light);
  border: 1.5px solid var(--color-success);
  color: var(--color-success);
}
.tree-node-inner:hover { transform: scale(1.04); box-shadow: var(--shadow-md); }

.tree-sub {
  font-family: var(--font-mono); font-weight: 400;
  font-size: 10px; color: var(--color-text-muted);
  margin-top: 2px;
}

.tree-node.dimmed .tree-node-inner { opacity: 0.35; }
```

### JS — Draw edges + highlight on click

```js
function renderTree() {
  const svg = document.querySelector('.svg-tree-svg');
  if (!svg) return;
  // Auto-draw edges between nodes
  svg.querySelectorAll('.tree-edge').forEach(edge => {
    const from = svg.getElementById(edge.dataset.from);
    const to = svg.getElementById(edge.dataset.to);
    if (!from || !to) return;
    const fx = +from.getAttribute('x') + +from.getAttribute('width') / 2;
    const fy = +from.getAttribute('y') + +from.getAttribute('height');
    const tx = +to.getAttribute('x') + +to.getAttribute('width') / 2;
    const ty = +to.getAttribute('y');
    const midY = (fy + ty) / 2;
    // Curved S-shape connector
    edge.setAttribute('d', `M ${fx} ${fy} C ${fx} ${midY}, ${tx} ${midY}, ${tx} ${ty}`);
  });

  // Click to highlight path
  svg.querySelectorAll('.tree-node-a').forEach(node => {
    node.addEventListener('click', () => {
      const pathIds = (node.dataset.path || '').split(',').filter(Boolean).concat(node.id);
      svg.querySelectorAll('.tree-edge').forEach(e => {
        const inPath = pathIds.includes(e.dataset.from) && pathIds.includes(e.dataset.to);
        e.classList.toggle('highlighted', inPath);
      });
      svg.querySelectorAll('.tree-node').forEach(n => {
        n.classList.toggle('dimmed', !pathIds.includes(n.id));
      });
    });
  });
}
renderTree();
```

### Notes

- **Positions** are hardcoded in the SVG (`x`, `y` attrs). For a 3-level tree, 640×480 viewBox is ample.
- **Nodes use `<foreignObject>`** so text wraps natively — pure SVG `<text>` doesn't wrap.
- **Mobile fallback:** at viewport < 480px, the tree may need to scroll horizontally. Wrap `.svg-tree-svg` in `overflow-x: auto` if needed.

---

## 4. Radial Mind Map — Center + Spokes

Replaces the 3x2 grid mindmap with a true radial layout. Center node, 6 spokes radiating outward, hover highlights spoke + dims others.

### HTML / SVG Structure

```html
<div class="radial-mindmap animate-in">
  <svg viewBox="0 0 600 600" class="radial-svg" role="img" aria-label="项目架构思维导图">
    <!-- Spokes (connectors) -->
    <g class="radial-spokes">
      <line class="radial-spoke" data-branch="b1" x1="300" y1="300" x2="300" y2="80"/>
      <line class="radial-spoke" data-branch="b2" x1="300" y1="300" x2="491" y2="190"/>
      <line class="radial-spoke" data-branch="b3" x1="300" y1="300" x2="491" y2="410"/>
      <line class="radial-spoke" data-branch="b4" x1="300" y1="300" x2="300" y2="520"/>
      <line class="radial-spoke" data-branch="b5" x1="300" y1="300" x2="109" y2="410"/>
      <line class="radial-spoke" data-branch="b6" x1="300" y1="300" x2="109" y2="190"/>
    </g>
    <!-- Center node -->
    <g class="radial-center">
      <circle cx="300" cy="300" r="60" />
      <text x="300" y="295" text-anchor="middle">🧠</text>
      <text x="300" y="320" text-anchor="middle" class="radial-center-label">项目核心</text>
    </g>
    <!-- Branch nodes — use foreignObject for text wrapping -->
    <foreignObject id="b1" class="radial-branch" x="200" y="20" width="200" height="120">
      <div xmlns="http://www.w3.org/1999/xhtml" class="branch-inner">
        <div class="branch-icon">💾</div>
        <div class="branch-title">存储</div>
        <div class="branch-desc"><strong>对你来说</strong><br>本地持久化你的所有对话</div>
      </div>
    </foreignObject>
    <foreignObject id="b2" class="radial-branch" x="430" y="130" width="160" height="120">
      <div xmlns="http://www.w3.org/1999/xhtml" class="branch-inner">
        <div class="branch-icon">🔍</div>
        <div class="branch-title">检索</div>
        <div class="branch-desc"><strong>对你来说</strong><br>语义搜索找出相关上下文</div>
      </div>
    </foreignObject>
    <foreignObject id="b3" class="radial-branch" x="430" y="350" width="160" height="120">
      <div xmlns="http://www.w3.org/1999/xhtml" class="branch-inner">
        <div class="branch-icon">🔌</div>
        <div class="branch-title">接入</div>
        <div class="branch-desc"><strong>对你来说</strong><br>通过 MCP 给 Claude / Cursor 用</div>
      </div>
    </foreignObject>
    <foreignObject id="b4" class="radial-branch" x="200" y="460" width="200" height="120">
      <div xmlns="http://www.w3.org/1999/xhtml" class="branch-inner">
        <div class="branch-icon">⚙️</div>
        <div class="branch-title">同步</div>
        <div class="branch-desc"><strong>对你来说</strong><br>多设备之间同步同一份记忆</div>
      </div>
    </foreignObject>
    <foreignObject id="b5" class="radial-branch" x="10" y="350" width="160" height="120">
      <div xmlns="http://www.w3.org/1999/xhtml" class="branch-inner">
        <div class="branch-icon">🔐</div>
        <div class="branch-title">隐私</div>
        <div class="branch-desc"><strong>对你来说</strong><br>数据始终在本地，不上传</div>
      </div>
    </foreignObject>
    <foreignObject id="b6" class="radial-branch" x="10" y="130" width="160" height="120">
      <div xmlns="http://www.w3.org/1999/xhtml" class="branch-inner">
        <div class="branch-icon">📤</div>
        <div class="branch-title">导出</div>
        <div class="branch-desc"><strong>对你来说</strong><br>随时导出成 Markdown / JSON</div>
      </div>
    </foreignObject>
  </svg>
</div>
```

### CSS

```css
.radial-mindmap {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-6);
}
.radial-svg { width: 100%; max-width: 640px; display: block; margin: 0 auto; }

.radial-spoke {
  stroke: var(--color-border);
  stroke-width: 2;
  stroke-dasharray: 4 4;
  transition: stroke var(--duration-fast), stroke-width var(--duration-fast);
}
.radial-spoke.highlighted {
  stroke: var(--color-accent);
  stroke-width: 3;
  stroke-dasharray: 0;
}

.radial-center circle {
  fill: var(--color-accent);
  stroke: var(--color-accent-hover);
  stroke-width: 3;
}
.radial-center text:first-of-type { font-size: 28px; }
.radial-center-label {
  fill: white; font-family: var(--font-display);
  font-weight: 700; font-size: 13px;
}

.branch-inner {
  background: var(--color-bg-warm);
  border: 1.5px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-3);
  height: calc(100% - 4px);
  text-align: center;
  transition: all var(--duration-normal);
  cursor: pointer;
  box-sizing: border-box;
}
.branch-inner:hover {
  border-color: var(--color-accent);
  background: var(--color-accent-light);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}
.branch-icon { font-size: 22px; margin-bottom: 4px; }
.branch-title {
  font-family: var(--font-display); font-weight: 700;
  font-size: var(--text-base); color: var(--color-text);
  margin-bottom: 4px;
}
.branch-desc {
  font-size: 11px; color: var(--color-text-secondary);
  line-height: 1.5;
}
.branch-desc strong {
  color: var(--color-accent);
  font-family: var(--font-mono); font-size: 10px;
  text-transform: uppercase; letter-spacing: 0.05em;
}

.radial-branch.dimmed .branch-inner { opacity: 0.35; }

@media (max-width: 600px) {
  /* On mobile, fall back to the original 3x2 grid mindmap from interactive-elements.md §7 */
  .radial-mindmap { display: none; }
}
```

### JS — Hover highlight

```js
document.querySelectorAll('.radial-branch').forEach(branch => {
  const spoke = document.querySelector(`.radial-spoke[data-branch="${branch.id}"]`);
  branch.addEventListener('mouseenter', () => {
    spoke?.classList.add('highlighted');
    document.querySelectorAll('.radial-branch').forEach(b => {
      if (b !== branch) b.classList.add('dimmed');
    });
  });
  branch.addEventListener('mouseleave', () => {
    spoke?.classList.remove('highlighted');
    document.querySelectorAll('.radial-branch').forEach(b => b.classList.remove('dimmed'));
  });
});
```

### Notes

- **Positions** are calculated for 6 branches at 60° intervals around a circle of radius ~220 from center (300, 300).
- **Mobile fallback:** the radial layout doesn't scale below ~600px wide. Hide it via media query and show the grid mindmap from interactive-elements.md §7 as fallback.
- **Branch count:** works for 4-8 branches. Recompute spoke endpoints for other counts: `x = 300 + 220 * cos(-π/2 + 2π * i / n)`, `y = 300 + 220 * sin(...)`.

---

## Animation Timing

All four charts should animate in via the same IntersectionObserver pipeline. Stagger order recommendation:

1. **Radar:** polygons scale-in from center (0.2s stagger between layers)
2. **Sparkline:** line draws left-to-right via `stroke-dasharray` animation, then value counts up (see [animation-effects.md](animation-effects.md))
3. **Tree:** nodes fade in top-to-bottom (root → leaves, 0.15s stagger)
4. **Mind map:** center first, then spokes draw outward, then branches pop in

For draw-in line animation:

```css
.sparkline-line {
  stroke-dasharray: 1000;
  stroke-dashoffset: 1000;
  transition: stroke-dashoffset 1.5s var(--ease-out);
}
.sparkline-block.visible .sparkline-line { stroke-dashoffset: 0; }
```

---

## When to use what

| Module | Chart | Why |
|---|---|---|
| Module 0 (Hero) | Sparkline | Show momentum at a glance — credibility signal |
| Module 3 (Architecture) | Radial Mind Map | True hierarchical structure visible |
| Module 4 (Comparison) | Radar Chart | Multi-dimensional gestalt |
| Module 4 (Comparison) | SVG Decision Tree | Interactive "which one?" guidance |

Keep the existing DOM patterns from `interactive-elements.md` as fallback for mobile (≤600px) where SVG charts get cramped.
