# Theme Presets

4 visual themes for github-guide. Pick based on the repo's character — don't force one preset on every project. Each theme defines: fonts (Google Fonts), color palette (CSS variables), hero background style, and badge/accent treatments.

The **Warm Editorial** theme is the original github-guide style — use as default when the repo doesn't clearly match another theme.

---

## How to Pick

Before generating the HTML, look at the repo and decide:

| Repo character | Theme |
|---|---|
| Dev tool, CLI, terminal-oriented, hacker culture | **Terminal IDE** |
| AI / ML / data infrastructure, modern SaaS | **Electric Studio** |
| Documentation tool, knowledge base, content/CMS | **Paper & Ink** |
| Default — friendly, beginner-focused, mixed audience | **Warm Editorial** |

If unsure, ask the user with `AskUserQuestion`:

> "What kind of project is this? — Dev tool / AI infra / Docs-content / Default"

Then load only the chosen theme's variables. All four themes share the same component CSS (cards, callouts, quiz, etc.) — only the design tokens change.

---

## Theme 1: Warm Editorial (Default)

The original github-guide aesthetic. Warm off-white, teal accent, editorial serif headlines. Approachable for beginners.

### Fonts

```html
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,600;12..96,700;12..96,800&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600;9..40,700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

### Tokens

```css
:root {
  --font-display: 'Bricolage Grotesque', system-ui, sans-serif;
  --font-body: 'DM Sans', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --color-bg:             #FAF7F2;
  --color-bg-warm:        #F5F0E8;
  --color-bg-code:        #1E1E2E;
  --color-text:           #2C2A28;
  --color-text-secondary: #6B6560;
  --color-text-muted:     #9E9790;
  --color-border:         #E5DFD6;
  --color-border-light:   #EEEBE5;
  --color-surface:        #FFFFFF;
  --color-surface-warm:   #FDF9F3;
  --color-accent:         #2A7B9B;
  --color-accent-hover:   #1F6280;
  --color-accent-light:   #E4F2F7;
  --color-accent-muted:   #5A9DB8;
  --color-success:        #2D8B55;
  --color-success-light:  #E8F5EE;
  --color-error:          #C93B3B;
  --color-error-light:    #FDE8E8;
  --color-info:           #D4A843;
  --color-info-light:     #FDF4E3;
  --color-actor-1: #2A7B9B;
  --color-actor-2: #D94F30;
  --color-actor-3: #7B6DAA;
  --color-actor-4: #D4A843;
  --color-actor-5: #2D8B55;
}
```

### Hero Background

Subtle radial-gradient mesh + paper-grain SVG noise overlay (see [animation-effects.md](animation-effects.md) §1).

---

## Theme 2: Terminal IDE

Dark, hacker-grade aesthetic. JetBrains Mono everywhere. Terminal-green accents, syntax-highlighted code feel throughout. Use for CLI tools, dev infrastructure, low-level repos.

### Fonts

```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Note: Inter is normally on the "don't use" list — but in Terminal IDE it serves a specific role as the *body* contrast to mono display. The signature font is JetBrains Mono.

### Tokens

```css
:root {
  --font-display: 'JetBrains Mono', monospace;
  --font-body: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --color-bg:             #0D1117;
  --color-bg-warm:        #161B22;
  --color-bg-code:        #010409;
  --color-text:           #E6EDF3;
  --color-text-secondary: #8B949E;
  --color-text-muted:     #6E7681;
  --color-border:         #30363D;
  --color-border-light:   #21262D;
  --color-surface:        #161B22;
  --color-surface-warm:   #1C2128;
  --color-accent:         #39D353;     /* terminal green */
  --color-accent-hover:   #2EA043;
  --color-accent-light:   rgba(57, 211, 83, 0.12);
  --color-accent-muted:   #7EE787;
  --color-success:        #3FB950;
  --color-success-light:  rgba(63, 185, 80, 0.15);
  --color-error:          #F85149;
  --color-error-light:    rgba(248, 81, 73, 0.15);
  --color-info:           #D29922;
  --color-info-light:     rgba(210, 153, 34, 0.15);
  --color-actor-1: #58A6FF;  /* blue */
  --color-actor-2: #F85149;  /* red */
  --color-actor-3: #BC8CFF;  /* purple */
  --color-actor-4: #D29922;  /* yellow */
  --color-actor-5: #39D353;  /* green */
}
```

