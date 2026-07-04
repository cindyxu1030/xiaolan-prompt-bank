# 字幕动效 · Caption Motion Graphics — 什么时候用哪一档

字幕动效是分**档位**的。全片同一个强度 = 没有强度。一条口播视频的正确配比:

| 档位 | 条目 | 用在哪 | 一条视频用几次 |
|---|---|---|---|
| 默认档 | [逐字卡拉OK](02-word-pop-karaoke.md) | 整条视频的常规台词，跟着人声逐字点亮 | 全程 |
| 重音档 | [巨字重锤](01-hero-slam.md) | 金句/数字/结论 — 值得钉进观众脑子的那句 | 2–4 次 |
| 空间档 | [人后穿字](03-behind-subject-ghost.md) | 章节转场、氛围词 — 给画面加纵深 | 1–2 次 |
| 特效档 | [RGB 故障字](04-rgb-glitch-caption.md) | 冲突/警告/科技感的那一拍 | **1 次** |
| 海报档 | [手写海报叠字](05-script-poster-stack.md) | 开场定调或结尾金句 — 一帧变海报 | 1 次 |

## 通用法则（不分档位）· Universal rules

来自对 johnbucog 风格的逐帧拆解 — 这五条是所有字幕动效的地基:

1. **锁人声。** 每个字在它被说出的那一帧动作。先转写拿到词级时间戳，再对着时间戳做动画 — 目测对轴一眼假。
2. **尺寸反差 2.5–4 倍。** 一个巨字主角 + 小字铺垫。全都大 = 全都不大。
3. **两种颜色封顶。** 底色 + 一个重音色，重音色只给那一个要被记住的词。
4. **重锤 4–6 帧。** `scale 1.16→1.0, power4.out` — 再慢就是淡入，不是砸。
5. **落地不等于静止。** 每个主角字都保持微呼吸（≤4.5% scale）— 静止的字是贴图，呼吸的字是角色。
6. **脸是圣域。** 字可以裁出画框边缘（出血反而高级），永远不能盖脸。

## English — register guide

Caption styles are **registers**, not decorations — one intensity all video long means no intensity at all. Word-pop karaoke is the running default; the hero slam marks 2–4 quotable beats; behind-subject ghost adds depth once or twice; RGB glitch fires exactly once; the script-poster stack opens or closes the video. Universal rules from the johnbucog frame-by-frame breakdown: animate on word-level VO timestamps, 2.5–4× scale contrast, two colors max (one accent on the one word that matters), the punch lives in 4–6 frames (`1.16→1.0 power4.out`), nothing ever sits fully still, and text may bleed frame edges but never covers the face.
