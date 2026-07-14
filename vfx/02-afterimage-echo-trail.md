# 残影拖尾 · Afterimage Echo Trail

![preview](../assets/gifs/afterimage-echo-trail.gif)

**效果:** 主体一个箭步冲过画面，身后留下一串渐隐的"分身"残影 — 越老的残影越透明越模糊，速度线同时炸开。冲刺、闪避、瞬移，全靠这一串影子卖速度。
*What it delivers: the subject dashes across frame leaving a string of fading "clone" afterimages — older echoes more transparent and blurred — with speed lines bursting alongside. Dashes, dodges, teleports: the ghost string sells the speed.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 5-second "afterimage
echo trail" VFX demo on deep charcoal {BG, e.g. #0E1014}.
Subject color: white; echo tint: {ECHO, e.g. #6C8CFF}.

Content: a subject glyph — {SUBJECT, e.g. a simple rounded humanoid
silhouette or a comet capsule} (~280px tall — it must dominate the
frame and read at 480px preview width; filled white with a soft glow)
— plus 7 pre-built ECHO COPIES of it (same shape, {ECHO} fill at
60%+ opacity on drop, no glow), and 6 speed-line streaks (8–12px
thick, gradient bars long enough to span half the dash). A faint
horizon line + dim floor-glow pool ground the space so the motion
has a reference frame.

THE ECHO TECHNIQUE (the entry's core): echoes are NOT spawned at
runtime — pre-build a SEPARATE pool per dash phase (7 + 7 + 3 = 17
copies; reusing one pool across dashes breaks under parallel-worker
frame seeking). During a dash, echo i is "dropped" at the position the
subject occupied at (dash start + i * dashDuration/8): precompute
those positions from the dash's known path/ease, then at each drop
moment the echo appears there and fades: opacity ~.62→0 (linear, for
a clean age gradient), blur 0→12px, scale 1→.92, over ~0.7s. Echoes
start HIDDEN (opacity 0) and use immediateRender:true keyframes — a
visible from-state with immediateRender:false goes invisible on
workers that seek past the drop moment. Result: a trail whose spacing
STRETCHES with the ease (bunched at launch, spread at top speed) —
that spacing gradient is what reads as acceleration.

Animation timeline (~5s):
- 0.0–0.7s  subject idles left-of-frame (breathe, tiny hover bob).
- 1.0s      wind-up: it leans back 8°, squashes .9, holds 3 frames.
- 1.1–1.7s  DASH 1 (L→R, power2.in then hard stop): echoes drop per
            the technique; 4 speed lines streak through the wake
            (scaleX 0→1 from the launch side, opacity .5→0, staggered
            40ms); the subject overshoots its stop mark 14px and snaps
            back (hard stop = 3-frame settle, no soft ease).
- 2.0s      beat: last echo dissolves; subject blinks its glow once.
- 2.4–3.1s  DASH 2 (diagonal ↘→↖, faster): same anatomy, echoes tint
            10% brighter, 2 extra speed lines angled to the path; on
            the stop, a small ring ripple at the arrival point.
- 3.4–4.2s  FINISHER: a short triple-blink dash (three 0.12s micro-
            teleports in a row — subject jumps position, ONE echo held
            at each vacated spot, 0.3s fade) — reads as blink-step.
- 4.2–5.0s  hold: subject idles at rest point, the final echoes drain,
            speed lines gone; one soft glow pulse as the wink.

Render safety (required): one paused GSAP timeline on
window.__timelines["main"]; echo drop positions precomputed from the
dash path (deterministic — no runtime cloning, no Math.random /
Date.now); finite repeats; root div with data-composition-id="main"
data-duration="5" data-width="1920" data-height="1080".
```

## 要点 Key technique notes

- **残影间距要跟着缓动"拉伸"** — 起步密、高速疏；等间距的残影读成复制粘贴，不是加速度。
- 残影是预建的 7 个副本按预计算位置 `tl.set()` 落位 — 运行时克隆节点在逐帧 seek 下必炸；全部提前算好。
- 冲刺的收尾要"硬停"（过冲 14px + 3 帧回弹）— 软着陆会把速度感全部还回去。
- 三连微瞬移（blink-step）是这套语言的王牌用法：位置跳变 + 原地残影 = 瞬移，比任何模糊都便宜且清楚。
