---
name: creating-illustrated-feishu-doc
description: Use when the user wants Feishu Minutes, meeting notes, transcripts, video, audio, PPT, HTML slides, images, PDFs, or text turned into a richly interleaved image-text Feishu document, especially a slide/frame followed by its matching spoken explanation.
---

# Creating an Illustrated Feishu Document

## Purpose

Turn heterogeneous meeting or presentation materials into a readable Feishu document that preserves the source narrative while placing the right visual evidence beside the right passage. The target is Zara's “图文版逐字稿” pattern: section heading → Deck/video frame → matching transcript → next frame → next transcript, with useful captions and provenance.

This is an orchestration skill. Route source retrieval and document writes through the installed Feishu skills; do not assume that `frontend-slides` alone performs transcript-to-document alignment.

## Trigger and output contract

Use this skill when the user asks for a “图文版逐字稿”, “图文混排飞书文档”, “把妙记和 PPT 合成文档”, “把视频整理成图文稿”, or equivalent.

Deliver:

1. A source ledger: every input, type, access status, and role.
2. An alignment plan: section, visual evidence, transcript range, confidence, and provenance.
3. A new or explicitly targeted Feishu document containing the interleaved result.
4. A short verification note stating missing materials, uncertain matches, and what was checked.

Do not silently return a conventional summary when the user asked for the illustrated transcript. A summary can be an optional short introduction, never a replacement for the preserved transcript.

### Default audience-facing prose mode

Unless the user explicitly asks for a verbatim transcript, subtitles, timestamps, or a timeline, produce the readable Zara-style prose version:

- Put one meaningful visual first, followed by one continuous paragraph of the matching video稿. Split into two short paragraphs only when the topic genuinely changes.
- Remove per-sentence timestamps, timestamp ranges, subtitle cue labels, and decorative “插图占位” text from the visible document. Keep timestamps in the internal alignment map and verification notes.
- Remove obvious fillers, false starts, repeated fragments, and certain speech-recognition artifacts such as “嗯/啊/诶/那个/然后” when they do not carry meaning. Join adjacent cues into natural sentences.
- Preserve facts, numbers, names, technical terms, causal relationships, stance, examples, and meaningful repetition. This is light oral cleanup, not a summary or a formal essay rewrite.
- Keep image captions descriptive by default (what the frame shows), without a visible second-level timestamp unless the user asks for one.

If the user asks for “逐字稿/原话/字幕/时间轴/按秒对应”, switch back to timestamped cue-level output and preserve the requested temporal labels.

## Route materials to the right capability

Use the existing installed skills as follows:

- Feishu Minutes or a local audio/video file whose transcript is needed: read and follow `lark-minutes`; local audio/video transcription goes through Feishu Minutes, not local Whisper/ffmpeg ASR.
- A finished meeting note or related document: use `lark-note` or `lark-doc` as appropriate.
- Feishu document, file, image, attachment, or media: use `lark-doc` and `lark-drive`.
- Feishu chat links or missing source links that the user asks you to locate: use `lark-im` with the authorized user's identity and a bounded search.
- Feishu Slides: use `lark-slides`; PPT/PDF/HTML files: use the relevant document/presentation/PDF capability to render or extract visual pages.
- HTML slide creation is out of scope unless separately requested; use `frontend-slides` only for that subtask.

Read the referenced Feishu skill instructions before using its CLI. For `lark-doc` writes, obey its required XML/style/create-workflow references and prefer its media insertion shortcut for local images.

### Xiaohongshu link handling

For a user-provided Xiaohongshu share link, resolve it yourself before asking the user to copy a long URL:

1. Use a real HTTP GET with redirect following. Do not rely on a HEAD request, which may fail even when the share link is usable.
2. Treat the redirected long URL and its temporary access parameters as internal processing data; do not paste temporary tokens into the final document or public README.
3. Fetch the public note HTML and inspect its embedded page state for the title, author, description, poster, signed video stream, and subtitle URLs. Prefer the page-provided Chinese subtitles as the narrative source when available, and use representative video frames as visual evidence.
4. If the page or media cannot be accessed, report the exact missing material and continue only with the modalities that are actually available. Do not use unrelated third-party downloader services or claim the video was processed without evidence.

This path is especially useful for `xhslink.cn` share links that appear broken in ordinary browser previews but still return a valid redirect and public note state through a normal GET.

## Workflow

### 1. Intake and source ledger

Collect every material the user supplied or explicitly authorized you to find. Classify each item:

| Role | Examples | Use |
|---|---|---|
| Narrative source | Minutes transcript, meeting note, speaker transcript, audio transcript | Source of truth for what was said |
| Visual source | PPT, Feishu Slides, HTML deck, PDF pages, images | Page/slide evidence and section structure |
| Temporal visual source | Video recording, screen recording | Keyframes and demonstration state |
| Context source | Chat message, title, agenda, speaker list | Names, order, metadata, provenance |

Record access failures and do not fill gaps with guesses. If the user has provided only one modality, continue with that modality and disclose the fallback.

### 2. Extract, normalize, and preserve

- Fetch the full transcript when available, including timestamps and speakers. Do not use an AI summary as a substitute for the transcript.
- Fetch timestamps for alignment even when they will be hidden in the final prose document. Normalize only obvious filler words, clear verbal slips, and transcription errors that are certain from context. Preserve facts, numbers, names, quotations, claims, stance, order, and meaningful repetition.
- In the default prose mode, merge adjacent transcript cues into paragraph-level video稿 after normalization. Do not turn every subtitle cue into a separate paragraph.
- Extract visual pages at their native aspect ratio. Prefer the original Deck/slide page over a screenshot of a browser frame when both exist.
- For video, read `references/video-frame-policy.md` before selecting frames.
- Keep a stable source ID for each visual: `slide-N`, `video-HH:MM:SS`, `image-N`, or `page-N`.
- Track transcript coverage as source intervals. Every source interval must be either present in the output or explicitly listed as intentionally omitted with a reason; a visual alignment failure is not a reason to delete the text.

