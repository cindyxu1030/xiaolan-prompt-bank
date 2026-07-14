# 两端分置字幕 · Split-Edge Captions

![preview](../assets/gifs/split-edge-captions.gif)

**效果:** 一句短语拆成两半，一半飞向画面左缘、一半飞向右缘，把中间的主体夹在中间 — 电影感发布片里"字幕给画面让路"的那种克制排版。
*What it delivers: a short phrase splits in two — half gliding to the left frame edge, half to the right — bracketing the center subject between them. The restrained, cinema-grade caption treatment from launch films.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 6-second "split-edge
caption" scene on near-black {BG, e.g. #07080A}.

Content: a center subject placeholder — a breathing glowing orb (~120px,
layered CSS: radial-gradient sphere + blurred {ACCENT, e.g. #2FE0A0}
bloom behind). Two caption pairs, each pair = left fragment + right
fragment of one phrase:
pair 1: {L1, e.g. "It watches"} / {R1, e.g. "your footage"}
pair 2: {L2, e.g. "every"} / {R2, e.g. "movement"}
Closing line: {CLOSER, e.g. "and understands it."} centered.
Type: clean white grotesque, ~54px, letter-spacing 0.08em, soft glow
(text-shadow at 25%). Two colors max: white + the accent on ONE word.

Build:
- Flat near-black + one faint accent aurora pool low in frame (blurred
  element). The orb floats center at ~48% height, breathing (scale
  1→1.06 + bloom swing, sine.inOut yoyo, finite repeats) the whole time.
- Caption fragments sit at the SAME baseline height as the orb's center:
  left fragment LEFT-aligned with its outer edge 120px from the left
  frame edge, right fragment RIGHT-aligned 120px from the right frame
  edge — the negative space between them and the orb is the composition.

Animation timeline (~6s):
- 0.0–0.8s  orb fades up from blur; aurora blooms.
- 1.0s      pair 1: both fragments enter SIMULTANEOUSLY, each sliding
            outward from the center toward its edge (x ∓60→0 relative to
            final position, opacity 0→1, 0.6s power3.out) — they part
            like curtains around the orb.
- 2.6s      pair 1 exits (fragments continue outward 20px + fade, 0.35s)
            and pair 2 enters with the same grammar 0.15s later.
- 4.2s      pair 2 fragments converge toward center and fade (x ±90
            inward) as {CLOSER} wipes in below the orb (~130px under
            its center) behind a pure left→right clip-path wipe,
            accent on its key word.
- 4.8–6.0s  hold: orb keeps breathing, the closer micro-breathes ≤1.02,
            aurora drifts.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Math.random / Date.now; finite repeat
counts; root div with data-composition-id="main" data-duration="6"
data-width="1920" data-height="1080".
```

## 要点 Key technique notes

- **两个 fragment 必须同时入场、朝相反方向滑** — 像帘幕向两边拉开。先左后右就变成了普通字幕排队。
- 字幕基线和主体中心同高，让"主体被句子夹住"的几何成立；字幕放上下就丢了包夹感。
- 全片就两种颜色：白 + 一个 accent 词。发布片字幕的高级感全靠字距（0.08em+）和留白。
- 中间的 orb 可以换成任何主体（产品渲染、frame 里的 footage）— 呼吸不能停。
