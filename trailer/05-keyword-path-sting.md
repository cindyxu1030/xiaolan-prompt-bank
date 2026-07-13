# 关键词连线快闪 · Keyword Path Sting

![preview](../assets/gifs/keyword-path-sting.gif)

**效果:** 一条渐变光线在黑场里划出 S 形轨迹，划到哪儿哪个关键词弹出来，三个词三个颜色，最后光线退场、三个词合体成一句宣言 — 30 帧讲清方法论的节奏镜头。
*What it delivers: a gradient light stroke draws an S-path across black, popping a keyword at each waypoint — three words, three colors — then the path exits and the words merge into one tagline. Your methodology in a single rhythmic sting.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 6-second kinetic "keyword
path" sting on near-black {BG, e.g. #0A0A0C}.

Content: three keywords {WORD_1, e.g. "Hunt"} / {WORD_2, e.g. "Analyze"} /
{WORD_3, e.g. "Recreate"}, colored {C1, e.g. #35E0C8} / {C2, e.g. #7A5CFF} /
{C3, e.g. #F06FD8}. Closing line: "{WORD_1}, {WORD_2}, {WORD_3} —
{TAGLINE, e.g. "viral video with AI."}" (tagline in white).

Build:
- The path: ONE SVG path — a smooth S-curve entering off-frame left at
  ~62% height, swinging up through 3 waypoints (x ≈ 22% / 50% / 78%),
  exiting off-frame right. Stroke ~6px, round caps, stroked with a
  C1→C2→C3 linearGradient; add a wider (=~18px) low-opacity copy of the
  same path underneath as a glow.
- Each keyword sits just above its waypoint: 90px heavy grotesque in its
  color, with a small dot marker on the path itself.
- The closing line is a separate centered group: ~84px, the three words in
  their colors + the tagline in white.

Animation timeline (~6s):
- 0.0–2.4s  the path draws left→right via strokeDashoffset (power2.inOut);
            the glow copy draws in sync.
- as the draw front passes each waypoint (compute the crossing times from
  the path geometry — getPointAtLength arc-fractions mapped through the
  ease, NOT hardcoded guesses): dot pops, the word punches in (scale
  1.25→1 power4.out ~0.2s, opacity 0→1, y 14→0) — {WORD_1}, then
  {WORD_2}, then {WORD_3}.
- 2.6–3.1s  the path un-draws through the exit side (dashoffset continues)
            and its glow fades; dots shrink away; the three standing words
            hold with a micro-breathe.
- 3.1–3.8s  regroup: the three words shrink-slide (power3.inOut) into their
            slots in the centered closing line while the tagline wipes in
            behind a left→right clip-path.
- 3.8–6.0s  hold with life: the whole line breathes ≤1.03 (sine yoyo,
            finite), and a narrow diagonal sheen sweeps across the text
            once at ~4.6s (a rotated translucent white bar clipped to the
            text GLYPHS — e.g. background-clip:text on a duplicate line —
            not to the block's bounding box).

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; path draw via strokeDashoffset tweens; no
Math.random / Date.now; finite repeats only; root div with
data-composition-id="main" data-duration="6" data-width="1920"
data-height="1080".
```

## 要点 Key technique notes

- **词的弹出必须踩在描画前沿经过的那一帧** — 提前或滞后 5 帧都会断掉"线带出词"的因果感。用 getPointAtLength 算出每个 waypoint 的弧长占比，再过一遍 ease 的反函数得到精确时刻 — 不要拍脑袋写死时间。
- 光线 = 同一条 path 画两遍：细的实线 + 粗的低透明 glow。只有细线会显得廉价。
- un-draw 退场继续用 dashoffset（往 exit 方向推），别用整体 opacity 淡出 — 线要"走完"，不是"消失"。
- 合体那一步是把三个独立词 tween 到终稿行内各自的位置（FLIP 思路：先量好目标坐标），一步到位，不要中途换元素。
