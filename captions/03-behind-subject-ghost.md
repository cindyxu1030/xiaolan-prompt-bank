# 人后穿字 · Behind-Subject Ghost Text

![preview](../assets/gifs/behind-subject-ghost.gif)

**效果:** 巨字从人身后穿过 — 身体挡住笔画，字在背后滑行，瞬间有了纵深，画面从"贴字"变成"空间"。
*What it delivers: a giant word passes behind the speaker — the body occludes the strokes as the text glides past, instantly creating depth. The frame stops being "text pasted on video" and becomes a space.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a behind-subject text
layer for a clip of a person.

Footage: {CLIP — a talking-head or standing subject video}.
The ghost word: {WORD — one big word, e.g. FOCUS / 专注}.
Text color: {COLOR}, at 85–100% opacity. Font: heavy black-weight grotesque.

Build technique (the sandwich):
1. Make the subject cutout: run background removal on the clip (e.g.
   `npx hyperframes remove-background <clip>` or rembg) to get a matching
   alpha-matte video of JUST the person.
2. Stack three layers, bottom to top:
   - the original footage (full frame),
   - the TEXT layer (the giant word),
   - the subject cutout video, frame-synced with the footage below it.
   The cutout hides any stroke that passes "behind" the person — that
   occlusion IS the effect.
3. The word is HUGE (240–400px), positioned so roughly a third of it sits
   behind the body. It may bleed off frame edges. It must never cover the
   face — the face always belongs to the top layer anyway, but plan the
   path so strokes don't crowd the head.

Animation timeline:
- 0.0–0.5s  the word fades in already partly occluded (opacity 0→0.9,
            from a slight blur 4→0) — born in the scene, not pasted on.
- 0.5–4.0s  the glide: the word drifts slowly across/behind the subject
            (x across ~15% of frame width, linear or sine.inOut) — slow
            enough to feel architectural.
- 1.8s      optional pulse: one letter or the whole word breathes scale
            1.0→1.03 (sine yoyo).
- 4.0–4.5s  exit: continues its drift while fading (opacity→0, blur 0→6).

Render safety (required): both videos are timeline-driven media layers
(HyperFrames keeps them frame-synced — do NOT autoplay them); one paused GSAP
timeline on window.__timelines["main"]; no Date.now / Math.random; root div
with data-composition-id="main" data-duration="{CLIP_LENGTH}"
data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **It's a 3-layer sandwich:** footage → text → subject-cutout. The cutout doing the occluding is the entire trick; no masks-on-text needed.
- The two video layers must be frame-locked — one frame of drift between footage and cutout shows as a glowing halo around the person.
- Slow drift sells the depth. A fast-moving ghost word reads as a glitch, not a space.
- Body occludes, face never crowds: plan the word's path through the torso/shoulder zone.
