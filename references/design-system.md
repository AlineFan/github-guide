# Design System

## Typography

Load from Google Fonts CDN:
```html
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,600;12..96,700;12..96,800&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700;1,9..40,400;1,9..40,500&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

| Role | Font | Usage |
|------|------|-------|
| Display | `Bricolage Grotesque` | Headings, module numbers, titles |
| Body | `DM Sans` | Paragraphs, descriptions, UI text |
| Mono | `JetBrains Mono` | Code, labels, badges, progress |

## Color Tokens

```css
:root {
  /* Backgrounds */
  --color-bg:             #FAF7F2;   /* main page */
  --color-bg-warm:        #F5F0E8;   /* alternating sections */
  --color-bg-code:        #1E1E2E;   /* code blocks (Catppuccin) */

  /* Text */
  --color-text:           #2C2A28;
  --color-text-secondary: #6B6560;
  --color-text-muted:     #9E9790;

  /* Borders & Surfaces */
  --color-border:         #E5DFD6;
  --color-border-light:   #EEEBE5;
  --color-surface:        #FFFFFF;
  --color-surface-warm:   #FDF9F3;

  /* Accent (teal) */
  --color-accent:         #2A7B9B;
  --color-accent-hover:   #1F6280;
  --color-accent-light:   #E4F2F7;
  --color-accent-muted:   #5A9DB8;

  /* Semantic */
  --color-success:        #2D8B55;
  --color-success-light:  #E8F5EE;
  --color-error:          #C93B3B;
  --color-error-light:    #FDE8E8;
  --color-info:           #D4A843;
  --color-info-light:     #FDF4E3;

  /* Actor colors (for flow/chat actors) */
  --color-actor-1: #2A7B9B;  /* teal */
  --color-actor-2: #D94F30;  /* red-orange */
  --color-actor-3: #7B6DAA;  /* purple */
  --color-actor-4: #D4A843;  /* gold */
  --color-actor-5: #2D8B55;  /* green */
}
```

## Spacing Scale

```css
--space-1: 0.25rem;  --space-2: 0.5rem;   --space-3: 0.75rem;
--space-4: 1rem;     --space-5: 1.25rem;  --space-6: 1.5rem;
--space-8: 2rem;     --space-10: 2.5rem;  --space-12: 3rem;
--space-16: 4rem;    --space-20: 5rem;
```

## Type Scale

```css
--text-xs: 0.75rem;  --text-sm: 0.875rem;  --text-base: 1rem;
--text-lg: 1.125rem; --text-xl: 1.25rem;   --text-2xl: 1.5rem;
--text-3xl: 1.875rem; --text-4xl: 2.25rem; --text-5xl: 3rem;
--text-6xl: 3.75rem;
```

## Layout

- Content max-width: `800px` (main), `1000px` (wide)
- Navigation height: `50px`
- Sections: `min-height: 100dvh`, `scroll-snap-align: start`
- Alternating backgrounds: `--color-bg` and `--color-bg-warm`

## Shadows & Radii

```css
--shadow-sm:  0 1px 2px rgba(44,42,40,0.05);
--shadow-md:  0 4px 12px rgba(44,42,40,0.08);
--shadow-lg:  0 8px 24px rgba(44,42,40,0.1);
--radius-sm: 8px;  --radius-md: 12px; --radius-lg: 16px;
--radius-full: 9999px;
```

## Animation Tokens

```css
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);
--duration-fast: 150ms;
--duration-normal: 300ms;
--duration-slow: 500ms;
--stagger-delay: 120ms;
```

## Scroll-Triggered Animations

Every element that should animate on scroll gets `class="animate-in"`:
```css
.animate-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity var(--duration-slow) var(--ease-out),
              transform var(--duration-slow) var(--ease-out);
}
.animate-in.visible {
  opacity: 1;
  transform: translateY(0);
}
```

Stagger children inside a container:
```css
.stagger-children > .animate-in {
  transition-delay: calc(var(--stagger-index, 0) * var(--stagger-delay));
}
```

JS to assign stagger indices and trigger animations:
```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target);
    }
  });
}, { rootMargin: '0px 0px -10% 0px', threshold: 0.1 });

document.querySelectorAll('.animate-in').forEach(el => observer.observe(el));

document.querySelectorAll('.stagger-children').forEach(parent => {
  Array.from(parent.children).forEach((child, i) => {
    child.style.setProperty('--stagger-index', i);
  });
});
```

## Responsive Breakpoints

```css
@media (max-width: 768px) {
  :root { --text-4xl: 1.875rem; --text-5xl: 2.25rem; --text-6xl: 3rem; }
  .pattern-cards { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 480px) {
  :root { --text-4xl: 1.5rem; --text-5xl: 1.875rem; --text-6xl: 2.25rem; }
  .pattern-cards { grid-template-columns: 1fr; }
}
```

## Source Badge Colors (for use cases)

```css
.source-badge.github   { background: #24292f; }
.source-badge.x        { background: #0f1419; }
.source-badge.reddit   { background: #ff4500; }
.source-badge.blog     { background: #e8590c; }
.source-badge.dev      { background: #3b49df; }
.source-badge.hn       { background: #ff6600; }
.source-badge.community { background: var(--color-accent); }
```

## Before / After Cards

```css
.ba-card--before { background: #FEF2F2; border-color: #FECACA; }
.ba-card--after  { background: #F0FDF4; border-color: #BBF7D0; }
.ba-card--before .ba-label { color: #DC2626; }
.ba-card--after  .ba-label { color: #16A34A; }
```

## Footer

Footer must be **inside the last `<section>`** to avoid scroll-snap bounce. Use `.footer-inner` class.
