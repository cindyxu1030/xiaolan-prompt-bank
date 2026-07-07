# 环形仪表盘 · Radial Gauge Fill

![preview](../assets/gifs/radial-gauge.gif)

**效果:** 一个半圆/环形仪表，指针从零扫到目标值，弧条跟着填充变色，中心大数字滚到位 — 单一 KPI 达成率的镇场镜头。
*What it delivers: a semicircular gauge whose needle sweeps from zero to target as the arc fills and shifts color, the big center number rolling into place — the anchor shot for a single-KPI achievement.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 5-second radial gauge
on {BG — dark #0E1116 or light #F4F1EA}.

Title: {TITLE e.g. "目标完成率"}. Gauge value: {VALUE e.g. 87} out of {MAX e.g.
100} {UNIT e.g. "%"}. Optional threshold zones: {e.g. red<40, amber 40-70,
green>70}. Accent: {ACCENT}. Ink: {INK}.

Build the gauge:
- A 240°-sweep arc (or full ring) as a background track (muted) + a value
  arc on top (ACCENT or zone-colored), both SVG stroke arcs. The value arc
  reveals via stroke-dashoffset to the fraction VALUE/MAX.
- Optional tick marks around the arc (small radial lines) + zone-colored
  bands on the track.
- A NEEDLE: a thin tapered pointer pivoting at the gauge center
  (transform-origin center), rotating from the min angle to the value angle.
- Center: the value number huge (tabular-nums) + unit + label beneath.

Animation timeline (~5s):
- 0.0-0.6s  gauge track + ticks + title fade/scale in.
- 0.8-2.6s  the SWEEP: needle rotates min-angle → value-angle AND the value
            arc fills in lockstep (share one progress proxy in onUpdate),
            ease power2.out (fast then eases into the target). The center
            number counts up in sync. The arc COLOR interpolates through the
            zones as it passes them (red→amber→green) if using thresholds.
- 2.6s      arrival: needle does a tiny overshoot past the value then settles
            (back.out), a 1.08 punch on the center number, a glow pulse on
            the arc tip.
- 2.8-5s    hold: needle micro-jitters ±0.4° (a live meter never sits dead
            still, sine, finite repeats); arc tip glows; label steady.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; needle + arc + count share one progress value in
onUpdate (perfect sync); arc via stroke-dashoffset; no Date.now / Math.random;
finite repeats; root div with data-composition-id="main" data-duration="5"
data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **Drive needle, arc, and number from ONE progress value** in a single `onUpdate` — if they're separate tweens they drift and the needle points at the wrong spot on the arc.
- Needle pivots at the gauge center (`transform-origin: center` on a pointer whose base sits at center); the tiny overshoot-and-settle at the target is what makes it read as a physical meter.
- Interpolating the arc color through threshold zones as it fills (red→amber→green) tells the "good/bad" story without a legend.
- The ±0.4° idle jitter on the hold sells "live instrument" — a frozen needle looks like a screenshot.
