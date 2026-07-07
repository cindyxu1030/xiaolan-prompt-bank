# 图标阵列占比 · Pictograph Isotype Grid

![preview](../assets/gifs/pictograph-isotype.gif)

**效果:** 一排排小图标依次点亮，用"10 个里有 7 个"这种方式讲比例 — 比百分比更好懂、更好记的数据表达。
*What it delivers: rows of little icons light up in sequence to tell a proportion as "7 in 10" — a share that's easier to grasp and remember than a bare percentage.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 6-second pictograph
(isotype) chart on {BG — flat #F4F1EA or dark}.

Headline: {STATEMENT e.g. "每 10 个创作者，有 7 个卡在剪辑"}. Ratio:
{FILLED} of {TOTAL} (e.g. 7 of 10, or 70 of 100 for a 10x10 grid). Icon:
{ICON — a simple SVG glyph matching the topic: person, phone, star, drop…}.
Filled color: {ACCENT}. Empty color: muted {INK} at ~18%.

Build the grid:
- A grid of {TOTAL} identical SVG icons (e.g. 10 in a row, or a 10x10 block
  for percentages). All start in the "empty" muted color.
- The FILLED icons (first N in reading order) will animate to ACCENT.
- Big headline above; a large "{FILLED}/{TOTAL}" or "{PCT}%" readout that
  counts up alongside.

Animation timeline (~6s):
- 0.0-0.8s  headline reveals per-word; the full empty grid fades in.
- 1.0-3.4s  the filled icons light up ONE BY ONE in reading order (0.12s
            apart): each does color muted→ACCENT + a scale 1→1.25→1 pop
            (back.out) + a tiny glow flash. The readout counts up as they
            fill (7 as 7 icons land, or the % ticking).
- 3.4s      completion beat: the filled block pulses together once; a
            bracket/underline sweeps under the filled group to visually
            separate "the 7" from "the 3"; the empty ones dim further.
- 3.6-6s    hold: filled icons keep a gentle staggered idle bob (finite
            repeats); headline steady; optional one-line takeaway fades in.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; per-icon delays from index (no Math.random);
count-up tweens a plain object; no Date.now; finite repeats; root div with
data-composition-id="main" data-duration="6" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Fill in reading order, one at a time.** The sequential light-up IS the comprehension — the viewer counts along with it. A grid that fills all at once is just a colored block.
- Match the icon to the topic (people for audience, phones for users, drops for retention) — the glyph carries half the meaning.
- The bracket/underline separating "the filled" from "the empty" at the end nails the takeaway ("7 of them, not 3").
- 10 icons for simple ratios, 10×10 for percentages; per-icon delays from index keep it deterministic.
