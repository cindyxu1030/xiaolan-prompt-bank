# 满幅散布巨字 · Scatter Full-Bleed

![preview](../assets/gifs/scatter-fullbleed.gif)

**效果:** 巨大的小写单词散布在人物四周 — 有的裁出画框，有的藏进身体后面，每个词跟着人声各自入场。johnbucog 的招牌画面。
*What it delivers: giant lowercase words scattered around the subject — some cropping off the frame edges, some tucked behind the body, each entering on its own VO beat. The signature johnbucog frame.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a scatter full-bleed
caption layer over a clip of a person ({CLIP — a standing subject with
negative space around them works best}).

The words (4-6, from one spoken line): {WORDS + VO timestamps}.
Hero word: {HERO + ONE_ACCENT}. Base: white. Heavy black-weight lowercase.

Build the scatter:
1. Map the frame's negative space around the subject (extract a frame,
   note where the empty zones are: above the head / left of torso / floor /
   right shoulder). Hand-place each word in a zone: different sizes
   (90-260px; the hero biggest), different positions — NOT a grid, NOT a
   lower-third. At least one word bleeds off the left or right frame edge
   (crop mid-glyph — containment is the template tell).
2. Occlusion: 1-2 words pass BEHIND the subject (matte sandwich: footage →
   behind-words → rembg cutout → front-words). The rest sit in front.
   Behind-words get +giant size (250px+); body occludes, face never.
3. Optional depth-of-field: the 1-2 words farthest from the subject's plane
   get filter: blur(2-3px) — the lens has an opinion about every word.

Animation timeline (clip length):
- Each word punches in on ITS OWN VO timestamp: {opacity 0, scale 1.16,
  y 18, blur 8} → {1, 1, 0, 0}, 4-6 frames, power4.out. No two words enter
  together — the scatter builds beat by beat.
- Life: every landed word keeps its own micro-motion (scale ≤1.045 or y
  drift ≤8px, sine yoyo, desynced periods) + the whole layer gets a slow
  1.5% push-in across the clip.
- The hero enters LAST and biggest, knocking every other word 4px outward
  from its position (radial settle) on impact.
- Exit: words blur out in reverse entry order, 0.1s apart.

Render safety (required): both video layers timeline-driven (no autoplay),
frame-locked; one paused GSAP timeline on window.__timelines["main"];
no Date.now / Math.random; root div with data-composition-id="main"
data-duration="{CLIP_LENGTH}" data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **Placement is scene-reading, not layout.** Map the actual empty zones in YOUR footage and put words there — the scatter must feel like the words found their own parking spots in the room.
- Size variance 90→260px and at least one edge-bleed crop; uniform sizes or full containment and it's a PowerPoint again.
- 1-2 behind-subject words ground the whole cloud in the scene; blur on the far words makes the lens complicit.
- One word per VO beat, hero last. The scatter is a rhythm section, not a poster dump.