### Hero Background

Scan-line overlay + blinking cursor on title. See [animation-effects.md](animation-effects.md) §2.

### Signature Elements

- All numeric stats prefixed with `$` or `>` like terminal output
- Code blocks have no rounded corners (sharp `border-radius: 2px`)
- Source badges look like `[github]` `[x.com]` (bracket-wrapped mono)
- Footer mimics `tail -f` log lines

### Before / After cards in dark theme

```css
.ba-card--before { background: rgba(248, 81, 73, 0.08); border-color: rgba(248, 81, 73, 0.3); }
.ba-card--after  { background: rgba(63, 185, 80, 0.08); border-color: rgba(63, 185, 80, 0.3); }
.ba-card--before .ba-label { color: #F85149; }
.ba-card--after  .ba-label { color: #3FB950; }
```

---

## Theme 3: Electric Studio

Bold, high-contrast SaaS aesthetic. Manrope display, electric blue accent. For AI/ML/data infrastructure projects that need to feel "modern, serious, well-funded."

### Fonts

```html
<link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&family=IBM+Plex+Sans:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

### Tokens

```css
:root {
  --font-display: 'Manrope', system-ui, sans-serif;
  --font-body: 'IBM Plex Sans', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --color-bg:             #FAFBFC;
  --color-bg-warm:        #F2F4F8;
  --color-bg-code:        #0A0E1A;
  --color-text:           #0A0E1A;
  --color-text-secondary: #4A5568;
  --color-text-muted:     #8B95A7;
  --color-border:         #E2E6EE;
  --color-border-light:   #EEF1F5;
  --color-surface:        #FFFFFF;
  --color-surface-warm:   #F8FAFC;
  --color-accent:         #4361EE;     /* electric blue */
  --color-accent-hover:   #3651D4;
  --color-accent-light:   #E4EAFE;
  --color-accent-muted:   #7B8FFD;
  --color-success:        #10B981;
  --color-success-light:  #D1FAE5;
  --color-error:          #EF4444;
  --color-error-light:    #FEE2E2;
  --color-info:           #F59E0B;
  --color-info-light:     #FEF3C7;
  --color-actor-1: #4361EE;
  --color-actor-2: #EF4444;
  --color-actor-3: #8B5CF6;
  --color-actor-4: #F59E0B;
  --color-actor-5: #10B981;
}
```

### Hero Background

Two-panel diagonal split: top 60% white, bottom 40% electric-blue gradient. Title sits across both halves with mix-blend-mode. See [animation-effects.md](animation-effects.md) §3.

### Signature Elements

- Module numbers as oversized outline numbers (`-webkit-text-stroke`)
- Heavy `font-weight: 800` headlines, tight letter-spacing
- Cards have a single accent border on left edge (4px solid blue)
- Use case cards stack vertically, not in a grid — like a feed

---

## Theme 4: Paper & Ink

Literary, editorial, slow-reading. Cormorant serif display, generous white space, crimson accent. For documentation tools, knowledge bases, anything that wants to feel like a book.

### Fonts

```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant:opsz,wght@14..32,400;14..32,500;14..32,600;14..32,700&family=Source+Serif+4:opsz,wght@8..60,400;8..60,500;8..60,600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### Tokens