### 3. Build the alignment map before writing

Never write the document directly from an unaligned transcript. Create an internal map with:

```text
section_title
  visual_id + source/page/timestamp
  transcript_start/end + speaker(s)
  relation: explains | demonstrates | introduces | compares | context-only
  confidence: high | medium | low
  notes: corrections, omissions, or unresolved ambiguity
```

Align in this order:

1. Explicit slide/page changes or timestamps.
2. Matching titles, on-screen text, named examples, and spoken anchors.
3. Chronology, speaker turns, and nearby context.
4. Semantic similarity only as a tie-breaker, never as permission to invent.

When a passage spans multiple slides, attach the passage to the first relevant visual and add the next visual at the point where the topic changes. When one slide has multiple spoken segments, repeat the slide only if doing so materially improves navigation; otherwise use a caption such as “继续讲解”.

### 4. Choose the layout mode

Choose one mode per section, not one rigid template for the whole document:

- **Deck + transcript:** section heading → slide image → matching transcript → next slide.
- **Video demonstration:** section heading → representative screen frame → descriptive caption → matching prose video稿; add a small speaker/avatar frame only when identity or presentation context matters. Show the timestamp only when requested.
- **Minutes + mixed materials:** chapter heading → relevant visual evidence → transcript block → related quote/callout → next evidence.
- **Text-only fallback:** preserve transcript with headings and source/timestamp markers; never create decorative or invented images.

When a visual is expected but unavailable, put an explicit inline placeholder at that point, for example: `【视觉材料缺失：原 Deck 第 5 页未能访问】`, followed by the matching transcript. Do not silently collapse the section into an unexplained text wall.

The default visual rhythm is one meaningful visual followed by the text it explains. Use a visual before the text when the reader needs to see the slide/interface first; use a short explanatory paragraph before the visual only when the speaker introduces what will appear next.

### 5. Write the Feishu document

Create a new document unless the user explicitly names an existing target. Do not overwrite an existing document without explicit instruction.

For an existing target document, use this safe update path:

1. Fetch the current document with block IDs and identify the exact section or generated-run boundary.
2. Decide whether the user wants append, replace, or a targeted section update. If the wording does not make this clear, ask before writing; never infer overwrite or append from “继续整理”.
3. Preserve unrelated blocks and source links. Before a replacement, fetch and save a local snapshot of the current content for recovery, then state the replacement scope.
4. After each material write, re-fetch the affected structure. Never reuse block IDs invalidated by overwrite/replace/delete, and do not append a section whose source ID already exists.
5. Verify that the update did not duplicate headings, images, or transcript intervals.

Recommended structure:

```text
Title
Source note: original materials and processing scope
Contents / sections

## Section title
### Page or moment label
[visual]
Caption: what the slide/frame shows
视频稿: one continuous cleaned paragraph matching the visual

### Next page or moment
...
```

Use rich XML through `lark-doc` so headings, paragraphs, images, captions, callouts, links, and spacing remain editable. Keep visual blocks large enough to read, but do not stretch low-resolution frames. Use descriptive captions for page numbers and visual context. Keep video timestamps out of the audience-facing document by default; retain them in the internal alignment map or verification note.

Do not turn every sentence into a card, add a gallery of near-duplicate frames, or place all images at the end. The document should read like a visual transcript, not an asset dump.

### 6. Verify the rendered result

After writing, fetch the document and inspect its block structure and media references. If available, use the document/media preview or a browser screenshot to inspect the rendered page. Check:

- section order follows the source;
- every major visual has matching text nearby;
- no large transcript section is left visually orphaned;
- no slide/frame is obviously mismatched or duplicated without reason;
- video frames show the meaningful screen state, not only the corner avatar;
- images are readable and successfully inserted;
- source links, page numbers, timestamps, and uncertainty notes survive;
- default prose outputs contain no per-cue timestamp clutter, and each visual is followed by a coherent matching paragraph;
- retries will not append a second copy of the same section.

If visual inspection is unavailable, say so explicitly and report the API/block-level checks instead of claiming the layout was verified.

## Guardrails

- Do not fabricate missing transcript, slide text, speaker identity, avatar, timestamp, or visual interpretation.
- Do not silently replace a complete transcript with a polished summary.
- Do not call a transcript “complete” unless source intervals and output intervals have been compared; retain a compact coverage table with source start/end, output start/end, and any omission reason. Report every unclosed or intentionally omitted range.
- Do not force a low-confidence semantic match; label it and keep the source order.
- Do not upload private media to an unrelated service. Keep processing within the authorized Feishu/workspace path.
- Do not send the finished document to a chat or change sharing permissions unless the user asks.
- For partial failures, preserve a recoverable draft or stop before destructive overwrite; report the exact missing asset.

## References

- Read [zara-reference.md](references/zara-reference.md) when matching the target look and content fidelity.
- Read [video-frame-policy.md](references/video-frame-policy.md) for screen-share/avatar separation and keyframe selection.
- Read [alignment-and-layout.md](references/alignment-and-layout.md) before building the alignment map or composing rich Feishu XML.
