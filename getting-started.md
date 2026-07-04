# 3 步跑起来 · Getting Started

这些 prompt 生成的是 **HTML 动画**（GSAP 时间轴），用 [HyperFrames](https://www.npmjs.com/package/hyperframes) 渲染成视频。你不需要会写代码 —— 写代码的是你的 coding agent。

## 你需要 What you need

- Node.js 18+（`node --version` 检查）
- 一个 coding agent：Claude Code / Codex / Cursor 都行
- 一个浏览器（渲染用的 Chromium，HyperFrames 会自己搞定）

## 3 步 The 3 steps

```bash
# 1. 新建一个项目
mkdir my-animation && cd my-animation
npx hyperframes init
```

**2. 把 prompt 交给你的 coding agent。** 打开你想要的条目（比如 [logo/01-stroke-glow-reveal.md](logo/01-stroke-glow-reveal.md)），复制 ```text 代码块里的整条 prompt，把 `{占位符}` 换成你自己的品牌/文案/数据，然后原样贴给 agent。它会写出 `index.html`。

```bash
# 3. 检查 + 预览 + 渲染
npx hyperframes lint      # 0 errors 再继续
npx hyperframes preview   # 浏览器里目检一遍
npx hyperframes render    # → renders/*.mp4
```

## 提示 Tips

- **别删 prompt 里的 "Render safety" 段落** —— 那段保证动画每一帧都能被渲染器精确定位（暂停的时间轴、无随机数）。删了会渲染出错帧。
- 中文文案记得给 agent 本地字体文件（woff2），CDN 字体渲染时可能加载不出来。
- 效果不对？把 GIF 发给 agent 说"对着这个改" —— GIF 就是验收标准。
- 每个条目底部的 **要点 Key technique notes** 是踩坑提醒，agent 跑偏时用它纠正。

## English

These prompts produce **HTML animations** (GSAP timelines) rendered to video by [HyperFrames](https://www.npmjs.com/package/hyperframes). You don't write the code — your coding agent does.

1. `mkdir my-animation && cd my-animation && npx hyperframes init`
2. Copy the ```text prompt block from any entry, fill the `{PLACEHOLDERS}` with your brand/copy/data, paste it to your coding agent (Claude Code / Codex / Cursor). It writes `index.html`.
3. `npx hyperframes lint` (until 0 errors) → `npx hyperframes preview` → `npx hyperframes render` → `renders/*.mp4`.

Keep the "Render safety" paragraph in every prompt — it's what makes the animation deterministic and frame-accurate under the renderer. If the result drifts from the GIF, show the GIF to your agent and say "match this."
