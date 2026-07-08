<!--
投稿模板 · Entry template — copy this file to <category>/<next-number>-<your-slug>.md
Fill every {PLACEHOLDER}, delete these comment lines, then render your GIF from the prompt.
See CONTRIBUTING.md for the rules. The `../` in the preview path is correct (entries live one folder deep).
-->

# {中文名} · {English Name}

![preview](../assets/gifs/{your-slug}.gif)

**效果:** {一句话说清这条 prompt 交付什么效果}。
*What it delivers: {one sentence — what this prompt produces}.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a {WIDTH}x{HEIGHT} HyperFrames composition — a {DURATION}-second {WHAT}
on {BG}.

{Describe the elements to build, with {PLACEHOLDERS} for brand/logo/data/color.}

Animation timeline (~{DURATION}s):
- {beat by beat: what enters when, the ease, the payoff moment}

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"]; all motion tweened on the timeline; any
"randomness" derived from index/trig (no Math.random); no Date.now; finite
repeat counts; root div with data-composition-id="main" data-duration="{DURATION}"
data-width="{WIDTH}" data-height="{HEIGHT}".
```

## 要点 Key technique notes

- {2–4 bullets: the non-obvious craft — the one thing a builder gets wrong, the reason a value matters, the "sell" of the effect}
