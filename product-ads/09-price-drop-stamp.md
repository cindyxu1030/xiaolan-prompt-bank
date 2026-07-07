# 降价爆点广告 · Price-Drop Stamp

![preview](../assets/gifs/price-drop-stamp.gif)

**效果:** 原价被一道划线划掉，新价从天砸下弹定，"限时 5 折"印章"咔"地盖上，倒计时开始跳 — 促销转化那一击。
*What it delivers: the old price gets struck through, the new price slams down and bounces, a "50% OFF" stamp hits, and a countdown starts ticking — the conversion punch of a sale.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 5-second price-drop
promo on {BG — high-energy brand gradient or flat accent}.

Product/offer: {NAME}. Old price: {OLD e.g. ¥398}. New price: {NEW e.g. ¥199}.
Discount: {PCT e.g. 50}% / label {LABEL e.g. "限时5折"}. Countdown: {TIME e.g.
"23:59:47"}. Accent: {ACCENT}. Sale color: {SALE e.g. #E4322B}.

Build the layout (center column):
- Product name / small hero at top.
- Old price (large, will be struck through).
- New price (HUGE, tabular-nums, SALE color, will slam in).
- A round or ribbon STAMP with "{PCT}% OFF" / {LABEL}.
- A countdown timer row (HH:MM:SS in mono/tabular digits).
- Optional CTA button.

Animation timeline (~5s):
- 0.0-0.6s  name + old price fade in (calm — this is "before").
- 0.8s      STRIKE: a SALE-color line draws through the old price
            (scaleX 0→1, 0.25s power2.out), and the old price dims to 55% +
            shrinks slightly (it's demoted).
- 1.1s      NEW PRICE SLAMS: drops from above (y -80→0, scale 1.3→1,
            back.out(2)), overshoots, settles; a 1-frame impact shake + a
            radial shock ring; the digits can count DOWN from old→new fast
            (0.3s) for extra drama.
- 1.6s      STAMP hits: the "{PCT}% OFF" stamp slams on at a tilt
            (scale 1.6→1, rotation -12°→-6°, back.out(2)) with an ink-thud
            flash — like a rubber stamp.
- 2.2s      countdown starts: the seconds digit begins ticking down
            (authored per-second .set()s, tabular-nums); a subtle urgency
            pulse on the timer.
- 2.6-5s    hold: CTA button pops + breathes (glow), new price keeps a tiny
            heartbeat scale, one shine sweep across the stamp.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; countdown digits are authored .set()s per tick
(no Date.now — never read the real clock); count-downs tween a plain object;
no Math.random; finite repeats; root div with data-composition-id="main"
data-duration="5" data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **Never read the real clock for the countdown** — author the ticking digits as `.set()`s per second on the timeline. `Date.now()`/`new Date()` break deterministic rendering (and are blocked in the render environment).
- Demote-then-slam: strike + dim the old price BEFORE the new one lands, so the eye is primed for the drop. Both events on top of each other reads as clutter.
- The stamp lands tilted (`-12°→-6°`) — a perfectly straight stamp looks like a UI badge, a crooked one looks slammed.
- `tabular-nums` on every price and timer digit, and tween count-downs on a plain object — jittering sale numbers kill urgency.
