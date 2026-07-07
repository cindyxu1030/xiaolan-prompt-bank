# 字后穿字 · Text-Behind-Text

![preview](../assets/gifs/text-behind-text.gif)

**效果:** 一个词从另一个巨字的笔画后面滑过 — 两层文字自己形成前后景，不需要任何人物抠像，纯排版做出纵深。
*What it delivers: one word slides behind the strokes of a giant front word — two type layers forming their own foreground/background. Depth from pure typography, no subject matte needed.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 5-second text-behind-
text caption on {BG — flat #101114, paper, or footage}.

FRONT word (the anchor, solid): {FRONT e.g. "规则"} — huge (300-420px),
heavy black weight, color {FRONT_COLOR e.g. white}, centered, may bleed edges.
BACK word (slides behind it): {BACK e.g. "打破"} — ~60% of front's size,
color {ACCENT — the accent goes on the BACK word}, opacity ~0.9.

Build the two z-planes (no matte needed — pure stacking):
- The trick: each front GLYPH must occlude the back word. Duplicate the front
  word into two stacked copies: (a) .front-base at z-index 1, (b) .front-cover
  at z-index 3 — identical text, identical position. The back word sits at
  z-index 2, BETWEEN them. Result: the back word passes behind solid glyphs.
  (With variable-alpha bgs keep both front copies fully opaque and identical
  so they layer invisibly.)
- Give the back word a slight blur (1.5px) and 0.9 opacity — it lives one
  depth plane back, the lens says so.

Animation timeline (~5s):
- 0.3s   FRONT word slams in (scale 1.14 → 1.0, power4.out, 5 frames,
         opacity 0→1) and holds with micro-breathe ≤1.02.
- 1.0-3.6s THE PASS: back word slides horizontally behind the front — enters
         from off-left at x -700, glides to x +700 (linear or slow
         sine.inOut), disappearing and re-emerging between the front glyphs'
         strokes as it travels. Add a slower parallax: the front word drifts
         x 0 → -20 during the pass (opposite direction, 3% of the back
         word's speed) — two planes, two speeds.
- 2.3s   as the back word crosses dead center (fully hidden behind the
         densest glyph), the front word pulses once (scale 1.0 → 1.035 → 1.0,
         0.3s) — it "feels" the pass.
- 3.6-5s hold: back word parked at ~+420 (partially visible, tucked behind
         the last glyph), both keep their drift; exit = both blur out
         together (0.25s).

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; finite repeats; root
div with data-composition-id="main" data-duration="5" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **The sandwich is three layers of TWO words:** front-base / back / front-cover. The duplicated front copy is what makes glyph strokes occlude — no mask, no matte, works on any background.
- Two planes need two speeds: the counter-drift on the front word (3% of the back word's speed, opposite direction) is what sells parallax; without it the pass reads as a marquee.
- Blur 1.5px + opacity 0.9 on the back word — the lens must agree it's farther away.
- Pick word pairs where the meaning IS the occlusion: 打破/规则, behind/ahead, 看不见/努力.
