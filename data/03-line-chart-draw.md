# 折线图描画 + 标注 · Line-Chart Draw with Annotations

![preview](../assets/gifs/line-chart-draw.gif)

**效果:** 趋势线从左到右一笔画出，线下渐变区域跟着填充，走到关键点位时暂停 — 标注卡弹出讲那个转折 — 增长故事的叙事镜头。
*What it delivers: the trend line draws itself left to right, the gradient area filling beneath it, pausing at key data points where annotation cards pop in to tell that turning point — the narrative shot for any growth story.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — an 8-second line-chart
story.

Chart title: {TITLE e.g. "粉丝增长的 90 天"}.
The line: {DESCRIBE THE SHAPE, e.g. "flat for a third, a dip, then a steep
climb to the top right"} — hand-author it as one smooth SVG path.
2 annotations: {POINT_1: WHAT_HAPPENED e.g. "第一条爆款视频"} /
{POINT_2: WHAT_HAPPENED}. End value: {END_VALUE e.g. "12.4k"}.
Palette: {BG} / {LINE_COLOR accent} / {INK}.

Layout:
- Title top; chart occupies the middle 55% inside comfortable margins.
- The line: one SVG path, 6px stroke, round caps, in LINE_COLOR.
- Beneath it a gradient area fill (same path closed to the baseline, filled
  with a top-to-transparent gradient of LINE_COLOR at ~18%): reveal it with a
  clip-path rectangle that follows the draw edge.
- A leading dot rides the line tip while drawing (place it along the path via
  the same progress value driving the dashoffset).
- Each annotation = anchor dot on the line + a small card (accent hairline,
  20px radius, label 30px bold + one 24px sub-line) connected by a short stem.
- Faint gridlines ≤8% opacity; end-value tag at the line's final point.

Animation timeline (~8s):
- 0.0–0.6s  title + axes hairlines settle in.
- 0.8s      draw phase 1: strokeDashoffset animates the line from start to
            annotation-1's point (power1.inOut) with the leading dot riding
            the tip and the area-fill clip following in lockstep.
- ~2.2s     PAUSE 1: the dot pulses (swell + expanding ring — on SVG circles
            animate the radius attr, not scale, to avoid the GSAP-SVG
            transform-origin displacement trap), annotation card 1 pops in
            (y 14→0, scale .9→1, back.out(1.7)).
- 3.4s      draw phase 2: line continues to annotation-2's point.
- ~4.6s     PAUSE 2: same grammar, annotation card 2.
- 5.4s      finale: line races to the end (power2.in — acceleration reads as
            momentum), the end-value tag punches in at the tip (1.15→1) with
            a one-time glow flash on the whole line.
- 6.2–8s    hold: leading-dot glow breathes; cards still; slow 1.5% push-in.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; the draw is strokeDasharray/offset from
getTotalLength() (deterministic), segmented by tweening the SAME dashoffset to
intermediate values on the timeline; no Date.now / Math.random; root div with
data-composition-id="main" data-duration="8" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Draw-pause-annotate-draw** is the whole trick: the pauses turn a chart into a *story with chapters*. A line that draws straight through says nothing.
- Segment the draw by tweening one `strokeDashoffset` to intermediate values — same mechanism, chapter timing.
- The final segment uses `power2.in` (accelerating) — ending fast reads as momentum, perfect for growth stories.
- Author the path by hand for drama. Real data can come later; the *shape* is the message.
