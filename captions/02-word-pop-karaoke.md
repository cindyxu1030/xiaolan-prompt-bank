# 逐字卡拉OK · Word-Pop Karaoke Caption

![preview](../assets/gifs/word-pop-karaoke.gif)

**效果:** 整句待命，说到哪个词哪个词弹起变色，重点词带手绘下划线 — 跟着人声逐字点亮的"进行中"字幕，观众的眼睛被声音牵着走。
*What it delivers: the whole line stands by, and each word pops and recolors the instant it's spoken — key words get a hand-drawn underline. A VO-locked "live" caption that leads the viewer's eye by the voice.*

## Prompt（复制给你的 coding agent · copy-paste to your coding agent）

```text
Create a 1080x1920 vertical HyperFrames composition — a karaoke caption layer
for a talking-head clip (transparent background for compositing, or over the
provided footage).

The transcript with word-level timestamps: {PASTE — get them by transcribing
the VO with whisper (word timestamps on)}.
Colors: base text {BASE e.g. charcoal #1F1F1D}, active word {ACTIVE e.g.
#002FA7}, underline {UNDERLINE e.g. mint #8ACAB1}.
Emphasis words (stay recolored + underlined after spoken): {LIST 2–4 words}.

Structure the data as one JS array of phrase objects — one phrase per natural
speech pause: [{ t: startSec, end: endSec, words: [{w:"别", t:1.02},
{w:"囤", t:1.18, u:1}, ...] }] where u:1 marks emphasis words. Build all
phrase DOM up front; the timeline drives visibility.

Layout rules:
- One short line per speech pause — when the speaker breathes, the line swaps.
- The line sits in a fixed band (e.g. y ≈ 55–65% height), centered, max-width
  ~85% — auto-fit: measure the line; if it overflows, step the font down.
- Font: bold 56–72px, generous letter-spacing for CJK readability.
- If over footage on a light background, dark text — white text on light bg is
  invisible. Add nothing behind the text (no pill, no scrim) unless the bg is
  busy; then a soft 30% scrim.

Animation (all VO-locked):
- Phrase swap at each phrase's t: old line drops out (y +14, opacity→0, 0.18s),
  new line rises in (y 14→0, opacity 0→1, 0.22s power2.out).
- WORD POP at each word's t: the word scales 1.0→1.13→1.0 (0.16s total,
  back.out(2)) and recolors base→ACTIVE for its spoken duration, then returns
  to base — except emphasis words (u:1), which STAY in ACTIVE and get a
  hand-drawn underline: an SVG squiggle path under the word, drawn on via
  strokeDashoffset (0.25s) right as the word pops.
- Nothing else moves. The pops are the rhythm; extra motion muddies it.

Render safety (required): one single paused GSAP timeline on
window.__timelines["main"] — every pop is a tween AT its word's absolute
timestamp on that timeline (fromTo with immediateRender:false); no Date.now /
Math.random; root div with data-composition-id="main"
data-duration="{CLIP_LENGTH}" data-width="1080" data-height="1920".
```

## 要点 Key technique notes

- **Word-level timestamps are the whole style.** Transcribe first (whisper with word timestamps), animate to those numbers — eyeballing sync reads instantly as "off."
- One phrase per breath pause. Cramming two spoken phrases into one visual line kills the rhythm.
- Absolute-timestamp `fromTo` tweens (not chained/relative) keep every word exactly on its beat and keep the timeline seekable.
- Emphasis words persist their color + underline — by the end of the phrase the viewer sees the line's skeleton highlighted.
