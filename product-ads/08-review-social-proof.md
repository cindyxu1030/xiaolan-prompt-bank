# 好评口碑广告 · Review Social-Proof Stack

![preview](../assets/gifs/review-social-proof.gif)

**效果:** 五星一颗颗点亮、评分数字滚到 4.9，用户好评卡片依次滑入叠起，头像墙铺开 — 三秒建立"大家都在用"的信任。
*What it delivers: stars light up one by one, the rating counts to 4.9, review cards slide in and stack, an avatar wall spreads — three seconds of "everyone's using this" trust.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a 6-second social-proof
review ad on {BG — clean light #F7F5F0 or brand dark}.

Product/brand: {NAME}. Headline rating: {RATING e.g. 4.9} from {COUNT e.g.
12,000+} reviews. 3 short review quotes + reviewer first-names (make them
generic/fictional): {REVIEW_1 "…" — Name / REVIEW_2 / REVIEW_3}. Accent:
{ACCENT}. Star color: gold #FFC53D.

Build the elements:
- Top: a big star row (5 stars) + the rating number (huge, tabular-nums) +
  "{COUNT} 条好评" subline.
- Middle: 3 review cards (rounded, soft shadow, hairline) each with a small
  avatar circle (initial or generated gradient orb), the 5-star mini row,
  the quote, and the name.
- Bottom: an avatar WALL — a grid/cluster of ~15-24 small circle avatars
  (gradient orbs, deterministic colors by index) implying the crowd.

Animation timeline (~6s):
- 0.0-1.0s  stars light up left→right (each: scale 0→1.2→1, back.out, gold
            fill sweeps in), 0.12s apart; the rating number counts up 0→4.9
            (power2.out) landing with a 1.1 punch; subline fades in.
- 1.2-3.6s  review cards slide/stack in from alternating sides (x ∓120,
            0.5s expo.out), 0.5s apart; each card's mini-stars ping and its
            quote does a quick per-line reveal.
- 3.8-4.8s  avatar wall pops in: circles scale 0→1 in a staggered ripple
            from center (back.out, delay = distance-from-center); a small
            "+{COUNT}" chip stamps on.
- 4.8-6s    hold: everything breathes subtly; one gold shimmer sweeps the
            star row; optional CTA/logo lockup fades in.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; avatar colors + ripple delays derived from index
(no Math.random); count-up tweens a plain object; no Date.now; finite repeats;
root div with data-composition-id="main" data-duration="6" data-width="1080"
data-height="1920".
```

## 要点 Key technique notes

- **Sequence the trust cues:** stars → rating count-up → review cards → avatar wall. Each layer stacks proof; dumping them all at once reads as an ad, building them reads as momentum.
- `font-variant-numeric: tabular-nums` on the rating (and count-up a plain object on the timeline) — a 4.9 that jitters mid-count undercuts the credibility.
- Avatar orbs get deterministic gradient colors by index (e.g. hue = `i * 37 % 360`), never random — keeps the render stable and the wall colorful.
- Use obviously-rounded fictional numbers ("12,000+") and generic first-names — it's a template demo, not a claim.
