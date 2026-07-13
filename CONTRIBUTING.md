# 投稿指南 · Contributing

欢迎投稿你自己的动效 prompt！这个库的唯一承诺是：**每条 prompt 旁边的 GIF，就是这条 prompt 真实渲染出来的效果**。所以投稿的核心不是"写一段听起来很厉害的 prompt"，而是**证明它能跑出那个效果** —— 你先写 prompt，再照着它构建、渲染、录成 GIF，两者必须对得上。

> English below · 英文版见下方 [## English](#english)

---

## 投稿长什么样 · What a submission is

一个投稿 = **一个 `.md` 条目页 + 一张 GIF**，放进对应分类文件夹：

| 分类 | 文件夹 | 画幅 |
|---|---|---|
| Logo 动画 | `logo/` | 通常 16:9 (1920×1080) |
| 产品广告 | `product-ads/` | 9:16 (1080×1920) |
| 数据动画 | `data/` | 9:16 (1080×1920) |
| 字幕动效 | `captions/` | 9:16 (1080×1920) |
| 发布片场景 | `trailer/` | 16:9 (1920×1080) |

- 文件命名：`<下一个序号>-<英文-slug>.md`（看该文件夹现有最大序号 +1），例如 `logo/09-holographic-tilt.md`。
- GIF 命名：`assets/gifs/<英文-slug>.gif`（和条目页同名，不带序号）。

条目页用 [`ENTRY-TEMPLATE.md`](ENTRY-TEMPLATE.md) 直接复制改写。

## 六条硬规则 · The 6 hard rules

投稿必须满足这六条，否则不会合并：

1. **GIF 必须由这条 prompt 渲染出来。** 先写 prompt → 交给你的 coding agent 照着构建 → 渲染 → 录 GIF。不要先做动画再倒推 prompt——那样 prompt 和效果会对不上，破坏整个库的信任。

2. **确定性渲染（Render safety）。** 动画必须能被逐帧精确定位：单条暂停的 GSAP 时间轴挂在 `window.__timelines["main"]`，所有运动都 tween 在时间轴上，**禁用 `Math.random` / `Date.now` / 无参 `new Date()`**（随机/时间会让每次渲染不一样，渲染器会出错帧）。需要"随机感"就用 index/三角函数写死。根节点带 `data-composition-id="main"` + `data-duration/width/height`。

3. **不放任何真实品牌 / 真实数据 / 个人信息。** 用虚构品牌名（NOVA、AURA、FLUX…）和假数据。不要放真实公司 logo（商标风险）、真实用户数据、真人出镜素材、真实邮箱/用户名/密钥/本地路径。

4. **prompt 自成一体、带占位符。** 别人复制你的 prompt，把 `{YOUR_LOGO}` `{ACCENT}` `{DATA}` 换成自己的就能用。prompt 里要包含那段 "Render safety (required): ..." 说明——它是效果可复现的保证。

5. **GIF 规格。** 两遍调色板（palettegen/paletteuse）、12fps、3–6 秒最佳片段；16:9 宽 480px，9:16 宽 360px；**单个 ≤ 8MB**（大多数应在 0.3–4MB）。命令见下。

6. **保留 Obsidian/品牌无关**——这是一个通用动效库，不是某个品牌的私料。字幕类投稿请在 [`captions/README.md`](captions/README.md) 的档位表里加一行说明"什么时候用这一档"。

## 怎么录 GIF · Making the GIF

用 ffmpeg，两遍调色板质量最好：

```bash
# 9:16 竖屏（产品/数据/字幕）
ffmpeg -ss <起始秒> -t 5 -i render.mp4 -filter_complex \
 "fps=12,scale=360:-2:flags=lanczos,split[a][b];[a]palettegen=stats_mode=diff[p];[b][p]paletteuse=dither=bayer:bayer_scale=4:diff_mode=rectangle" \
 -y assets/gifs/<你的-slug>.gif

# 16:9 横屏（logo）把 scale=360 改成 scale=480
```

选那段最能体现动画精华的 3–6 秒（重锤那一下、名次反超那一刻），不用放完整时长。

## 提交流程 · How to submit

1. Fork 这个仓库。
2. 加你的 `<分类>/<序号>-<slug>.md` + `assets/gifs/<slug>.gif`。
3. 在对应分类的 README 表格里加一行（GIF 缩略图 | 名称链接 | 一句话效果）。字幕类再更新档位表。
4. 自查 [PR 清单](#pr-清单--pr-checklist)。
5. 提 Pull Request，标题写清楚加的是哪条、哪个分类。

Cindy 会评审：先看 GIF 是否真由 prompt 跑出、是否满足六条规则，再看动画质量。

## PR 清单 · PR checklist

- [ ] GIF 是照着我提交的 prompt 构建渲染出来的（不是倒推）
- [ ] 无 `Math.random` / `Date.now` / `new Date()`；单条暂停时间轴挂在 `window.__timelines["main"]`
- [ ] 无真实品牌 logo / 真实数据 / 个人信息 / 真人素材
- [ ] prompt 自成一体、带 `{占位符}`、包含 "Render safety" 段
- [ ] GIF ≤ 8MB，12fps，画幅正确
- [ ] 在分类 README 加了一行（字幕类也更新了档位表）
- [ ] 文件命名 = 该文件夹下一个序号 + 英文 slug；GIF 同名

## 授权 · License

投稿即表示你同意以本库的 [CC BY-NC-SA 4.0](LICENSE) 授权发布你的贡献。你用 prompt 渲染出的成片仍归你自己。

---

## English

Thanks for contributing your own motion-graphics prompt! This bank has one promise: **the GIF beside each prompt is what that prompt actually renders.** So a submission isn't "a cool-sounding prompt" — it's a prompt you've **proven** by building, rendering, and recording it. Write the prompt first, then build the GIF from it; the two must match.

### What a submission is

One submission = **one `.md` entry page + one GIF**, dropped into the matching category folder: `logo/` (usually 16:9), `product-ads/` / `data/` / `captions/` (9:16), `trailer/` (16:9). Name the page `<next-number>-<english-slug>.md` (look at the folder's highest number, +1); name the GIF `assets/gifs/<english-slug>.gif` (same slug, no number). Copy [`ENTRY-TEMPLATE.md`](ENTRY-TEMPLATE.md) to start.

### The 6 hard rules

1. **The GIF must be rendered FROM this prompt.** Write the prompt → hand it to your coding agent → build → render → record the GIF. Don't reverse-engineer a prompt from an existing animation — they'll drift apart and break the bank's trust.
2. **Deterministic render.** One paused GSAP timeline on `window.__timelines["main"]`, all motion tweened on it, **no `Math.random` / `Date.now` / argless `new Date()`** (randomness/time make every render differ and produce bad frames). Derive any "randomness" from index/trig. Root node carries `data-composition-id="main"` + `data-duration/width/height`.
3. **No real brands / real data / personal info.** Fictional brand names + fake data. No real company logos (trademark), real user data, real-person footage, or real emails/usernames/keys/local paths.
4. **Self-contained prompt with placeholders.** Someone swaps `{YOUR_LOGO}` `{ACCENT}` `{DATA}` for their own and it works. Keep the "Render safety (required): …" paragraph — it's the reproducibility guarantee.
5. **GIF spec.** Two-pass palette, 12fps, best 3–6s window; 480px wide for 16:9, 360px for 9:16; **≤ 8MB each** (most land 0.3–4MB). Command above.
6. **Keep it brand-neutral** — this is a general library, not one brand's private assets. For caption submissions, add a row to the register table in [`captions/README.md`](captions/README.md) saying when to use it.

### How to submit

Fork → add your `<category>/<number>-<slug>.md` + `assets/gifs/<slug>.gif` → add a row to the category README table (GIF thumb | linked name | one-line effect) → self-check the PR checklist → open a PR naming what you added and its category. Review order: is the GIF genuinely rendered from the prompt, does it meet the 6 rules, then animation quality.

By contributing you agree to license your contribution under this repo's [CC BY-NC-SA 4.0](LICENSE). Videos you render from prompts remain yours.
