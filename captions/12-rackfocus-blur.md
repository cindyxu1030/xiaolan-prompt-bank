# 焦点拉字 · Rack-Focus Blur Type

![preview](../assets/gifs/rackfocus-blur.gif)

**效果:** 文字和人物在同一个镜头里抢焦点 — 先字实人虚，"拉焦"，变成人实字虚。文字瞬间变成房间里真实存在的一层。
*What it delivers: the text and the subject fight for focus in one lens — text sharp / subject soft, then the focus racks: subject sharp / text bokeh. The type instantly becomes a physical plane in the room.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a rack-focus caption
layer over a clip ({CLIP — a moody / silhouette / backlit shot sells the
lens-feel best}).

The line: {TEXT e.g. "先看清自己"} — one statement, 110-160px, heavy weight,
white (or {ACCENT}). Optional small second line at 40px.

Build the two focus planes:
- The TEXT layer and the FOOTAGE are the two planes. The footage keeps its
  natural look; the "rack" is simulated on both sides:
  - text blur: filter: blur() animated 0 ↔ 12px
  - subject softness: a duplicated footage layer with blur(6px) whose
    opacity crossfades 0 ↔ 1 over the original (cheap, render-safe fake
    of defocus — never blur the base layer directly, stack the soft twin).
- Planes never blur simultaneously: one is always sharp — that's what makes
  it read as ONE lens with shallow depth of field, not a soggy filter.

Animation timeline (~5s or clip length):
- 0.0-0.6s  text fades in ALREADY SOFT (blur 10, opacity 0→0.85) while the
            subject is sharp — a bokeh ghost of a sentence you can't read
            yet. Curiosity beat.
- 1.2s      RACK 1: over 0.7s (power2.inOut), text blur 10 → 0 & opacity
            → 1 as the soft-twin footage fades IN (subject goes soft).
            Now you read the line; the person is a blur behind it.
- 2.8s      hold the text-sharp plane with micro-breathe ≤1.02.
- 3.4s      RACK 2 (back): text blur 0 → 12, opacity → 0.7; subject snaps
            sharp again over 0.6s — the room takes the frame back.
- 4.2-5s    text fades out fully while soft; subject sharp at the end.
- The text also drifts y -10 across its whole life (one slow linear tween)
  — a plane hanging in air, not a sticker.

Render safety (required): footage layers timeline-driven (no autoplay);
one paused GSAP timeline on window.__timelines["main"]; no Date.now /
Math.random; root div with data-composition-id="main"
data-duration="{CLIP_LENGTH}" data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **One plane sharp at all times.** The rack is a crossfade of focus, never a both-blurry moment — two soft planes reads as a broken render, one soft plane reads as a lens.
- Don't blur the base footage — stack a pre-blurred twin and crossfade its opacity. Animating blur on video layers is expensive and shimmers; opacity is free and stable.
- Enter soft, resolve sharp: showing the unreadable bokeh FIRST buys a curiosity beat that a plain fade-in never gets.
- Best over silhouette/backlit footage where the subject is a clean shape — the focus story stays legible even in a 360px GIF.
- If the clean zone in your footage is a narrow band, set CJK vertically (upright column) instead of shrinking a horizontal line until it's illegible — and give white type a soft dark text-shadow where it crosses bright glow.
