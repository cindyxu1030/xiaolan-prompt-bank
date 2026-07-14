# 手绘小人演员 · Sketch Puppet Actor

![preview](../assets/gifs/sketch-puppet-actor.gif)

**效果:** 一个手绘小人真的在"演戏"：走进画面、抓住一团乱线、身体后仰使劲拽、拽开后拍拍手眨眨眼 — 不是滑动的贴纸，是有关节、有重量、有反应的角色。
*What it delivers: a hand-drawn character genuinely ACTS — walks in, grabs a tangled scribble, leans back and heaves, untangles it, dusts its hands and blinks. Not a sliding sticker: a rigged, weighted, reacting performer.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 7-second
"sketch puppet" acting beat on warm paper {PAPER, e.g. #F7F2E7}.

The character (pure SVG, {INK, e.g. #1F3A93} strokes): a simple blob
body path + 2 white dot eyes (with lid rects for blinks) + thin
2-segment arm groups + 2 leg groups — EACH limb group pivoted at its
own joint (shoulder/hip) via transform-origin at the local joint
coordinate.
The prop: a tangled scribble ball (one long messy SVG path) sitting
right-of-center, with a loose thread end. After untangling it becomes
a straight line leading to a {REWARD, e.g. a lightbulb glyph}.
Caption {CAPTION, e.g. "解开它"} in a casual handwritten face, appears
at the payoff.

RIGGING TECHNIQUE (the entry's core): drive all body parts from plain
proxy objects (walk = {bob, legL, legR, armL, armR, lean, blink}),
tween the PROXIES on the timeline, and compose every part's transform
attribute in ONE applyAll() inside the timeline's onUpdate — this
sidesteps GSAP's SVG transform-origin pain and stays deterministic
under seek. Add the boiling-line treatment (3 feTurbulence/
feDisplacementMap plates swapped via floor(tl.time()*8)%3) over the
whole sketch group.

Animation timeline (~7s):
- 0.0–0.5s  scene draws on: ground line + scribble ball (dashoffset).
- 0.5–2.0s  WALK-IN from left: legs swing alternately (proxy yoyo ±22°
            out of phase), body bobs ±6px synced to steps, arms
            counter-swing slightly; a blink mid-walk.
- 2.0–2.6s  GRAB: it stops at the thread end, leans forward (lean
            +12°), arm reaches (shoulder rotates), hand "closes" on
            the thread (a small c-shape snaps to the thread end).
- 2.6–4.2s  THE HEAVE: two attempts — lean back -18° with both legs
            braced (heels dig, two little dust ticks), pull; scribble
            deforms but holds (elastic wobble); second pull HARDER
            (lean -26°, one foot hops), then the tangle SNAPS free:
            the scribble path un-draws to a straight line (dashoffset
            run-out) while the character stumbles back 2 steps
            (elastic.out recover, arms windmill once).
- 4.4–5.2s  PAYOFF: the straight line draws to the {REWARD} which pops
            on (back.out(2)); character dusts hands (both arms brush
            twice), then turns to "camera" (eyes shift) and blinks
            twice.
- 5.4–7.0s  hold: reward glows softly, character idle (breath bob ±3px,
            one more blink at 6.4s), boiling never stops.

Render safety (required): one paused GSAP timeline on
window.__timelines["main"]; ALL limb motion via proxy objects composed
in onUpdate (deterministic); boiling plates from timeline time; hard
tl.set() to final poses after any elastic; no Math.random / Date.now;
finite repeats; root div with data-composition-id="main"
data-duration="7" data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **代理对象 + 单一 applyAll() 是这套 rig 的命门** — 直接 tween SVG 子元素的 transform-origin 会在 seek 下漂移；把所有关节角度收进 proxy，每帧一次性拼 transform 字符串。
- 表演的重量感来自"反作用"：拽不动时身体后仰、脚跟蹬出尘点；拽开瞬间要向后趔趄两步再弹性回正。
- 每个 elastic 之后紧跟一个硬 `tl.set()` 锁定终态 — 弹性尾巴在逐帧渲染下会留残影。
- 眨眼是廉价的灵魂：走路中一次、望镜头两次、收尾一次 — 没有眨眼就是家具。
