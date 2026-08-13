# Zara reference: target behavior

Use this as a fidelity reference, not as a claim that Zara's private agent or implementation is available.

## Confirmed example

- Document: [图文版逐字稿：4 个可以立刻落地的 AI-native 工作方式](https://bytedance.us.larkoffice.com/docx/YwYUdeyyWoJsLoxEvXQuQuVKsUh)
- Related Minutes: [妙记回放](https://bytedance.us.larkoffice.com/minutes/obusu3imt7yv9iil1s9r9h44)
- Related Deck: [4 个 AI-native 工作方式](https://bytedance.feishuapp.com/app/app_179rdtf6x9e/)

## Source prompt pattern

The useful core of Zara's prompt is:

> 基于演讲回放妙记和演讲用的 deck，做一个图文版本，用飞书文档输出；放完整逐字稿，除了口误和口水词/语气词不要删改；基于上下文纠正转写错误；穿插 deck 每一页截图；每一页 deck 紧跟这一页对应的逐字稿；根据 deck 结构编排 sections 小标题。

The skill extends this pattern to arbitrary modalities and adds explicit alignment confidence, video frame selection, provenance, and rendered-document verification.

## Visual-transcript acceptance test

The output is correct only if a reader can follow the talk by scrolling:

1. A section heading gives the topic.
2. A slide or meaningful video frame appears at the point where it matters.
3. The matching spoken passage follows immediately or is clearly scoped to that visual.
4. The next visual and passage continue the source chronology.
5. The full meaningful transcript remains available; it is not reduced to bullet points.

A document with all images first and all transcript later fails this test, even if every asset is technically present.
