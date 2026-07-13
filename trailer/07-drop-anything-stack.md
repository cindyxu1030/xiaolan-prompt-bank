# 万物拖入 · Drop-Anything Stack

![preview](../assets/gifs/drop-anything-stack.gif)

**效果:** "Drop anything" 巨字坐在黑场中央，链接胶囊、DOC 卡、PPT 卡、图片缩略、视频片段从画框外甩进来，一张落地巨字被撞一下，最后素材吸进一道发光槽 — "什么都能喂给它"的物理感镜头。
*What it delivers: a giant "Drop anything" sits center-black while a link pill, DOC card, PPT card, image thumb and video clip hurl in from off-frame — the headline kicks on every landing — then everything gets sucked into a glowing intake slot. "Feed it anything," with physics.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 7-second "drop anything"
asset-rain scene on near-black {BG, e.g. #0B0B0F}.

Content: headline {HEADLINE, e.g. "Drop anything"} in a soft {C1, e.g.
#9AE6D8}→{C2, e.g. #B79AFF} duotone gradient fill, ~150px heavy grotesque,
centered slightly above middle. Five asset pieces (all pure CSS/SVG, no
real files/brands):
1) a link pill — 🔗 glyph + "{URL, e.g. yoursite.com/launch}" mono text;
2) a DOC card — white card, blue "DOC" tag, 3 gray text bars;
3) a PPT card — white card, orange "PPT" tag, a title bar + image rect;
4) an image thumb — rounded rect filled with a tiny CSS gradient scene;
5) a video clip — dark rounded rect, play triangle, 3-bar "waveform".

Build:
- Background: flat near-black + ONE dim blurred radial pool behind the
  headline; a faint dotted grid at 4% white for spatial reference.
- Assets land in a loose arc under/around the headline, each with its own
  resting tilt (-8°…+8°, assigned by index).
- The intake slot for the ending: a 460x14px rounded bar below center,
  invisible until its beat, with an accent glow when active.

Animation timeline (~7s):
- 0.0–0.8s  headline fades up from blur (opacity 0→1, blur 10→0, y 20→0);
            gradient slowly shifts across it (background-position tween,
            whole duration).
- 1.0–4.0s  the five assets fly in one at a time (~500ms apart), each from
            a DIFFERENT off-frame direction (4 edges + 1 corner) with
            rotation: enter at 1.15 scale
            → land with squash (scaleY .88 for 2 frames) → settle to its
            tilt (back.out(1.6)); a soft shadow fades in under it on
            landing. ON EACH LANDING the headline kicks: y +5px and back
            (sine.out, 0.18s) — impact transferred.
- 3.8–4.2s  beat of stillness: all five do one synchronized micro-bounce
            (y -6→0 stagger 40ms) — "ready".
- 4.3–5.6s  the intake slot fades in + glows; assets fly into it in landing
            order (power3.in, scale→0.1, slight arc via rotation), the slot
            flashes brighter on each swallow.
- 5.6–7.0s  the slot emits ONE pulse ring; headline gets a final gradient
            sweep + settles into a ≤1.02 breathe (finite yoyo) to the end.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; entry angles/tilts derived from index (no
Math.random / Date.now); finite repeat counts; root div with
data-composition-id="main" data-duration="7" data-width="1920"
data-height="1080".
```

## 要点 Key technique notes

- **落地三件套：squash + 影子浮现 + 巨字被撞** — 第三个最关键，标题跟着震说明这些东西"有质量"，镜头瞬间物理化。
- 五个素材五个入场边 + 各自倾角，同边入场读成列表动画。
- 结尾"吸入槽"把散点收束成一个动作，给下一镜头一个出口 — 发布片里它接的就是"生成中"。
- 素材全部 CSS/SVG 手搓（假 URL、假文档条），符合库规则：不放真品牌/真截图。
