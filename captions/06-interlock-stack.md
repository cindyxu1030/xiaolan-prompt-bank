# 咬合字块 · Interlock Stack

![preview](../assets/gifs/interlock-stack.gif)

**效果:** 大小不一的小写单词像填字游戏一样咬合成一个整体字块，按阅读顺序逐词弹入，落定后整块一起呼吸 — 一句话变成一个 logo。
*What it delivers: lowercase words of mixed sizes nest together like a crossword into one tight block, popping in reading order, then breathing as a single mark — a sentence becomes a logo.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 5-second interlocked
type-stack caption on {BG — dark footage or flat #101114}.

The line (3-5 short words/phrases, one per row): {LINES e.g. "so / how do you /
own / your voice?"}. Hero word(s) to color: {HERO + ACCENT_COLOR — exactly one
accent}. Base color: white (or dark ink on light bg).

Build the interlock:
- One centered block; each row is its own element with its OWN font-size,
  chosen so the row WIDTHS nest — short word rows render huge, long phrase
  rows render small, and every row's width lands within ~10% of its neighbors
  (e.g. row widths 740/730/760/750px from sizes like 210/64/230/58px).
  Measure and iterate until the block reads as one rectangle.
- Interlock the baselines: negative margin-top between rows (-0.13 to -0.15em
  of the LARGER neighbor) so ascenders/descenders tuck into each other's
  negative space. Tight = the whole point; if nothing overlaps it's just a
  list.
- Heavy black-weight lowercase grotesque. Line-height 1. Letter-spacing 0.
- The hero word(s) wear the accent inside the block; everything else base.

Animation timeline (~5s):
- Words pop in READING ORDER (top row first), each on its own beat 0.14-0.2s
  apart: {opacity 0, scale .86, y 20} → {1, 1, 0}, 0.24s back.out(2.0). If
  synced to VO, use each word's spoken timestamp instead of even spacing.
- When the last row lands, the BLOCK does one unified settle punch
  (scale 1.03 → 1.0, power4.out, 0.15s) — the moment it fuses into a mark.
- Hold: the whole block breathes as ONE unit (scale 1.0 → 1.03, sine yoyo,
  ~1.8s period, finite repeats) + drifts y 0 → -8 across the hold. Rows never
  move independently after fusing.
- Exit: the block blurs+fades out as one (blur 0→8, opacity→0, 0.2s).

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; finite repeat counts;
root div with data-composition-id="main" data-duration="5" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Row widths must nest.** The craft is in the font-size arithmetic: pick sizes so every row's rendered width lands within ~10% of the block width. Measure, don't guess — cap glyphs run ≈0.71× font-size per character in heavy grotesques.
- Negative margin-top (−0.13 to −0.15em) is what turns rows into an interlock; without it you have a boring stack.
- After the fuse punch, the block moves only as a unit — independent row motion breaks the "one mark" illusion.
- This register won a 3-way pick against depth-pyramid and highlighter treatments for a closing beat — it's strongest as a video's final statement.
