# 手写×粗黑海报叠字 · Script-Poster Stack

![preview](../assets/gifs/script-poster-stack.gif)

**效果:** 粗黑大字排成海报骨架，一个手写词斜着叠上去当灵魂 — 手写笔画像真人写字一样描画出来。理性信息 + 感性签名，一帧变海报。
*What it delivers: heavy sans lines stack into a poster skeleton, then one script word lays diagonally across as the soul — its strokes drawing on like live handwriting. Rational message + emotional signature; the frame becomes a poster.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a script-over-sans poster
lockup, 5 seconds, on {BG — flat paper #F4F1EA / dark #101114 / or footage}.

The sans stack (2–3 short lines): {LINES e.g. "DON'T / COPY / TOOLS"}.
The script word (the emotional counterpoint): {SCRIPT_WORD e.g. "create"}.
Ink: {INK}. Script accent: {ACCENT — one color, e.g. #CE1214}.

Typography:
- Sans stack: black-weight grotesque, UPPERCASE, 140–200px, line-height
  0.85–0.95, stacked tight like a poster masthead. Left-aligned or centered;
  the block may bleed one edge.
- Script word: a genuine script font (e.g. Caveat 600 / a brush face), 1.5–2x
  the sans cap-height, laid DIAGONALLY (rotate -6° to -10°) across the stack,
  overlapping at least two sans lines, in ACCENT.

Animation timeline (~5s):
- 0.0–1.0s  the sans lines slam in one by one (0.22s apart), each with the
            hard punch: scale 1.14→1.0 power4.out, opacity 0→1, y 16→0,
            ~5 frames each. On each landing the previous lines get a 3px
            knock and settle.
- 1.4–2.6s  THE SIGNATURE: the script word HANDWRITES itself — convert the
            word to SVG stroke paths (one path per pen stroke) and draw them
            in writing order via strokeDasharray/dashoffset, each stroke
            slightly overlapping the previous one's end time (natural pen
            rhythm, ~1.2s total, power1.inOut per stroke). A subtle drop
            shadow under the script lifts it off the sans layer.
- 2.8s      lockup beat: the whole poster does one unified micro-punch
            (scale 1.02→1.0) + the script word's underline flick (a short
            swash stroke draws under it, 0.2s).
- 3.0–5.0s  hold: script breathes rotation -8°→-7.2° (sine yoyo, slow); sans
            stack dead still — the contrast between still structure and
            living signature IS the style.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; handwriting via getTotalLength() dash draw
(deterministic); no Date.now / Math.random; root div with
data-composition-id="main" data-duration="5" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **The style is a contrast pair:** mechanical sans slabs + one living script stroke. If both move the same way, it's just two fonts.
- Real handwriting = SVG stroke paths drawn in pen order with overlapping timing — a left-to-right mask wipe over script text always reads as a wipe, never as writing.
- The script goes diagonal and overlaps the stack — parallel and separated reads as two unrelated captions.
- Structure holds still on the hold; only the signature breathes.
