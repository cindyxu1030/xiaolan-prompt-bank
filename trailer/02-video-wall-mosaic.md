# 成片墙爆开 · Video-Wall Mosaic

![preview](../assets/gifs/video-wall-mosaic.gif)

**效果:** 几十张竖版成片卡从景深里飞进来，散落成一面错落的作品墙，前后有虚实，一行大字数着"50 videos. one upload." — 用密度本身当卖点的镜头。
*What it delivers: dozens of vertical output cards fly in from depth and scatter into a staggered wall — near cards sharp, far cards blurred — while a stat line counts "50 videos. one upload." Density itself is the sales pitch.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 7-second "output wall" scene
on a deep {BASE_COLOR, e.g. royal blue #122FA8} void.

Copy: "{COUNT, e.g. 50} {NOUN, e.g. videos}. {QUALIFIER, e.g. one upload.}"
— the count in huge bold, the qualifier in an italic serif/script contrast
face. Text color white.

Build:
- Background: {BASE_COLOR} base + 2 big blurred radial pools (one lighter,
  one deeper shade) drifting slowly — blurred elements, not full-bleed
  gradients.
- 24 vertical 9:16 cards (~150x266px, radius 16px) — the wall stays at ~24
  cards regardless of {COUNT}; the wall suggests density, the number makes
  the claim. Each card is a stand-in "video":
  a unique 2-stop CSS gradient scene (vary hue/angle BY INDEX, no repeats
  side by side), a subtle inner top highlight, and a small white play
  triangle at low opacity. No real footage, no logos.
- Cards live in one 3D-perspective parent (perspective 1200px). Assign each
  card a depth tier: front (sharp, larger), mid (blur ~1.2px), back
  (smaller, blur 3–6px, dimmed 20%). Scatter positions asymmetrically across the frame with an
  index-derived jitter (trig on index, NOT Math.random), leaving a clear
  band for the text on the left third.
- Stat line sits left-center on top: count digits (~170px) + noun, second
  line italic qualifier (~64px).

Animation timeline (~7s):
- 0.0–2.5s  cards fly in: from z -900px / random-looking offsets (index
            trig), staggered 60ms apart (power3.out, 1.1s each), each
            settling into its wall slot with a tiny overshoot (back.out(1.2)).
            Back tier arrives first, front tier lands last and largest.
- 1.6–2.6s  the count rolls up 0→{COUNT} (snap: 1) as the noun fades in;
            the italic qualifier slides up 0.3s later (y 24→0).
- 2.6–6.2s  the whole wall parallax-drifts: parent rotateY -3°→3° and
            x ±20px (sine.inOut yoyo, finite repeats); front cards float
            y ±8px on offset phases (index-derived), play triangles pulse
            opacity gently in a staggered wave.
- 6.2–7.0s  the center-most front card scales up 8% and brightens as if
            selected
            (a closing wink), everything else keeps drifting.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; all "randomness" derived from index/trig (no
Math.random, no Date.now); finite repeat counts; root div with
data-composition-id="main" data-duration="7" data-width="1920"
data-height="1080".
```

## 要点 Key technique notes

- **密度是主角，文字是配角** — 卡片必须 20+ 张、深浅三层，少于这个量读出来是"几个卡片"，不是"产能"。
- 景深靠三件套：尺寸差 + blur 差 + 亮度差。只缩小不加 blur，墙会看起来是平的。
- 卡片散布用 index 三角函数抖动，肉眼是随机的，渲染是确定的。
- 前排最后落地、最大、带 overshoot — 落点顺序就是视线动线。
