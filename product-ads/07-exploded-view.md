# 爆炸拆解广告 · Exploded-View Reassembly

![preview](../assets/gifs/exploded-view.gif)

**效果:** 产品的零件沿轴线炸开悬浮，每层贴上材质/技术标注，然后"啪"地严丝合缝复原 — 讲"里面有什么"的工程感镜头。
*What it delivers: the product's parts blow apart along an axis and hover with material/tech labels, then snap precisely back together — the engineering shot that says "here's what's inside."*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 7-second exploded-view
product ad on {BG — clean light #F4F1EA or dark studio}.

My product built in LAYERS: {LIST 3-5 stacked parts from front to back —
e.g. "glass front / screen / logic board / battery / back shell", or for a
shoe "laces / upper / cushion / outsole"}. Build each as a CSS/SVG plane or
attach a PNG per layer. Accent: {ACCENT}. Per-layer labels: {LABEL each part}.

Build the stack:
- The parts assembled = stacked on a shared center with slight perspective
  (perspective: 1600px, each part at its own translateZ). Assembled, they
  read as one solid product.
- Each part can separate along the explosion AXIS (usually Z toward camera,
  or a vertical fan) to its own offset.

Animation timeline (~7s):
- 0.0-0.6s  assembled product rises in (y 40→0, scale .94→1).
- 0.8-2.2s  EXPLODE: parts separate along the axis, staggered 0.12s apart,
            each easing to its offset (power2.out) with a slight rotation
            for readability; thin connector lines draw between adjacent
            parts (strokeDashoffset) showing the assembly order.
- 2.2-4.6s  inspect: each part's label wipes in beside it (dot + line, clip
            reveal), one at a time; the whole exploded stack does a slow
            unified parallax tilt (±4°) so the depth reads.
- 4.6-5.6s  REASSEMBLE: parts fly back to zero in reverse-ish order,
            accelerating (power2.in), and SNAP together on the last frame
            with a unified settle punch (scale 1.03→1) + a bright seam-flash
            where they meet.
- 5.6-7s    hold: assembled product front-and-center, one light sweep, a
            tagline/logo lockup fades in.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; no Date.now / Math.random; finite repeats; root
div with data-composition-id="main" data-duration="7" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Author the product as separable layers up front** — the whole technique is the parts having their own offsets along one explosion axis (usually Z toward camera). Assembled = all offsets zero.
- Connector lines drawing between adjacent parts (strokeDashoffset) communicate assembly order and make it read as engineering, not just floating pieces.
- The reassembly ACCELERATES (`power2.in`) into a hard snap + seam-flash — the "precision-built" payoff. Exploding is setup; the snap is the sell.
- Stagger both the explode (0.12s apart) and the label reveals — showing all parts/labels at once is noise; sequence tells a story.
