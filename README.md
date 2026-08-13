English | [中文](README_CN.md)

# Creating Illustrated Feishu Docs

Turn videos, meeting recordings, Feishu Minutes, PPT/Decks, HTML slides, PDFs, images, audio transcripts, and notes into a Zara-style illustrated Feishu document:

> one meaningful visual → one readable matching video稿 paragraph → next visual → next paragraph

## Why this exists

Normal transcript tools produce a wall of timestamped subtitles. Normal summarizers lose the speaker's reasoning and the visual evidence. This skill connects the two: it preserves what was said, shows the slide or video frame being discussed, and turns fragmented speech into readable prose.

## What it solves

- A video has a speaker, slides, captions, and screen actions but no usable written artifact.
- PPT pages and the spoken explanation are separated, so readers cannot tell which words belong to which page.
- Timestamp-heavy transcripts are hard to read and share.
- Xiaohongshu share links often look inaccessible or only expose a short link.
- Meeting materials are scattered across Minutes, recordings, Decks, screenshots, and notes.

## Supported inputs

- Feishu Minutes, meeting notes, and transcript documents
- Local or publicly accessible video and audio
- PPT/PPTX, Feishu Slides, HTML decks, and PDF pages
- Screenshots, images, captions, subtitles, and plain text
- Xiaohongshu share links, including `xhslink.cn` links when the public note is accessible

## Default output

The default is a readable prose document, not a raw subtitle dump:

- timestamps and per-cue labels are hidden;
- filler words, false starts, repeated fragments, and obvious ASR noise are lightly cleaned;
- adjacent transcript cues are merged into natural paragraphs;
- facts, names, numbers, technical terms, examples, stance, and causal order are preserved;
- each visual is placed next to the paragraph it explains;
- captions describe the visual without exposing temporary media tokens.

Ask for “逐字稿”, “字幕”, “时间轴”, or “按秒对应” when timestamped cue-level output is desired.

## Xiaohongshu handling

The agent resolves the share link itself. It uses a normal GET request with redirect following, then inspects the public note page state for the title, author, poster, signed video stream, and page-provided subtitles. It uses the subtitles as the narrative source and representative video frames as visual evidence when available.

Temporary redirect parameters and signed media URLs remain internal. The workflow does not depend on unrelated third-party downloader services.

## Install

### Claude Code / compatible agents

```bash
git clone https://github.com/liangshimiao2691-wq/creating-illustrated-feishu-doc.git ~/.claude/skills/creating-illustrated-feishu-doc
```

For another agent, place the repository's `SKILL.md` and `references/` folder in that agent's skill directory.

### Feishu output

For a real Feishu document, the agent needs access to the Feishu/Lark document capability and an authenticated `lark-cli` user identity. If those are unavailable, the skill can still produce a local Markdown/HTML fallback when the host agent supports file output.

## Usage examples

```text
Use the illustrated Feishu doc skill on this Xiaohongshu link.
```

```text
Turn this meeting recording, PPT, and transcript into a Zara-style Feishu document.
```

```text
Make a visual transcript: show each slide, then the cleaned paragraph explaining it.
```

## Design principles

- source-faithful, not a generic summary;
- visual evidence before the matching explanation;
- light oral cleanup, not aggressive rewriting;
- timestamps retained internally for alignment and verification;
- no fabricated slide text, transcript, speaker identity, or visual interpretation;
- public source links preserved, temporary access tokens kept private.

## Search keywords

Illustrated Feishu document, Lark document, Zara-style visual transcript, Feishu Minutes, meeting transcript, video transcript, slide transcript, PPT narration, Deck explanation, HTML deck, Xiaohongshu video, xhslink, subtitles, keyframes, image-text layout.

## License

MIT
