# 冲击波命中 · Impact Shockwave

![preview](../assets/gifs/impact-shockwave.gif)

**效果:** 目标被连续三击 — 每次命中：白闪一瞬、冲击波环扩散、火花四溅、飘出数字，整个画面按"创伤值"震颤，越重的击打抖得越狠、衰减越慢。游戏级打击感，零素材纯代码。
*What it delivers: a target takes three escalating hits — each impact fires a white flash, an expanding shockwave ring, radial sparks, a floating number, and trauma-based screen shake: heavier hits shake harder and decay slower. Game-grade impact feel, zero assets, pure code.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 5-second "impact
shockwave" VFX demo on deep charcoal {BG, e.g. #0D0E12}.
Accent: {ACCENT, e.g. #FFB13D}; flash white; numbers {NUM_COLOR, e.g.
#FFF3D6}.

Content: a hero target centered — {TARGET, e.g. a hexagonal sigil}
built from SVG strokes with a soft accent glow, idling (breathe ≤3% +
slow 4° sway). Everything lives inside a "camera" wrapper div — the
shake is applied to the wrapper, never to individual elements.

THE TRAUMA SHAKE (the entry's core): keep a proxy {trauma: 0}. Each
hit ADDS trauma (hit1 +0.4, hit2 +0.6, hit3 +1.0, clamp 1). Trauma
decays linearly to 0 over ~0.7s (a tween per hit). In the timeline's
onUpdate, set camera offset from trauma²:
  x = trauma² * 26 * sin(t*127),  y = trauma² * 22 * cos(t*151),
  rotation = trauma² * 1.6 * sin(t*113)   (t = tl.time(); the primes
desync the axes). Trig-of-time = deterministic under seek; trauma² =
small hits whisper, big hits roar.

Per-hit anatomy (all five cues fire on the SAME frame):
1. white flash: a full-frame white overlay 0→65%→0 in 3 frames;
2. shockwave: 2 concentric rings from the impact point (thin accent
   stroke, scale 0.2→2.6/3.4, opacity .8→0, 0.5/0.7s, power2.out);
3. sparks: 10 small accent rects flying radially (angles = i*36° +
   hit# offset, distance 160–260px by index trig, rotate as they fly,
   fade by 0.4s);
4. floating number: {e.g. "128" / "412" / "999"} pops at the impact
   point +offset, rises 60px and fades (0.8s), scale grows per hit;
5. the target itself: recoil 12px opposite the hit direction + a 2-
   frame accent color flash on its strokes, back.out recover.

Animation timeline (~5s):
- 0.0–0.8s  target fades up, idle starts.
- 1.0s      HIT 1 (light, from the left): full anatomy, trauma +0.4.
- 2.2s      HIT 2 (medium, from upper-right): trauma +0.6; one crack
            line (jagged SVG path) dash-draws across the target and
            stays.
- 3.4s      HIT 3 (heavy, from below): trauma +1.0; both rings plus a
            third slow ring; the target drops 20px and recovers with
            elastic.out; a second crack; numbers biggest.
- 3.9–5.0s  aftermath: trauma decays to stillness, cracks glow faintly,
            the target's idle resumes — wounded but standing; glow
            embers (6 dots) drift up slowly.

Render safety (required): one paused GSAP timeline on
window.__timelines["main"]; shake = trauma proxy + trig of timeline
time (NO Math.random / Date.now); spark angles/distances from index;
finite repeats; root div with data-composition-id="main"
data-duration="5" data-width="1920" data-height="1080".
```

## 要点 Key technique notes

- **trauma² 是打击感的物理学** — 线性映射的抖动"电风扇"，平方后小击轻颤、重击狂暴，衰减自然。
- 抖动只加在 camera 包裹层，且用"质数频率的 sin/cos(tl.time())"— 三轴不同步才像失控，time 驱动才 seek 确定。
- 五个命中信号必须同帧起跳（闪/环/火花/数字/后坐）— 错开 1 帧打击感就散了。
- 裂纹留在目标上不消失 — 伤害的"记账"让三连击有累积叙事。
