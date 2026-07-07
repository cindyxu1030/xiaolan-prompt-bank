# 时间线里程碑 · Timeline Milestones Reveal

![preview](../assets/gifs/timeline-milestones.gif)

**效果:** 一条主线从上往下画出来，节点一个个亮起、里程碑卡片依次弹入，走到高光节点时放大强调 — 讲历程、复盘、增长故事的叙事镜头。
*What it delivers: a spine line draws down the frame, nodes lighting up and milestone cards popping in one by one, the highlight node scaling up — the narrative shot for a journey, recap, or growth story.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 7-second vertical
timeline on {BG — flat #F4F1EA or dark #101216}.

Title: {TITLE e.g. "从 0 到 10 万的 90 天"}. Milestones (4-6, date + event +
optional metric): {DATE — EVENT — METRIC, e.g. "Day 1 — 第一条视频 — 播放 2k"
/ "Day 18 — 第一条爆款 — 播放 50w" / "Day 45 — 开始日更" / "Day 90 — 破 10 万"}.
Accent: {ACCENT}. Highlight the biggest milestone.

Build the timeline:
- A vertical SPINE line down the center (or left), drawn via strokeDashoffset.
- A NODE (dot) at each milestone's position on the spine.
- Alternating milestone CARDS left/right of the spine (or all on one side),
  each: date chip + event title + optional metric, connected to its node by
  a short stem.

Animation timeline (~7s):
- 0.0-0.6s  title settles; the spine starts drawing.
- the spine DRAWS downward continuously (strokeDashoffset), and as its
  leading edge PASSES each node's position:
    → the node pops (scale 0→1.3→1, back.out) + a ring pulse,
    → its card wipes in from its side (clip-path inset + y rise),
    → its metric counts up.
  Space the milestones down the 7s so each reveals as the line reaches it
  (draw-pause-reveal rhythm, like chapters).
- at the HIGHLIGHT milestone: the node is bigger, the card scales up a touch,
  ACCENT glow, a brief hold before the line continues — the peak beat.
- final node (the payoff): line reaches the bottom, the last card punches in
  with its big metric + a burst; the whole spine flashes once.
- last 1s hold: nodes keep a gentle staggered idle glow; title steady.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; spine via strokeDashoffset; node/card reveals keyed
to the line's progress; count-ups on plain objects; no Date.now / Math.random;
finite repeats; root div with data-composition-id="main" data-duration="7"
data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **The spine draws, and reveals fire when its leading edge reaches each node** — draw-pause-reveal, chapter by chapter. A timeline where everything appears at once has no journey.
- Alternate cards left/right of the spine for rhythm and to fit more text; keep each card to date + event + one metric — a timeline is a skeleton, not an essay.
- Give the highlight milestone a real beat (bigger node, brief hold, glow) — the peak of the story deserves a pause the others don't get.
- Key the node/card reveals to the line's progress value (not fixed times) so they always fire exactly as the spine arrives, even if you retime the draw.
