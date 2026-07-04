# 数据战报仪表盘 · Count-Up Stat Dashboard

![preview](../assets/gifs/countup-dashboard.gif)

**效果:** 一张编辑部风格的数据战报: 巨大的主指标数字滚动跳涨，副指标行依次点亮，涨幅标签弹出 — 月报/战报/复盘视频的镇场镜头。
*What it delivers: an editorial-style stat report card — the giant hero metric counts up, secondary metric rows light in sequence, growth badges pop. The anchor shot for any monthly-report or recap video.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 6.5-second animated stat
dashboard.

Report title: {TITLE e.g. "近30天数据战报"} + period: {PERIOD e.g. "06.01–06.30"}.
HERO metric: {HERO_LABEL e.g. "播放量"} = {HERO_VALUE e.g. 186500}.
4 secondary metrics: {LABEL: VALUE, ×4 — e.g. 点赞 4520 / 分享 980 / 评论 265 /
净增粉丝 1560}. One highlight stat: {HIGHLIGHT e.g. "封面点击率 32.8%"}.
Palette: {BG e.g. cream #F5EFE2} / {INK dark} / {ACCENT e.g. #002FA7}.

Make the numbers editable: declare them as composition variables via a
data-composition-variables JSON attribute on <html> (id/type/label/default per
metric) and read them in JS — so next month's report re-renders by changing
variables, not code.

Layout (editorial, not "dashboard software"):
- Header: small kicker row (dot + title + mono-font EN tag) over a thin
  accent rule; period line in a mono font below.
- Hero block: label row, then the hero number HUGE — 150px+, font-variant-
  numeric: tabular-nums (mandatory — proportional digits jitter while counting),
  letter-spacing slightly negative, subtle accent text-shadow glow.
- Secondary grid: 2x2 rows, each = label (28px, muted) + value (64px bold,
  tabular-nums) + a small "+X%"-style badge chip.
- Highlight bar near the bottom: one accent-filled strip with the highlight
  stat reversed out.

Animation timeline (~8s):
- 0.0–0.6s  header assembles: rule scaleX 0→1 from left, kicker fades, period
            types in (clip-path reveal).
- 0.8–2.6s  HERO COUNT-UP: number rolls 0 → HERO_VALUE over 1.8s, power2.out
            (fast start, landing slow), via a tweened counter object rendered
            with toLocaleString(). A thin accent underline draws beneath it as
            it lands, and the number does one 1.04 punch on arrival.
- 2.8–4.8s  secondary rows light up in sequence (0.35s apart): each fades+rises
            (y 18→0), its value counts up over 0.7s, then its badge chip pops
            (scale 0→1, back.out(2)).
- 5.2s      highlight bar wipes in (clip-path inset left→0) with its stat
            counting to value.
- 5.6–6.5s  hold with life: hero glow breathes, badge chips micro-bob one at a
            time in a slow loop (finite repeats).

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; the count-ups tween a plain JS object's value ON
the timeline (onUpdate writes textContent) — deterministic; no Date.now /
Math.random; root div with data-composition-id="main" data-duration="6.5"
data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **`font-variant-numeric: tabular-nums` is mandatory** on anything that counts — proportional digits make the whole line shiver as widths change.
- Count-ups tween a number on a plain object (`gsap.to(counter, {value: N, onUpdate})`) — that keeps them ON the seekable timeline, unlike setInterval hacks.
- `power2.out` on the count = big numbers fly early, land slow — the landing is where the viewer actually reads the value.
- Declaring metrics as `data-composition-variables` turns the render into a **template**: next report = new variable values, zero code edits.
