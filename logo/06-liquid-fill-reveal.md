# 液体灌注 Logo · Liquid Fill Reveal

![preview](../assets/gifs/liquid-fill-reveal.gif)

**效果:** 空心 logo 轮廓里，液体从底部涨上来把它灌满，表面还有起伏的波浪 — 灌满的那一刻整体点亮。
*What it delivers: liquid rises from the bottom inside a hollow logo outline, filling it with a wavy moving surface — the whole mark lights up the instant it tops off.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 5-second liquid-fill logo
reveal on {BG — flat #0A0C10}.

My logo: {YOUR_LOGO — best as a bold solid SHAPE or heavy wordmark; give the
outline}. Liquid color: {ACCENT e.g. #2F6BFF} (or a top-to-bottom gradient).

Build the fill:
1. Two layers of the logo: (a) a hollow OUTLINE version (stroke only, ACCENT
   at ~35% — the empty vessel), and (b) a SOLID fill version used as a CSS
   mask / clip so the liquid only shows INSIDE the logo shape
   (-webkit-mask: url(logo-solid.svg) or a clip-path of the shape).
2. Inside that mask, a "liquid" div: a solid ACCENT rectangle taller than the
   logo, its TOP edge is an SVG wave path (two stacked sine humps). Two wave
   layers at slightly different phase/opacity give parallax to the surface.
3. Rising = translate the liquid div upward; the wave crest scrolls sideways
   (translateX loop) so the surface undulates as it climbs.

Animation timeline (~5s):
- 0.0-0.4s  hollow outline draws/fades in (the empty glass).
- 0.4-3.2s  liquid rises: translateY from below the logo to fully above it
            (ease power1.inOut — a touch of slosh: overshoot 4% then settle).
            The wave surface scrolls the whole time; a few rising bubble dots
            (small circles, authored paths) drift up inside.
- 3.2s      FILLED: the moment liquid tops the shape, the solid logo flashes
            to full brightness (outline → solid), one specular sweep crosses,
            and a subtle surface ripple radiates from center.
- 3.4-5s    hold: the filled surface keeps a gentle sway (wave amplitude
            breathing, finite repeats); logo glows softly.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; wave scroll + bubbles authored (no Math.random —
index/phase driven); no Date.now; finite repeats; root div with
data-composition-id="main" data-duration="5" data-width="1920"
data-height="1080".
```

## 要点 Key technique notes

- **The logo shape is a mask over the liquid**, so the rising fill is automatically clipped to the mark — no per-letter masking needed.
- The moving surface is one SVG wave path translated up while its crest scrolls sideways; two phase-offset wave layers add parallax and sell "liquid," not "progress bar."
- A small slosh overshoot (4% then settle) when it tops off is the difference between water and a loading fill.
- Bubbles and wave scroll must be index/phase-driven, not random, to stay render-deterministic.
