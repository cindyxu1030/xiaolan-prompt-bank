# 描边发光 Logo 亮起 · Stroke-Glow Logo Reveal

![preview](../assets/gifs/stroke-glow-reveal.gif)

**效果:** logo 线条像光笔一样一笔描画点亮，一颗白色光斑沿线奔跑，光晕呼吸，字标带渐变升起，底部有玻璃倒影。
*What it delivers: the mark draws on like a light-pen stroke, a white spark runs along the leading edge, the glow blooms and breathes, and the gradient wordmark rises in over a glossy floor reflection.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 6-second animated logo reveal on a
near-black background (#0a0607).

My logo: {YOUR_LOGO — describe the mark's shape, or attach the PNG}.
Brand gradient: {COLOR_1} → {COLOR_2} (e.g. #EC2D7C → #FB7A3E).

Build technique (follow exactly):
1. Recreate the logo mark as ONE continuous SVG centerline path (a single stroked
   path tracing the skeleton of the mark — NOT a filled outline). Round linecaps.
   Stack 4 copies of the same path in one <svg viewBox> matching the logo's native
   coordinate system:
   - .core     stroke: brand gradient (userSpaceOnUse linearGradient), width ~20
   - .glow     same gradient, width ~24, filter: blur(6px), opacity .9
   - .glow-far same gradient, width ~30, filter: blur(16px), starts opacity 0
   - .spark    stroke #fff, width ~21, filter: blur(2.5px), starts opacity 0
2. Draw-on: set strokeDasharray = strokeDashoffset = path.getTotalLength() on
   core/glow/glow-far, then tween strokeDashoffset → 0 over ~2.25s, power1.inOut.
   The spark uses a short dash (~18px) tweened along the same path so a bright
   head travels at the leading edge of the draw; fade it out as the draw completes.
3. Wordmark: the brand name as an alpha mask (mask: url(wordmark.png)) on a div
   filled with the brand gradient, plus drop-shadow glows. It fades + rises in
   (y 26 → 0, blur 6 → 0, power3.out) right as the line finishes.
4. Ambience: a blurred radial light pool behind the logo rises with the draw
   (use a blurred element, NOT a full-bleed CSS gradient — avoids H.264 banding),
   plus a fine grain overlay at ~5% opacity, mix-blend-mode overlay.
5. Finale: glow-far pulses up then settles; the logo scales 1.18 → 1.205 → 1.18
   softly; a second longer light-lap (~70px dash) travels the finished line at
   ~3.6s; gentle yoyo breathing on the glows for the hold.
6. A -webkit-box-reflect: below on the logo container gives the floor reflection
   (fade it with a to-bottom black gradient).

Render safety (required): one single paused GSAP timeline registered on
window.__timelines["main"]; all motion tweened ON the timeline; no Date.now, no
Math.random; root div has data-composition-id="main" data-duration="6"
data-width="1920" data-height="1080". Stroke length from getTotalLength() is
deterministic and fine.
```

## 要点 Key technique notes

- **Centerline, not outline.** The magic is tracing the mark's *skeleton* as one stroked path — a filled outline can't draw-on. If your logo has separate strokes, one path per stroke, staggered.
- The traveling spark is the same path with a short `strokeDasharray` head — no extra geometry.
- Local blurred elements for ambience instead of full-bleed CSS radial gradients: full-bleed gradients band badly in H.264.
- Recolor a flat wordmark PNG by using it as a CSS **mask** over a gradient div — no need to retype the glyphs.
