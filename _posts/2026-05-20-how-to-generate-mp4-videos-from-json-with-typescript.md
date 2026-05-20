---
layout: post
title: "How to Generate MP4 Videos from JSON with TypeScript"
description: "Build a portable JSON-to-MP4 workflow in TypeScript with VideoFlow and reusable renderers."
date: 2026-05-20 18:35:22 +0000
categories: [how-to]
tags: [videoflow, typescript, json-to-video, mp4, programmatic-video]
canonical_url: ""
image: "/assets/img/posts/2026-05-20-how-to-generate-mp4-videos-from-json-with-typescript/cover-9ed4a74847de.png"
---

# How to Generate MP4 Videos from JSON with TypeScript

If you need to turn structured data into a video without rebuilding the timeline by hand every time, VideoFlow gives you a clean path: describe the video once, keep it portable as VideoJSON, and render it where you need it. The open-source toolkit lives at [videoflow.dev](https://videoflow.dev/), with [docs](https://videoflow.dev/docs), a [playground](https://videoflow.dev/playground), [core docs](https://videoflow.dev/core), and [examples](https://videoflow.dev/examples).

The workflow is a good fit when you want product demos, explainers, onboarding clips, or automated social videos that stay maintainable in TypeScript. The key idea is simple: keep the scene definition structured, keep the renderer interchangeable, and keep the output format reusable.

![Structured JSON becomes layered scenes and a video strip](/assets/img/posts/2026-05-20-how-to-generate-mp4-videos-from-json-with-typescript/image-01-153eaf46c71a.png)

## Prerequisites

Before you start, make sure you have:

- A TypeScript project running on Node.js.
- A clear target size for your video, such as 1920x1080.
- One or more assets, such as product screenshots, logos, or illustrations.
- A decision about where the MP4 should render: browser, server, or a live preview surface.

VideoFlow's core package is @videoflow/core. If you want a dedicated browser or server renderer, the product also ships @videoflow/renderer-browser and @videoflow/renderer-server. If you want an embedded editor, there is also @videoflow/react-video-editor.

## 1. Model The Video As Data

Start by treating the video as a structured object instead of a one-off timeline. VideoFlow's core model is VideoJSON, which keeps the scene portable and easy to reuse later. That matters because the same data can be rendered, edited, stored, and versioned without rebuilding the content from scratch.

For a practical template, think in terms of layers:

- Text for titles, subtitles, and captions.
- Images for product shots, backgrounds, or branded visuals.
- Video for motion inserts.
- Audio for music or voice tracks.
- Shapes for overlays and panels.
- Captions for spoken content or accessibility.

That structure is what lets you start simple and expand later without changing the whole workflow.

![Flow of JSON cards into layered scenes](/assets/img/posts/2026-05-20-how-to-generate-mp4-videos-from-json-with-typescript/image-01-153eaf46c71a.png)

## 2. Install The Core Package And Pick A Renderer

Install the core package first:

~~~bash
npm install @videoflow/core
~~~

If your workflow needs browser export, install the browser renderer too. If you are running batch jobs, CI jobs, or queue-based rendering, add the server renderer.

~~~bash
npm install @videoflow/renderer-browser
npm install @videoflow/renderer-server
~~~

Expected result: your project can author videos in TypeScript and hand the same portable representation to the renderer that fits the job.

If you are building an app, the optional React editor is useful later because it gives you a multi-track timeline, keyframes, transitions, effects, themes, and MP4 export.

![Browser export workflow for MP4 rendering](/assets/img/posts/2026-05-20-how-to-generate-mp4-videos-from-json-with-typescript/image-02-92db88c98c64.png)

## 3. Write The TypeScript Builder

VideoFlow's core API is intentionally direct. You create a project, add content, compile it to JSON, and render it.

~~~ts
import VideoFlow from "@videoflow/core";

const $ = new VideoFlow({
  name: "My Video",
  width: 1920,
  height: 1080,
  fps: 30,
});

$.addText(
  { text: "Hello, VideoFlow!", fontSize: 7, fontWeight: 800 },
  { transitionIn: { transition: "overshootPop", duration: "500ms" } }
);

const json = await $.compile();
const blob = await $.renderVideo();
~~~

That example is small on purpose. The important part is the shape of the workflow: author in TypeScript, compile to portable VideoJSON, then render to MP4. Once that works, you can swap the text layer for images, captions, audio, shapes, or grouped layers without changing the basic pipeline.

## 4. Render The Same VideoJSON Where You Need It

This is where VideoFlow is most useful. The same VideoJSON can run in different environments, so you do not have to maintain separate templates for each output target.

Use browser rendering when the user should click an export button inside your app. Use server rendering when you need scheduled jobs, queues, or API-triggered video generation. Use a DOM preview when you want live playback inside an editor or dashboard.

The benefit is not just convenience. It reduces template drift. If your browser export and your server export share the same source data, you are much less likely to ship a preview that differs from the final render.

If you want a richer editing layer, the React video editor gives you a multi-track experience with drag, trim, snap, reorder, undo, keyframes, and theme control. That is useful when the people authoring the content are not all working in code.

![Multi-track timeline and VideoJSON editor](/assets/img/posts/2026-05-20-how-to-generate-mp4-videos-from-json-with-typescript/image-03-bce91e8c9fda.png)

## 5. Keep The Workflow Reusable

A good TypeScript video pipeline should be easy to change later. The easiest way to keep it healthy is to separate three things:

- Input data: the text, product info, or metrics that change from render to render.
- Template code: the reusable scene layout and animation logic.
- Output target: browser export, server render, or preview.

That separation makes the system easier to test and easier to automate. It also makes the template friendlier to Git, because the structure stays readable instead of collapsing into a pile of manual timeline edits.

If you plan to generate videos repeatedly, this same pattern works well for scheduled jobs and recurring automations. Feed in new data, reuse the template, and let the renderer do the rest.

## Related Guides

If you want to keep going after this article, these nearby guides cover the next layers of the workflow:

- [How to Build a Browser-Based MP4 Export Pipeline with VideoFlow](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-build-browser-based-mp4-export.html)
- [How to Turn Product Data Into MP4 Videos with VideoFlow](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-turn-product-data-into-mp4.html)
- [How to Build a Portable JSON-to-Video Workflow with VideoFlow](https://how-to.the-lean-ecommerce.com/2026/05/17/how-to-build-a-portable-json-to-video-workflow-with-videoflow/)
- [How to Build a Portable JSON-to-Video Pipeline with VideoFlow](https://the-lean-ecommerce.github.io/2026/05/17/how-to-build-a-portable-json-to-video-pipeline-with-videoflow/)

## Troubleshooting

If the output size looks wrong, set width, height, and fps explicitly in the VideoFlow project config.

If the timing feels inconsistent, keep your transitions and keyframes grouped around the same scene boundaries instead of spreading them across unrelated layers.

If your team needs a review step, publish the VideoJSON as a draft first and only export the final MP4 after the template and inputs look correct.

## Next Step

Start with the core docs, build one short scene, and render it to MP4 before you add more layers. Once that works, split the data from the template and reuse the same TypeScript pipeline for every new video.

If you want to explore the source and examples, use the [VideoFlow GitHub repository](https://github.com/ybouane/VideoFlow) alongside the [docs](https://videoflow.dev/docs) and [playground](https://videoflow.dev/playground).
