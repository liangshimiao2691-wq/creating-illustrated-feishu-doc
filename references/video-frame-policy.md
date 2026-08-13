# Video frame policy

Read this when a video contains a presenter, a screen share, a PPT demo, a browser, or a small webcam/avatar overlay.

## What to preserve

Treat the video as two related streams:

- **Presentation stream:** the main screen, slide, browser, product UI, whiteboard, or document being demonstrated.
- **Presenter stream:** the speaker's face, avatar, gesture, or webcam tile.

The presentation stream is normally the primary visual evidence. The presenter stream is secondary context. Do not use a face-only frame as the visual for a passage that explains a slide or UI unless the passage is specifically about the person, reaction, or delivery.

## Frame selection

Select keyframes at state changes and semantic anchors, not at a fixed high frequency. Good candidates include:

- slide/page changes;
- a new application or screen state;
- a visible code, diagram, table, or product action being discussed;
- the moment an on-screen result appears;
- a deliberate pause where the speaker explains a stable screen.

Reject or deduplicate frames that are:

- blank, loading, transitional, or dominated by menus;
- visually identical to the previous selected frame;
- too small or blurry to read;
- only the corner webcam/avatar while the screen is the topic;
- unrelated to the transcript segment.

If the only available frame is a composite video, preserve the full context when necessary but crop or select the main screen for the document's primary visual. Never crop away information needed to understand the action. Record the timestamp in the caption.

When cropping a composite frame, use the same source timestamp as the full frame and keep a mapping from the cropped image to its original composite frame. If a crop removes the only visible cue for the action, retain the composite frame as a secondary “full context” image or do not crop. A crop is a presentation choice, not a new event.

## Alignment rule

For each selected frame, bind a transcript interval using timestamp proximity first, then visible/on-screen and spoken anchors. The text immediately below the frame should explain that frame, not merely occur somewhere nearby in the video.

For long demonstrations, use a small number of state-change frames and longer transcript blocks. For a short, high-density interaction, use more frames only when each frame shows a distinct state. A useful default is 1–3 frames per conceptual step, not one frame per second.

## Failure disclosure

If the recording has no usable screen stream, say so and produce a transcript-led document. If the screen is unreadable, keep the timestamp and describe the limitation; do not infer hidden slide content from speech alone.
