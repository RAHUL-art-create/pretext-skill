---
name: pretext-layout-engine
description: The ultimate reference for AI agents to master @chenglou/pretext. Enables zero-reflow, pixel-perfect text layout at 120fps across all languages and mixed scripts.
author: RAHUL-art-create
version: 1.1.0
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

### The "Lyric Video" Sync (Audio-Reactive Text)
1. Prepare the full song lyrics once with `prepare()`.
2. In the hot path (requestAnimationFrame + audio.currentTime), call `layout()` with dynamic width that reacts to beat/energy (use Web Audio API analyser node).
3. Render each line to Canvas/WebGL with per-word color/scale/position offsets driven by real-time FFT data.
4. Bonus: ElevenLabs voice + Pretext = perfectly timed lyric videos at 120 fps.

### The "Full Document" Layout (Bitcoin Whitepaper Style)
1. Split the entire document into semantic `PreparedText` blocks (title, paragraphs, pull-quotes, code).
2. Use `layout()` to compute exact heights before any rendering.
3. Build a scroll-driven helix/3D layout: each block’s y-position and rotation is pure math based on scroll progress + its own LayoutResult.
4. Add Satoshi-style pull-quotes that shrinkwrap and animate on scroll. Works in DOM, Canvas, or Three.js.

### The "Live2D / Complex Animation" Integration
1. Prepare all UI text (dialogue bubbles, menus, captions) once.
2. In the Live2D render loop, pass the current model parameter (mouth open, eye direction) into a dynamic `width` for `layout()`.
3. Position the resulting line boxes so text perfectly follows the animated character’s mouth/hand without reflow.
4. Zero DOM thrashing -> buttery 60–120 fps Live2D + text.

### The "Three.js / WebGL Native" Recipe
1. Use OffscreenCanvas + Pretext (or main-thread Canvas) to get LayoutResult.
2. Convert each line’s glyph positions into Three.js TextGeometry or custom InstancedMesh (with correct kerning & line breaks).
3. All measurements stay in userland -> no more fighting DOM sync in 3D scenes.

### The "Infinite Masonry / 100k+ Virtualization" (Cheng’s flagship demo)
1. Prepare every card’s text content once (store `PreparedText` in a Map keyed by content hash).
2. In the virtualized render loop, call `layout()` only for items in the current viewport using the exact container width.
3. Return `LayoutResult` (line boxes + total height) to your virtualizer (e.g. TanStack Virtual or custom WebGL list).
4. Result: 100k+ variable-height items at locked 120 fps with perfect mixed-script support and zero reflow ever.

### The "Shrinkwrap Chat Bubble" (Dynamic width + tail)
1. Prepare each message once.
2. Call `layout(message, maxBubbleWidth, config)` -> get `LayoutResult`.
3. Use the returned `totalWidth` and `totalHeight` to size the bubble container (DOM or Canvas).
4. Position the chat tail at the bottom-right of the final bounding box. Re-layout instantly on window resize or message edit.

### The "Variable-Font ASCII / Generative Art"
1. Prepare the text with the base font (e.g. 'Space Grotesk').
2. In the hot path, pass a dynamic `fontVariationSettings` string (e.g. `'wght' 700 'wdth' 120`) into `PretextConfig`.
3. Re-layout with tiny changes -> instant real-time ASCII art, glitch effects, or weight-driven animations at 120 fps.

### The "Text Flow Around Assets" (Images / SVGs / 3D objects)
1. Prepare the full paragraph once.
2. Run `layout()` in two passes: first at full width to get line boxes.
3. For each line, detect overlap with floating assets (image/SVG coords) and call `layout()` again with reduced width for that line only.
4. Render text segments around the asset on Canvas or WebGL — perfect editorial “magazine” flow without CSS floats.

### The "React / Svelte / Vue Auto-Growing Textarea" (Framework-native)
1. Prepare user input on every `onChange` (debounced).
2. In the render cycle, call `layout(prepared, currentContainerWidth)` to get exact `totalHeight`.
3. Set `textarea.style.height = layoutResult.totalHeight + 'px'` (or pass height to virtual DOM).
4. Zero flash, zero reflow, works even inside virtualized lists or with mixed RTL + emojis.

### The "AI-Generated Content Renderer" (Real-time long-form)
1. As the LLM streams tokens, append them to the current text buffer.
2. Every N tokens (or on punctuation), re-prepare only the newest paragraph (incremental caching).
3. Use `layout()` in the hot path to compute final positions before committing to DOM/Canvas.
4. Enables buttery-smooth AI writing experiences, live justification, or infinite generative magazines.

---

**How to use any recipe**  
Just tell any AI agent:  
> “Follow SKILLS.md strictly (sections 1–5). Use Pretext for all text measurement and layout. Task: [masonry grid / shrinkwrap chat / variable-font ASCII / AI content renderer / etc.]”

## 6. Authoritative References

- **Official GitHub**: [chenglou/pretext](https://github.com/chenglou/pretext) (Source Code & Implementation)
- **Official Demos**: [chenglou.me/pretext](https://chenglou.me/pretext/) (Reference Showcases)
- **Community Experiments**: [somnai-dreams/pretext-demos](https://somnai-dreams.github.io/pretext-demos/) (Extra Recipes)