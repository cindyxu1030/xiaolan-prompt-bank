# 巨字重锤 · Hero Slam Caption

![preview](../assets/gifs/hero-slam.gif)

**效果:** 说到关键词的那一帧，一个巨字硬砸进画面 — 小字铺垫 + 巨字主角 + 唯一重音色，出血裁切，落地后微呼吸。口播视频的"金句时刻"。
*What it delivers: on the exact frame the key word is spoken, a giant word slams into frame — small setup word + massive hero + a single accent color, full-bleed crop, micro-breathing after landing. The "quotable moment" treatment for talking-head video.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a caption overlay layer
for a talking-head clip (transparent background if compositing later, or place
over the provided footage).

The spoken line: "{LINE e.g. 别再囤工具了，去搭系统}".
The HERO word: {HERO — the ONE word that must be remembered, e.g. 系统}.
Its VO timestamp: {T_HERO — the second the word is spoken}.
Setup words: the rest of the line, small. Accent: {ONE_ACCENT e.g. #CE1214}.
Base text color: {WHITE on footage / dark on light bg}.

Typography rules (non-negotiable):
- Heavy black-weight grotesque (Inter Black or similar). CJK: a heavy 黑体.
- Scale contrast 2.5–4x: setup words 52–84px, HERO 200–320px.
- The hero word bleeds off at least one frame edge — containment is the #1
  "template" tell.
- Scatter placement: setup words live in open space around the subject
  (upper-left / mid-right), NOT in a lower-third band. Never cover the face.
- Two colors total: base + the accent ON the hero word only.

Animation timeline:
- T_HERO - 0.5s  setup words punch in small, each on its own beat (opacity
                 0→1, y 18→0, scale 1.08→1, ~0.14s each, power3.out).
- T_HERO         THE SLAM: hero word punches in — scale 1.16→1.0 power4.out
                 within 4–6 frames, opacity 0→1, blur 8px→0. The impact must
                 live inside ~0.18s, timed EXACTLY on the spoken word.
- T_HERO + 0.1s  impact echo: setup words get knocked 5px away and settle;
                 optional 1-frame frame-shake (whole group y ±4px, 2 steps).
- afterwards     life on the hold: hero breathes scale 1.0→1.045 (sine yoyo,
                 ~1.6s) and drifts y 0→-10 slowly. Never static.
- exit           hero blurs out fast (blur 0→10, opacity→0, scale→1.08, 6–8
                 frames) right before the next line starts.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; root div with
data-composition-id="main" data-duration="{CLIP_LENGTH}" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Timed to the voice, or it's wallpaper.** The slam lands on the word's exact VO timestamp — get word-level timestamps first (whisper transcription), then animate to them.
- 4–6 frames of entry. `power4.out` from 1.16. Slower = fade, and a fade is not a slam.
- One hero per line, one accent per video. The moment everything is big, nothing is.
- Face is sacred: words may crop off frame edges, never off the speaker's face.