```css
:root {
  --font-display: 'Cormorant', Georgia, serif;
  --font-body: 'Source Serif 4', Georgia, serif;
  --font-mono: 'JetBrains Mono', monospace;

  --color-bg:             #FAF9F7;
  --color-bg-warm:        #F2EFE9;
  --color-bg-code:        #1A1A1A;
  --color-text:           #1A1A1A;
  --color-text-secondary: #4A4A4A;
  --color-text-muted:     #8A8580;
  --color-border:         #D9D4CC;
  --color-border-light:   #E8E4DC;
  --color-surface:        #FFFFFF;
  --color-surface-warm:   #FDFCFA;
  --color-accent:         #C41E3A;     /* crimson */
  --color-accent-hover:   #A01831;
  --color-accent-light:   #FBE4E8;
  --color-accent-muted:   #D85470;
  --color-success:        #1B5E20;
  --color-success-light:  #E8F5E9;
  --color-error:          #B71C1C;
  --color-error-light:    #FFEBEE;
  --color-info:           #B8860B;
  --color-info-light:     #FFF8E1;
  --color-actor-1: #C41E3A;
  --color-actor-2: #2E5C8A;
  --color-actor-3: #6A4C93;
  --color-actor-4: #B8860B;
  --color-actor-5: #1B5E20;
}
```

### Hero Background

Cream paper texture (SVG noise) + a single decorative horizontal rule with center dot ornament. No gradients. See [animation-effects.md](animation-effects.md) §4.

### Signature Elements

- Headlines use Cormorant **italic** for module subtitles (`font-style: italic`)
- First letter of each major paragraph is a drop cap (4 lines tall, accent-colored)
- Pull-quote treatment for key sentences: `font-family: var(--font-display); font-size: 1.5em; font-style: italic;` with left crimson border bar
- Horizontal rules use a center ornament: `<hr class="ornament">` → `* * *` styled

```css
.ornament {
  border: none; height: 24px; position: relative;
  margin: var(--space-10) auto;
}
.ornament::after {
  content: '* * *';
  position: absolute; left: 50%; top: 50%;
  transform: translate(-50%, -50%);
  color: var(--color-accent); letter-spacing: 0.5em;
  font-size: var(--text-lg);
}
.drop-cap::first-letter {
  font-family: var(--font-display);
  font-size: 3.2em; line-height: 0.85;
  float: left; padding: 4px 8px 0 0;
  color: var(--color-accent); font-weight: 700;
}
```

---

## Theme-aware adjustments to existing components

When switching themes, these components need small tweaks beyond just color variables:

### Code blocks

- Warm Editorial, Electric Studio, Paper & Ink: Catppuccin-style dark code blocks (default)
- Terminal IDE: code blocks blend with bg (`background: var(--color-bg-code)`, no border)

### Source badges

Terminal IDE overrides:
```css
.source-badge { font-family: var(--font-mono); border-radius: 2px; }
.source-badge::before { content: '['; }
.source-badge::after { content: ']'; }
```

### Workflow tool badges

Each theme should pick tool-badge bg colors from its own `--color-actor-*` palette, not the original teal/purple/gold.

---

## Theme switcher (optional)

If you want to let the reader switch themes at runtime, expose a button group:

```html
<div class="theme-switcher">
  <button data-theme="warm" class="active">Warm</button>
  <button data-theme="terminal">Terminal</button>
  <button data-theme="electric">Electric</button>
  <button data-theme="paper">Paper</button>
</div>
```

```js
const themes = {
  warm: { /* tokens object */ },
  terminal: { /* tokens object */ },
  // ...
};
document.querySelectorAll('.theme-switcher button').forEach(btn => {
  btn.addEventListener('click', () => {
    const root = document.documentElement;
    Object.entries(themes[btn.dataset.theme]).forEach(([k, v]) => root.style.setProperty(k, v));
    document.querySelectorAll('.theme-switcher button').forEach(b => b.classList.toggle('active', b === btn));
  });
});
```

But default: ship with one chosen theme baked in. Runtime switching is optional polish.

---

## DO NOT

- **Don't use Inter as display font** in any theme except Terminal IDE (where its role is body text contrasting mono display)
- **Don't use generic purple gradients** (`#6366f1` etc.) — Electric Studio uses electric blue, not generic indigo
- **Don't pick a theme by personal preference** — pick based on what the repo *is*. A doc tool in Terminal IDE feels wrong; a CLI in Paper & Ink feels precious
