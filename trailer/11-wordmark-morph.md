# 字标变身 · Wordmark A→B Morph

![preview](../assets/gifs/wordmark-morph.gif)

**效果:** 一个宽字距字标"变身"成另一个 — 共用的字母滑到新位置，多余的字母化雾消失，新字母从光点里长出来 — 母品牌引出子产品的那一拍（NOCTAL→LANA 式）。
*What it delivers: one wide-tracked wordmark transforms into another — shared letters glide to their new slots, extra letters dissolve to mist, new letters bloom from a light point. The parent-brand-reveals-product beat (NOCTAL→LANA style).*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 6-second wordmark morph
from {BRAND_A, e.g. "NOCTURN"} to {BRAND_B, e.g. "LUNA"} on near-black
{BG, e.g. #08090C}. Accent: {ACCENT, e.g. #35E0C8}.

Type: thin-to-medium weight CAPS, wide letter-spacing (0.35em+), white,
~110px — the elegant launch-film register, NOT a heavy slam face.
Each letter is its own span (needed for per-letter choreography).

Build:
- Flat near-black + a dim accent pool behind the wordmark zone (blurred
  element) + a tiny glowing "seed" dot (accent, 10px + bloom) hidden at
  center, used as the birth point.
- Compute both layouts up front: letter positions for A centered, letter
  positions for B centered (FLIP-style: measure, then tween between).
  Classify letters: SHARED (appear in both names — reuse the actual
  letter, map each to its nearest same-letter slot in B), OLD-ONLY,
  NEW-ONLY.

Animation timeline (~6s):
- 0.0–1.0s  {BRAND_A} assembles: letters fade in with a 45ms stagger
            (y 16→0, blur 4→0), tracking settles from 0.5em→0.35em.
- 1.4s      a thin accent underline draws beneath it (scaleX 0→1).
- 2.2–3.4s  THE MORPH: the underline retracts; OLD-ONLY letters dissolve
            upward (opacity→0, y -14, blur 6, 60ms stagger); SHARED
            letters glide along gentle arcs to their B positions
            (power3.inOut, 0.9s, slight scale dip to .92 mid-flight);
            simultaneously the seed dot flares at center and NEW-ONLY
            letters bloom outward from it (scale 0.4→1, blur 8→0,
            opacity, 70ms stagger, back.out(1.4)).
- 3.6s      {BRAND_B} locks: one soft accent glow pulse washes through
            the letters left→right (color tween there-and-back, 80ms
            stagger); the underline redraws at B's width.
- 4.2–6.0s  hold with life: letter-spacing eases +0.02em wider over the
            hold (the "settling exhale"), the pool breathes, and at 5.4s
            ONE letter's glow pings as a wink.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; letter targets measured deterministically at
build time; no Math.random / Date.now; finite repeats; root div with
data-composition-id="main" data-duration="6" data-width="1920"
data-height="1080".
```

## 要点 Key technique notes

- **先把 A、B 两套字母坐标都量出来再动（FLIP 思路）** — 共用字母飞向"最近的同字母槽位"，这条映射错了整个变身就乱了。
- 三类字母三种命运：共用=滑行、旧有=化雾上飘、新增=从中心光点里长出来。同时发生才像"变身"，排队发生像"换片"。
- 宽字距细体 CAPS 是这个镜头的气质底座 — 换成重锤字体就成了另一个（已有的）条目。
- 收尾的字距再放宽 0.02em 是"呼气"— 观众感觉不到但缺了就僵。
