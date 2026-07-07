# 3D 立体旋转 Logo · 3D Extrude Spin

![preview](../assets/gifs/3d-extrude-spin.gif)

**效果:** logo 带着厚度在空间里旋转飞入，侧面能看到挤出的立体棱边，转正时一道高光扫过 — 有重量感的立体标志。
*What it delivers: the logo spins in through space with real extruded thickness — you see its beveled side edges — and a highlight sweeps as it settles face-on. A logo with weight.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1920x1080 HyperFrames composition — a 5-second 3D extruded logo spin
on {BG — flat #0B0D12 with a soft floor gradient}.

My logo: {YOUR_LOGO — a bold mark or single glyph/monogram works best for
extrusion}. Face color: {ACCENT}. Side/extrude color: a darker shade of ACCENT.

Build the extrusion (CSS 3D, no libraries needed):
1. Wrap everything in a stage with perspective: 1400px.
2. The extruded logo = the face element + a stack of copies each pushed back
   on Z in the darker side color, forming a solid extruded body; the frontmost
   is the bright face. Step deep enough that total depth ≈ 1/3 of the mark
   width, with enough layers (~40-50) that the side wall has NO comb gaps — a
   literal 1px×i step reads as a flat stroke edge, not a bevel. (For a
   wordmark, extrude each letter the same way, or extrude one monogram.)
3. transform-style: preserve-3d on the group so the whole stack rotates as
   one solid object.

Animation timeline (~5s):
- 0.0-1.6s  the logo flies in spinning: rotationY from -220° → 0 and
            rotationX from 30° → 0, translateZ from -600 → 0, opacity 0→1,
            ease power2.out (NOT power3 — power3 is so front-loaded the turn
            snaps face-on in ~0.1s and the side-face reveal flashes past;
            power2.out keeps it turning through the 3/4 view). Mid-spin you
            SEE the extruded side faces — that's the payoff, the thickness
            catches light.
- 1.6s      settle: a tiny overshoot past 0° then back (rotationY 0→8°→0,
            0.4s) — inertia.
- 1.7s      a specular highlight bar sweeps diagonally across the face
            (skewed light gradient, x -120%→220%, 0.5s) as it locks front-on.
- 1.8-5s    hold: a slow idle rotation keeps it 3D-alive — rotationY -6°→6°
            and rotationX -3°→3° (sine yoyo, finite repeats) so the light
            plays across the bevel; soft contact shadow on the floor tracks
            the tilt.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; finite repeats; root
div with data-composition-id="main" data-duration="5" data-width="1920"
data-height="1080".
```

## 要点 Key technique notes

- **Extrusion = stacked Z-copies.** ~40-50 copies of the mark stepped back on `translateZ` to a total depth ≈ 1/3 the mark width, all under one `preserve-3d` parent, reads as a solid beveled object when it rotates. Too few/shallow copies (a 1px step) combs into a flat edge. No 3D engine required.
- The spin must pass through an angle where the side faces show (start ~-220° Y + 30° X) — the thickness catching light IS the reveal; a flat face-on fade wastes the technique.
- The idle rotation on the hold is essential: a motionless 3D logo looks like a flat sticker again. Keep ±6° of life so the bevel shimmers.
- For heavy wordmarks, extrude per-letter or use a single monogram — extruding a long thin wordmark gets expensive (many stacked copies × letters).
