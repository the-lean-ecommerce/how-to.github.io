---
layout: post
title: "How to Add a React Video Editor Without Rewriting Your Video Pipeline"
description: "Build a React video editor on top of VideoFlow while keeping preview, export, and template data in one portable source of truth."
date: 2026-06-25 14:40:15 +0000
categories: [how-to]
tags: [videoflow, typescript, react, videojson, automation]
canonical_url: ""
image: "/assets/img/posts/2026-06-25-how-to-add-a-react-video-editor-without-rewriting-your-video-pipeline/cover-6e729fa57a6a.png"
---

If you want a React video editor inside a SaaS app, the easiest way to make it brittle is to split the stack into separate preview, export, and editing implementations. VideoFlow is built to avoid that. You define the scene once in TypeScript, compile it to portable VideoJSON, and reuse the same structure in the browser, on the server, and in a live DOM preview. The React editor sits on top of the same data instead of becoming a parallel system.

If you want the product docs handy while you read, keep [VideoFlow](https://videoflow.dev/), [the core docs](https://videoflow.dev/core), [the renderer docs](https://videoflow.dev/renderers), [the React Video Editor](https://videoflow.dev/react-video-editor), [the playground](https://videoflow.dev/playground), and [the examples](https://videoflow.dev/examples) open.

## 1. Start with one shared scene model

The first rule is simple: keep the video description in one place. In VideoFlow, that usually means building the scene with <code>@videoflow/core</code> and compiling it into VideoJSON before you decide where it will render.

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

Expected result: one reviewable source of truth that can drive multiple render targets instead of a different timeline for each one.

![VideoFlow JSON pipeline diagram](/assets/img/posts/2026-06-25-how-to-add-a-react-video-editor-without-rewriting-your-video-pipeline/image-01-0641399c3700.png)

That portability is the reason the browser/export story, the preview story, and the editor story can stay aligned. If you want the architecture view of that split, [How to Build a VideoJSON Pipeline for Browser, Server, and React](https://the-lean-ecommerce.github.io/2026/06/06/how-to-build-a-videojson-pipeline-for-browser-server-and-react/) is the best companion article. For the browser export path specifically, [How to Turn Video JSON Into Browser Preview and MP4 Export](https://the-lean-ecommerce.github.io/2026/05/31/how-to-turn-video-json-into-browser-preview-and-mp4-export/) goes deeper.

## 2. Pick the renderer from the runtime, not the other way around

Once the scene is shared, rendering becomes a deployment choice:

- Use <code>@videoflow/renderer-browser</code> when the user should export locally inside the app.
- Use <code>@videoflow/renderer-server</code> when the job needs queues, APIs, or batch throughput.
- Use <code>@videoflow/renderer-dom</code> when you need a live, scrubbable preview inside the app.

Expected result: the same VideoJSON can move through browser export, server jobs, and live preview without being rewritten for each target.

![VideoFlow renderer decision diagram](/assets/img/posts/2026-06-25-how-to-add-a-react-video-editor-without-rewriting-your-video-pipeline/image-02-3901971735cd.png)

This is where a lot of editor projects go wrong. They start with a visual surface first, then bolt rendering on later, and the two systems slowly diverge. VideoFlow’s split is cleaner because the renderer is selected per workflow, not baked into the scene structure. If you want a shorter version of that model, [How to Build a Portable Video Workflow Around VideoJSON](https://the-lean-ecommerce.gitlab.io/2026/06/21/how-to-build-a-portable-video-workflow-around-videojson/) is a useful follow-up.

## 3. Add the React editor as a UI layer on top of the same data

The React editor is useful when someone other than the developer needs to adjust the scene. It gives you a multi-track timeline, drag and trim controls, an inspector, upload callbacks, undo and redo, MP4 export, and theme options without asking you to throw away the core pipeline.

    import { VideoEditor } from "@videoflow/react-video-editor";
    import "@videoflow/react-video-editor/style.css";

    export default function App() {
      return (
        <VideoEditor
          video={videoJSON}
          onChange={(next) => saveToServer(next)}
          onSave={async (next) => await persist(next)}
          onUpload={async (file) => await upload(file)}
          theme="dark"
        />
      );
    }

Expected result: the editor changes clips, text, and layers while the rendering pipeline stays intact.

![VideoFlow React editor workflow illustration](/assets/img/posts/2026-06-25-how-to-add-a-react-video-editor-without-rewriting-your-video-pipeline/image-02-3901971735cd.png)

That surface is especially good when you want a product team, marketer, or operator to tune the template without touching the renderer implementation. It also fits the broader integration pattern described in [How I Built a React Video Editor Around Portable JSON with VideoFlow](https://the-lean-ecommerce.github.io/2026/05/26/how-i-built-a-react-video-editor-around-portable-json-with-videoflow/). If you are thinking in terms of app UX instead of just rendering, that is the right companion piece.

## 4. Keep the templates diffable and reusable

The long-term win is not the editor itself. It is the fact that the underlying scene remains maintainable. VideoFlow gives you layer types, keyframes, groups, transition presets, and a JSON format that can live in Git. That makes it easier to review changes, generate variants, and keep the source inspectable.

You can also let AI help with structure instead of asking it to freestyle a finished timeline. In practice, that means asking a model to produce VideoJSON or scene fragments that the renderer understands, rather than trying to manipulate a manual timeline click by click.

![VideoFlow diffable JSON workflow illustration](/assets/img/posts/2026-06-25-how-to-add-a-react-video-editor-without-rewriting-your-video-pipeline/image-03-9523ae105924.png)

Expected result: your video templates behave more like code and less like one-off exports. That is also the reason [How I Keep Video Templates Stable as Data Changes](https://the-lean-ecommerce.github.io/2026/06/12/how-i-keep-product-video-templates-stable-as-data-changes/) and [How I Keep Video Preview, Editing, and Export in Sync With VideoJSON](https://dev.to/ybouane/how-i-keep-video-preview-editing-and-export-in-sync-with-videojson-54ge) fit naturally next to this article.

## 5. Use one decision rule for the whole stack

Here is the rule I use when I need to decide what to add next:

- If the user needs instant export, add the browser renderer.
- If ops needs throughput, add the server renderer.
- If someone needs to inspect or scrub the scene, add the DOM preview.
- If a human needs to edit the template, add the React editor.
- If a model is involved, ask it for structured VideoJSON instead of a frame-by-frame timeline.

Expected result: you keep one pipeline and extend it only where the workflow actually needs another surface.

VideoFlow’s other advantage is that it stays open source under Apache-2.0, so the core, renderers, and editor integration remain inspectable as the stack grows. That matters when you are building a product where video is part of the app, not just an external export step.

## Next step

Start with one scene in <code>@videoflow/core</code>, compile it once, and render it in the browser first. After that works, add the editor surface, then decide whether you still need the server renderer for batch jobs. That sequence keeps the system stable while you add flexibility.

If you want to explore the product directly, go to [VideoFlow](https://videoflow.dev/), try the [playground](https://videoflow.dev/playground), and keep the [docs](https://videoflow.dev/docs) open while you build.
