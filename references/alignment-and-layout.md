# Alignment and layout reference

## Alignment ledger

Before a Feishu write, keep a ledger like this:

| Order | Section | Visual | Source locator | Transcript range | Relation | Confidence |
|---:|---|---|---|---|---|---|
| 1 | 会议即 Context | slide-05 | Deck p.5 | 00:08:10–00:10:42 | explains | high |
| 2 | 操作演示 | video-02 | 00:14:03 | 00:13:50–00:15:20 | demonstrates | medium |

The ledger is internal unless a concise source note helps the reader. It prevents accidental reordering and provides a recovery point for partial writes.

## Transcript coverage check

Maintain two ordered interval lists: source transcript intervals and output transcript intervals. Before claiming completion, check that every source interval is covered once, or is explicitly marked as omitted with a timestamped reason such as “duplicate filler removed” or “unintelligible audio”. A long unmarked gap is a failure. Retain a compact coverage table in the handoff or internal work note. Visuals may be missing; the corresponding transcript must still remain in the output with an inline visual-missing marker.

## Visual rhythm

Use this block sequence as the default:

```text
section heading
short orientation sentence (optional)
visual
caption / provenance
matching transcript
optional quote, callout, or related link
next visual...
```

Use a larger heading when the Deck changes section; use a smaller heading for each slide or video moment. Keep related transcript paragraphs together, but split at real topic or visual-state changes. Preserve speaker labels only when they help follow a multi-speaker exchange.

## Feishu XML decisions

For a new rich document, follow the installed `lark-doc` XML and style references. For an existing document, first fetch with IDs and use its update workflow; do not blindly append a second generated run. Prefer native editable blocks:

- headings for sections and moments;
- paragraphs for transcript;
- image blocks for Deck pages and keyframes;
- short captions for page/timestamp/source;
- callouts for uncertainty, source gaps, or a compact “本段要点” only when useful;
- links for original Minutes, Deck, video, or source document.

Do not use a table as the primary layout for long transcript passages. Do not put the entire document into one giant HTML block. Use grids/columns sparingly for a compact metadata header or side-by-side comparison; the reading path should remain vertical and mobile-friendly.

## Density and fidelity

- Prefer one meaningful visual per conceptual step.
- Keep the original slide/frame ratio; never distort it to fill a placeholder.
- If a page is unreadable at document width, insert a larger image or link to the original source rather than inventing a transcription.
- Keep full transcript text in the document; use summaries only as navigation aids.
- Mark low-confidence alignment and missing media instead of silently hiding them.
