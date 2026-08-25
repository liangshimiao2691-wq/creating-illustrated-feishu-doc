[中文](README_CN.md) · [Skill specification](SKILL.md)

# Creating Illustrated Feishu Docs

> Turn videos, meetings, slides, transcripts, and screenshots into source-faithful Feishu/Lark documents where each visual is followed by the prose it explains.

![Xiaohongshu video to illustrated Feishu document](assets/xiaohongshu-to-feishu.png)

## Real case study

Input: a 2m49s public Xiaohongshu video, [“How to find vibe-coding product ideas”](http://xhslink.cn/o/5KiCbQ9MxCZ), by **张咋啦**.

The workflow:

1. resolves the public share link and extracts title, author, page captions, and video metadata;
2. selects nine frames that materially advance the narrative;
3. aligns each frame with the transcript passage being spoken;
4. removes filler, repetition, and reader-facing timestamps while preserving claims, examples, technical terms, and causal order.

The result is a shareable Feishu document titled “如何想出 vibe coding 的产品灵感｜Zara 风格图文版.” Readers can understand the method without replaying the full video.

![Privacy-safe reconstruction of the final Feishu document](assets/feishu-output-safe.png)

> The image above is a privacy-safe reconstruction using the real final title, prose, and keyframe. Account identity, browser tabs, desktop content, and temporary media URLs are excluded.

## What it solves

| Raw-material problem | Skill behavior | Reader outcome |
| --- | --- | --- |
| Long video, little time | Extract captions and meaningful keyframes | Scan the core argument in minutes |
| Visuals and narration are separated | Align by timeline and semantics | Each image is followed by its explanation |
| Cue-level transcript is fragmented | Light cleanup and paragraph merging | Readable prose instead of ASR output |
| Generic summaries lose evidence | Retain Deck pages, UI states, and video frames | Claims remain traceable to the source |
| Signed media URLs may leak | Keep download/upload parameters internal | No temporary token in the deliverable |

## Default document pattern

```text
Topic and reading guide

Section heading
Visual takeaway
[keyframe / slide / screenshot]
one complete matching transcript paragraph

next section…
```

By default the workflow:

- hides cue-level timestamps and placeholder labels;
- removes filler, false starts, repeated fragments, and obvious ASR noise;
- merges adjacent cues into natural paragraphs;
- preserves facts, names, numbers, technical terms, examples, stance, and causal order;
- keeps timing internally for alignment and verification;
- prevents temporary media tokens from reaching captions or prose.

Ask for “逐字稿”, “字幕”, “时间轴”, or “按秒对应” when timestamped cue-level output is required.

## Supported inputs

- Feishu Minutes, meeting notes, transcripts, recordings;
- local or publicly accessible video and audio;
- PPT/PPTX, Feishu Slides, HTML decks, PDFs;
- screenshots, images, captions, subtitles, and plain text;
- public Xiaohongshu share links, including `xhslink.cn` short links.

## Xiaohongshu handling

The agent follows the public redirect itself and inspects the public note state for title, author, poster, video stream, and page-provided captions. Captions become the narrative source and representative frames become visual evidence.

Redirect parameters and signed media URLs remain internal. The workflow does not depend on unrelated third-party downloader services.

## Usage

```text
Use the illustrated Feishu doc Skill on this Xiaohongshu link.
Put one cleaned paragraph after every meaningful visual and hide timestamps.
```

```text
Turn this meeting recording, PPT, and Feishu Minutes into a Zara-style illustrated document.
Preserve key numbers, decisions, examples, and the explanation for each slide.
```

## Install

```bash
git clone https://github.com/liangshimiao2691-wq/creating-illustrated-feishu-doc.git ~/.claude/skills/creating-illustrated-feishu-doc
```

For another agent, place `SKILL.md`, `references/`, and `agents/` in that agent's Skill directory.

A real Feishu document requires a host Agent with Feishu/Lark document access and an authenticated user identity. Without that capability, the workflow can fall back to local Markdown or HTML.

## Principles

- source-faithful, not a generic summary;
- visual evidence before matching explanation;
- light oral cleanup, not aggressive rewriting;
- timestamps retained internally for alignment and verification;
- no fabricated slide text, transcript, speaker identity, or visual interpretation;
- public sources preserved, temporary access parameters protected.

## License

MIT
