---
layout: post
title: "How to Keep Browser, Server, and Editor in Sync With One VideoJSON File"
description: "Use VideoFlow to define one portable VideoJSON source of truth that renders in the browser, on a server, and inside a React video editor."
date: 2026-07-23 07:00:00 +0000
categories: [how-to]
tags: [videoflow, json-to-video, typescript, react-video-editor, browser-video-rendering, programmatic-video]
canonical_url: ""
image: "/assets/img/posts/2026-07-23-how-to-keep-browser-server-and-editor-in-sync-with-one-videojson-file/cover-d4d1da0bf9d0.png"
---

# How to Keep Browser, Server, and Editor in Sync With One VideoJSON File

If you are building programmatic video tools, the hard part is usually not generating one render. It is keeping preview, export, and editing on the same project format. VideoFlow is built for that problem: you author a video in TypeScript, compile it into portable VideoJSON, and render the same scene in the browser, on a server, or inside a live DOM preview.

That is the thread running through [How to Build a Browser-Based MP4 Export Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/07/14/how-to-build-a-browser-based-mp4-export-workflow-with-videoflow/), [How to Build a Reviewable JSON-to-Video Pipeline With VideoFlow](https://how-to-blog.gitlab.io/2026/07/19/how-to-build-a-reviewable-json-to-video-pipeline-with-videoflow/), [How to Build a Merge-Request-Friendly Video Workflow With VideoFlow](https://how-to-blog.gitlab.io/2026/07/20/how-to-build-a-merge-request-friendly-video-workflow-with-videoflow/), and [How I Build a Three-Renderer Video Workflow With VideoFlow](https://the-lean-ecommerce.github.io/2026/07/20/how-i-build-a-three-renderer-video-workflow-with-videoflow/). This guide narrows the problem to one rule: keep one JSON source of truth and make every renderer consume it.

The relevant docs are the [VideoFlow docs](https://videoflow.dev/docs), the [renderer docs](https://videoflow.dev/renderers), and the [React video editor docs](https://videoflow.dev/react-video-editor).

## 1. Start With One VideoJSON Contract

Use <code>@videoflow/core</code> as the authoring layer. Keep the project definition in one TypeScript file and compile it into VideoJSON instead of hand-building separate browser and server timelines.

    npm install @videoflow/core

    import VideoFlow from "@videoflow/core";

    const $ = new VideoFlow({
      name: "Launch Preview",
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

Expected result: one JSON artifact becomes the handoff between authoring, preview, and export.

![Abstract pipeline diagram linking one video file to three render targets](/assets/img/posts/2026-07-23-how-to-keep-browser-server-and-editor-in-sync-with-one-videojson-file/image-01-385a181ff0bf.png)

## 2. Pick the Right Renderer for the Job

VideoFlow’s renderer split is the part that makes the workflow portable. The same VideoJSON can drive browser export, server-side rendering, and a live DOM preview, so you can keep the project format stable while choosing the runtime that fits the job.

Use the browser renderer when the user is already inside your app and clicks Export. Use the server renderer when you need batch jobs, queue workers, scheduled renders, or API-driven jobs. Use the DOM renderer when you need a live, scrubbable preview that tracks the project as it changes.

A practical rule:

- Browser renderer for low-friction, client-side export.
- Server renderer for queues, APIs, and large jobs.
- DOM renderer for editors, dashboards, and frame-accurate preview.

Expected result: you stop rewriting timelines for each runtime and start choosing a renderer by operational need instead of by project format.

![One VideoJSON source branching to browser preview, server render, and editor](/assets/img/posts/2026-07-23-how-to-keep-browser-server-and-editor-in-sync-with-one-videojson-file/image-02-3467d77602cd.png)

## 3. Add a React Editor Without Changing the Format

If users need to edit the video inside your product, add the optional React editor rather than inventing a second data model. The editor accepts the same project JSON, gives you a multi-track timeline, and keeps the preview tied to the same underlying structure.

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

The editor path is where teams usually drift. The safe pattern is to treat the editor as a view over VideoJSON, not as a competing source of truth. That keeps layers, keyframes, transitions, themes, and MP4 export aligned with the core project.

Expected result: users can trim, reorder, keyframe, and export without creating a second format that later needs migration.

![Video editor timeline and inspector for a JSON-first project](/assets/img/posts/2026-07-23-how-to-keep-browser-server-and-editor-in-sync-with-one-videojson-file/image-03-7c5e8d7c51c4.png)

## 4. Keep the Project Reviewable in Git

This is where VideoFlow becomes useful for teams that care about code review. When the project lives in Git, the rendered output is still important, but the reviewable artifact is the project JSON or the TypeScript builder that creates it.

Keep one file per template. Review changes in a branch before they reach production. Re-render the same JSON after merge so the visual output stays tied to a commit instead of to a manual editing session.

That is why the Git-focused posts in this series matter: [How to Build a Git-Friendly Video Template System With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/07/13/how-to-build-a-git-friendly-video-template-system-with-videoflow/) shows the template side, while [How to Build a Reviewable JSON-to-Video Pipeline With VideoFlow](https://how-to-blog.gitlab.io/2026/07/19/how-to-build-a-reviewable-json-to-video-pipeline-with-videoflow/) and [How to Build a Merge-Request-Friendly Video Workflow With VideoFlow](https://how-to-blog.gitlab.io/2026/07/20/how-to-build-a-merge-request-friendly-video-workflow-with-videoflow/) show how to make that reviewable in practice.

Expected result: diffs are small enough to inspect, and the same project can move cleanly from branch to render without format drift.

![Git-friendly diff view for a VideoFlow project](/assets/img/posts/2026-07-23-how-to-keep-browser-server-and-editor-in-sync-with-one-videojson-file/image-04-ddf7626c2c9b.png)

## 5. Decide Which Path Owns Export

Once the shared format is in place, the decision becomes operational rather than architectural.

If the user is exporting inside your app, let the browser renderer own that path. If the job is queued, scheduled, or triggered by automation, let the server renderer own it. If the user needs to edit the content before export, keep the React editor in front of the same VideoJSON and let the renderer consume the result.

That is the same pattern behind [How to Build a Portable Video Export Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/07/18/how-to-build-a-portable-video-export-pipeline-in-videoflow/) and [How to Build a Browser-Based MP4 Export Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/07/14/how-to-build-a-browser-based-mp4-export-workflow-with-videoflow/). The format stays still even when the runtime changes.

## Conclusion

The simplest way to keep browser preview, server export, and editor behavior in sync is to stop letting each one invent its own project shape. Build once in <code>@videoflow/core</code>, compile to VideoJSON, and let the browser, server, and editor all consume the same artifact.

If you want to try it, start with the [VideoFlow Playground](https://videoflow.dev/playground), install <code>@videoflow/core</code>, and render one small scene in both the browser and server paths before you expand the template.
