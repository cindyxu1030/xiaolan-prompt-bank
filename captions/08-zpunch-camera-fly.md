# 冲脸巨字 · Z-Punch Camera Fly

![preview](../assets/gifs/zpunch-camera-fly.gif)

**效果:** 单词从画面深处直接飞到你脸上 — 从模糊的远点沿 Z 轴冲出，一帧带风。全片只给最大的那一个词用。
*What it delivers: the word flies from deep in the frame straight AT your face — blurred at the far point, punching out along the z-axis. Reserved for the single biggest word of the video.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 4-second z-punch
caption demo on {BG — dark footage or flat #0B0B0D}.

Setup line (small, appears first): {SETUP e.g. "最重要的一件事"}.
THE word (flies at the camera): {HERO e.g. "执行"}. Color: {ONE_ACCENT or
white}. Heavy black-weight type; CJK = heavy 黑体.

Build the z-space:
- The stage (or a wrapper div) gets perspective: 1000px so translateZ reads
  as true depth.
- Hero word ~200-280px at rest scale, centered, may bleed edges at arrival.

Animation timeline (~4s):
- 0.2s   setup line pops in small (pop: {opacity 0, scale .86, y 20} → 0.24s
         back.out(2)), upper third.
- 1.0s   THE Z-PUNCH: hero enters {opacity 0, z: -700, blur 14px} →
         {opacity 1, z: 0, blur 0}, 0.35s power4.out. It should read as
         accelerating out of the depth — tiny and soft, then huge and sharp
         in your face. The last 3 frames overshoot to z +40 and settle back
         to 0 (the "almost hit you" kiss).
- 1.1s   impact: setup line gets blown 10px upward and dims to 60%; one
         2-frame radial shock hint (a subtle expanding vignette ring or
         1-frame frame-shake y ±5px).
- 1.4-4s hold: hero keeps the depth alive — z drifts 0 → +25 slowly
         (sine yoyo) + micro-breathe scale ≤1.03. Never static.
- 3.6s   exit: hero blasts PAST the camera — z 0 → +600, blur 0 → 18,
         opacity → 0 in 0.2s power2.in (it flies over your shoulder, not
         a fade).

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; finite repeats; root
div with data-composition-id="main" data-duration="4" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Real z, not scale.** `perspective: 1000px` + `translateZ` gives parallax-correct acceleration that a scale tween can't fake — the word genuinely travels through the lens space.
- Blur maps to depth: 14px at z −700, 0 at arrival, 18px flying past. Blur IS the depth cue.
- Exit through the camera (z → +600), never a fade — the word entered from the world, it should leave past your ear.
- One z-punch per video. It's the biggest gun in the kinetic vocabulary; firing it twice makes both shots smaller.
