---
name: pretext-layout-engine
description: The ultimate reference for AI agents to master @chenglou/pretext. Enables zero-reflow, pixel-perfect text layout at 120fps across all languages and mixed scripts.
author: RAHUL U
version: 1.0.0
tags: ["layout", "typography", "performance", "ui", "typescript"]
---

# Pretext Layout Engine – Universal AI Agent Skill Reference
> **The official guide for AI-driven 120fps typography and zero-reflow UIs.**

**Pretext** (`@chenglou/pretext`) v0.0.3 is a tiny (~15 KB), zero-dependency, pure TypeScript/JavaScript library that solves the **15-year web text-layout problem** once and for all.

It uses the browser’s own font engine (via Canvas) for **pixel-perfect accuracy** across **every language, emoji, CJK, RTL, mixed scripts, and browser quirks** — then does **all layout with pure arithmetic**.  
No more `getBoundingClientRect`, `offsetHeight`, `scrollHeight`, or any DOM measurement that causes reflows.

`prepare()` = one-time work.  
`layout()` = blazing-fast hot-path math.  

Result: **120 fps UIs, perfect virtualization, zero layout thrashing**, and layouts that CSS still can’t do reliably.

This `SKILLS.md` file is the **single authoritative source of truth** for your entire open-source project.  
Any AI coding agent (Claude, Cursor, Grok, Windsurf, etc.) or human contributor can now build **any use case** — even if you give the agent almost zero extra instructions.

Just say:  
> “Follow SKILLS.md strictly and use Pretext for all text measurement and layout. Assets: [your code/text/fonts]. Task: [your request]”

The agent will automatically apply best practices, caching, correct APIs, and deliver production-grade results.

## 1. Why Pretext Matters

- Perfect for **any** text-heavy UI: chat bubbles, virtualized lists, masonry grids, accordions, dynamic cards, text flow around images/SVGs, canvas/SVG rendering, editorial layouts, etc.
- Works with **any** framework or rendering strategy.
- Unlocks buttery-smooth experiences that were previously painful or impossible with pure CSS/DOM.

## 2. Installation (Copy-Paste)

```bash
# Install with skill.sh
npx skills add pretext-layout-engine

# Or install the library manually
npm install @chenglou/pretext
```

## 3. Core Concepts for Agents

### `prepare(text, config)`
Performs the one-time, heavy-lifting work (Canvas measurement). This should be cached.

### `layout(prepared, width, config)`
Calculates line breaks, wrapping, and overflow in microseconds without touching the DOM.

## 4. Viral Visual Recipes

### The "Dragon" Wrap
1. Use `layout()` to find exact bounding boxes of each line.
2. In the render loop, adjust line `x` or `width` based on the proximity of the animated element (the dragon).
3. Use GSAP for smooth transitions between layout states.

### The "Editorial" Reflow
1. Split text into blocks.
2. Calculate column heights using `layout()` before rendering.
3. Distribute blocks across columns to balance visual weight.