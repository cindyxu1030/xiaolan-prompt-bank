# 弧线滚落字 · Cascade Path Text

![preview](../assets/gifs/cascade-path-text.gif)

**效果:** 整句话拆成词，沿一条弧线绕着人物"滚落"下来 — 每个词都贴着弧线的切线角度，一个接一个落位，像句子顺着光束流下来。
*What it delivers: a sentence breaks into words that tumble down a curved path around the subject — each word rotated to the path's tangent, landing one after another, like the line flowing down a light beam.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a "cascade path"
caption layer over {BACKDROP — your footage clip, or the built demo
scene: charcoal #16181C stage, one soft spotlight beam from top-right
(a rotated blurred white wedge at 10%), and a simple standing-figure
silhouette (CSS/SVG, matte black #0A0B0D with a soft edge) lower-left,
~55% frame height}. Duration ~6s.

The sentence (5–7 words): {WORDS, e.g. "every / cut / lands / on / the
/ beat"}. Hero word: {HERO, e.g. "beat"} in {ACCENT — one color, e.g.
#FFD84A}; all other words white. Heavy black-weight lowercase.

Build the path + words:
- Define ONE smooth bezier arc that enters near the top-right (beside
  the spotlight source), curves around the figure's right side, and
  runs out toward bottom-center — through the scene's negative space,
  never crossing the face.
- Distribute the words along the path by arc length (even VISUAL
  spacing — size-weighted by each word's footprint so big words don't
  collide; slight size decay 150px→90px down the path; the HERO breaks
  the decay: biggest of all, ~180px, at the arc's final position).
- EACH word is rotated to the path's local tangent angle at its slot
  (getPointAtLength on a hidden SVG path — measure angle from
  neighboring points), CLAMPED to ±35° — the tangent gives the lean,
  but every word must stay instantly readable; never let the path
  rotate a word toward vertical.
- Shape the arc so its lower end flattens back out: the HERO lands
  near bottom-center at ≤15° tilt, fully inside the frame — the money
  word is the most readable thing on screen, not the most rotated.

Animation timeline (~6s):
- 0.0–0.6s  backdrop establishes (spotlight blooms, silhouette fades
            up if using the demo scene).
- 0.8s+     words tumble in DOWN the path in order, 180ms apart: each
            enters from slightly UP-path (walk it backward ~40px along
            the arc) and slides to its slot while rotating from
            tangent+14° to its final tangent angle, opacity 0→1, ~0.4s
            power3.out with a tiny overshoot past the slot and back.
- HERO      lands last with a punch (scale 1.16→1, 5 frames,
            power4.out) and knocks the two nearest words 5px outward
            along their tangents (they settle back, sine.out).
- then      life: every word keeps a micro-drift ALONG the path (±4px
            arc-length slide, sine yoyo, desynced by index) — the
            sentence breathes as one stream; spotlight flickers 2%.
- last 0.8s words flow OUT continuing down the path (slide +60px along
            arc + fade, 80ms stagger, entry order) — the stream drains.

Render safety (required): one paused GSAP timeline on
window.__timelines["main"]; path sampling via getPointAtLength at build
time (deterministic); no Math.random / Date.now; finite repeats; root
div with data-composition-id="main" data-duration="6" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **切线角度是全部的戏，但要夹在 ±35° 以内** — 切线给的是"倾斜感"，不是"竖排"；词转到接近竖直就不可读了。hero 落点必须回平（≤15°）、完整在画面内。
- 9:16 竖幅的几何事实：从顶部下到底部的弧线大部分路段必然接近竖直，所以铺垫词会统一顶在 ±35° 的夹角上（一列同角度斜排）— 这是正确结果；切线的"变化感"体现在结尾放平的 hero 上，不在铺垫词之间。
- 入场从"弧线上游 40px"滑到位、出场继续"顺流而下" — 词的整个生命周期都在这条河里，不是飞进飞出。
- 尺寸沿路径衰减（150→90px）制造透视流感，hero 破例最大、落在弧线终点砸尾。
- 弧线要绕开脸、穿过画面的负空间 — 先读镜头再画线（demo 场景的光束就是弧线的"理由"）。
