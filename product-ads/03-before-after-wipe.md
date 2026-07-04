# Before/After 对比擦除 · Before-After Compare Wipe

![preview](../assets/gifs/before-after-wipe.gif)

**效果:** 一条发光的分割线从右侧扫入，把"之前"擦成"之后"，左边始终是之前、右边始终是之后 — 3 秒讲完一个升级故事。
*What it delivers: a glowing divider sweeps in from the right, wiping "before" into "after" — left of the divider is always BEFORE, right is always AFTER — a full upgrade story in 3 seconds.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 6-second before/after
compare-wipe ad.

BEFORE state: {BEFORE — an image, or a CSS-built scene, e.g. "cluttered desktop
with 23 windows"}. AFTER state: {AFTER — same framing, upgraded, e.g. "one clean
dashboard"}. Both states MUST share identical framing/geometry so the wipe reads
as transformation, not a cut.
Labels: {BEFORE_LABEL e.g. "之前"} / {AFTER_LABEL e.g. "之后"}.
Accent: {ACCENT_COLOR}.

Build the stage:
- Stack the two states full-bleed, AFTER on top. Clip the AFTER layer with
  clip-path: inset(0 0 0 100%) so it's hidden, revealed from the RIGHT by
  animating the LEFT inset toward 0. This keeps the corner labels truthful at
  every frame: left of the divider is always BEFORE, right is always AFTER.
  NEVER animate width — clip-path keeps the content pinned so the two layers
  stay pixel-aligned.
- The divider: a 4px vertical accent-color line + a soft glow (box-shadow /
  blurred twin) + a small grabber chip in the middle, positioned to ride exactly
  at the reveal edge (animate its x in lockstep with the clip inset — same
  duration/ease values).
- Labels top-left and top-right in bold 56px; BEFORE label starts full opacity,
  AFTER label starts dim.

Animation timeline (~6s):
- 0.0–0.7s  BEFORE state settles in (fade + 1.03→1 scale), BEFORE label pops.
- 1.2–2.6s  THE WIPE: divider starts at the right edge; divider + clip sweep
            right→left with power2.inOut. As the edge passes center,
            cross-fade label emphasis (BEFORE dims to 40%, AFTER rises to
            100% with a small punch scale 1.12→1).
- 2.8s      arrival: divider glow pulses once; 2–3 accent sparkle ticks pop
            along the reveal edge (scale 0→1→0, staggered).
- 3.2–4.4s  tease-back: divider drifts back toward the right (to ~38%
            revealed) and returns to ~96% revealed (sine.inOut) — the
            "compare me" wiggle that invites a rewatch.
- 4.4–6s    final commit: UNLOCK the divider from the clip — the clip finishes
            to full AFTER while the divider independently glides back and
            parks near the right edge (~96%), out of the way; gentle 1.5%
            push-in on the AFTER layer for the hold.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; root div with
data-composition-id="main" data-duration="6" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **`clip-path: inset()` — never `width`.** Animating width squishes the underlying content; inset reveals it in place, which is what makes it read as the *same thing transformed*.
- The divider must ride the clip edge exactly: give both tweens identical duration + ease, or the line visibly detaches from the reveal.
- The tease-back wiggle is the retention trick — a straight one-way wipe gets swiped away at 3s.
- Identical framing between states is non-negotiable; if the camera "jumps" at the edge, the illusion dies.
