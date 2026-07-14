# 画框破格 · Frame Break-Out

![preview](../assets/gifs/frame-breakout.gif)

**效果:** 内容先在一个圆角发光画框里播放，讲到重点那一拍，画框猛地撑满全屏 — "给你看一眼"升级成"带你进去"。
*What it delivers: content plays inside a rounded glowing frame until the point lands — then the frame expands to full-bleed. "Here's a peek" escalating into "step inside."*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 7-second "frame break-out"
scene on near-black {BG, e.g. #07080B}. Accent: {ACCENT, e.g. #35E0C8}.

Content: inside the frame lives a built CSS "footage" scene (no real
video): a dusk landscape made of 3 layered gradient bands (sky / hills /
ground in deep teal-navy), a low glowing sun disc, 2 drifting cloud
blurs, and a small moving silhouette (a simple rounded-rect vehicle or
walking-dot) crossing slowly. Grain overlay (static baked texture, 4%).
Caption pair: small setup {SETUP, e.g. "it sees the whole scene"} beside
the frame, and full-bleed statement {STATEMENT, e.g. "every frame of
it."} after the break-out — accent on ONE word.

Build:
- The frame: a rounded container (radius 28px, ~46% frame width, 16:9
  inner ratio) centered-left at ~40% x; 1.5px white 30% stroke + a soft
  accent outer glow. The scene inside always fills it (overflow hidden).
- The inner scene keeps moving for the WHOLE composition — parallax
  drift on the bands (±10-20px, slower the farther), sun glow breathing,
  silhouette crossing.
- {SETUP} caption sits right of the frame with a thin connector line.

Animation timeline (~7s):
- 0.0–0.9s  frame rises in (y 40→0, scale .95→1, power3.out), glow
            blooms; inner scene already alive.
- 1.2s      {SETUP} caption wipes in behind a clip-path + its connector
            line draws from the frame edge.
- 2.0–3.6s  dwell: inner scene parallax continues; the frame itself
            drifts ±6px on a slow sine; glow pulses once.
- 3.8–4.6s  THE BREAK-OUT: caption + connector snap-fade out (0.2s);
            the frame expands to full-bleed VIA TRANSFORM SCALE
            (author the frame at native full-bleed size and present
            the dwell state scaled down; tween back to identity —
            never animate width/height, layout props break under
            seek), border-radius 28→0 (pre-divide by the dwell scale
            so it reads as 28px when small), stroke and glow fade to
            0 — power3.inOut; the inner scene scales up in sympathy
            1→1.08 so the expansion reads as a camera push.
- 4.8s      {STATEMENT} lands over the full-bleed scene: words stagger
            in 70ms apart (y 24→0, blur 5→0), ~110px heavy grotesque,
            accent word colored; a subtle dark gradient scrim fades in
            at the bottom third to hold contrast.
- 5.4–7.0s  hold with life: scene parallax + statement micro-breathe
            ≤1.02; at 6.4s the sun glow flares gently as a wink.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Math.random / Date.now; finite repeats;
grain static; root div with data-composition-id="main" data-duration="7"
data-width="1920" data-height="1080".
```

## 要点 Key technique notes

- **破格瞬间给内景一个 1→1.08 的同步放大** — 这让扩张读成"镜头推进去"，不加的话是"窗户被拉大"，廉价一半。
- 内景在框里的每一秒都要活着（视差 + 呼吸 + 移动剪影）— 静止内容破格出来也只是一张大图。
- 全屏后底部要压一层暗渐变 scrim，statement 才有对比度可站。
- 框期字幕小而侧置、破格后字幕大而全屏 — 尺寸跳变本身就是"重点到了"的信号。
