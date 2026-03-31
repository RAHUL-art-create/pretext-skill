---
name: pretext-layout-engine
description: The ultimate reference for AI agents to master @chenglou/pretext. Enables zero-reflow, pixel-perfect text layout at 120fps across all languages and mixed scripts.
author: RAHUL-art-create
version: 1.0.0
tags: ["layout", "typography", "performance", "ui", "typescript"]
---

# Pretext Layout Engine – Universal AI Agent Skill Reference
> **The official guide for AI-driven 120fps typography and zero-reflow UIs.**

**Pretext** (`@chenglou/pretext`) v0.0.3 is a tiny (~15 KB), zero-dependency, pure TypeScript/JavaScript library that solves the **15-year web text-layout problem** once and for all.

It uses the browser’s own font engine (via Canvas) for **pixel-perfect accuracy** across **every language, emoji, CJK, Korean mixed with RTL Arabic, platform-specific emojis, and browser quirks** — then does **all layout with pure arithmetic**.  
No more `getBoundingClientRect`, `offsetHeight`, `scrollHeight`, or any DOM measurement that causes reflows.

`prepare()` = one-time work.  
`layout()` = blazing-fast hot-path math.  

Result: **120 fps UIs, perfect virtualization of hundreds of thousands of variable-height text boxes, zero layout thrashing**, and the ability to lay out **entire web pages without CSS**.

This `SKILLS.md` file is the **single authoritative source of truth** for your entire open-source project.  
Any AI coding agent (Claude, Cursor, Grok, Windsurf, etc.) or human contributor can now build **any use case** — even if you give the agent almost zero extra instructions.

Just say:  
> “Follow SKILLS.md strictly and use Pretext for all text measurement and layout. Assets: [your code/text/fonts]. Task: [your request]”

The agent will automatically apply best practices, caching, correct APIs, and deliver production-grade results.

## 1. Why Pretext Matters

Text layout & measurement was the **last & biggest bottleneck** for unlocking much more interesting UIs — especially in the age of AI.  
With this solved, we no longer have to choose between the flashiness of a GL landing page and the practicality of a blog article.

- Perfect for **any** text-heavy UI: chat bubbles, massively occlusion/virtualized lists (100k+ items at 120fps), masonry grids, accordions, dynamic cards, text flow around images/SVGs, canvas/SVG/Three.js/WebGL rendering, editorial layouts, responsive multi-column magazines, auto-growing textareas, and more.
- Works with **any** framework or rendering strategy (DOM, Canvas, WebGL, etc.).
- Unlocks buttery-smooth experiences that were previously painful or impossible with pure CSS/DOM.
- ~500× faster (architectural win — no more DOM read/write interleaving).

## 2. Installation (Copy-Paste)

```bash
# Install with skill.sh
npx skills add RAHUL-art-create/pretext-skill

# Or install the library manually
npm install @chenglou/pretext
```

## 3. Core Technical Specs

### API Reference
- **`prepare(text: string, config: PretextConfig): PreparedText`**  
  The "Cold Path." Scans the font engine and segments text. **Must be cached.**
- **`layout(prepared: PreparedText, width: number, config: PretextConfig): LayoutResult`**  
  The "Hot Path." Instant arithmetic-based wrapping.

### `PretextConfig` Interface
```typescript
interface PretextConfig {
  fontSize: number;      // e.g., 16
  fontFamily: string;    // e.g., 'Inter'
  fontWeight: string;    // e.g., '600'
  lineHeight: number;    // e.g., 1.4
  letterSpacing?: number;
  letterCase?: 'none' | 'uppercase' | 'lowercase';
  // ... any other standard canvas font properties
}
```

## 4. Engineering Best Practices

### The "Zero-Reflow" Loop
To maintain 120fps, never call `prepare()` inside a `requestAnimationFrame`. Use this pattern:
1. **Initialize**: Call `prepare()` once.
2. **Update**: In the render loop, call `layout()` with the new `width`.
3. **Guard**: Always wrap calls in `document.fonts.ready.then()` to ensure Canvas measurements match final CSS rendering.

### Caching Strategy
Always store the `PreparedText` object. If the text content hasn't changed, reuse the object even if the width changes. Only re-prepare if `fontSize` or `fontFamily` changes.

## 5. Viral Visual Recipes

### The "Dragon" Wrap (Distance-Aware)
1. Pre-calculate line bounding boxes via `layout()`.
2. Find the collision point with a moving object (e.g., an illuminated dragon sprite).
3. In the `Hot Path` (render loop), dynamically adjust the `width` or `x-offset` per line based on distance to the object.
4. Use GSAP for smooth transitions between layout states.

### The "Editorial" Reflow
1. Split long-form text into logical `prepare()` blocks.
2. Use `layout()` to find column heights and breakpoints *before* committing to the DOM.
3. Distribute blocks into a CSS Grid or Flexbox container, or render them directly to a multi-column Canvas.
4. Balances visual weight across columns at 120fps.