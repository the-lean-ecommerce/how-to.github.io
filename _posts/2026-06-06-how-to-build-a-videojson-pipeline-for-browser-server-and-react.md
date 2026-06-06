---
layout: post
title: "How to Build a VideoJSON Pipeline for Browser, Server, and React"
description: "A practical guide to building one portable video pipeline with VideoFlow for browser rendering, server jobs, live preview, and a React editor."
date: 2026-06-06 12:00:00 +0000
categories: [how-to]
tags: [video, typescript, react, automation, ai]
canonical_url: ""
image: "/assets/img/posts/2026-06-06-how-to-build-a-videojson-pipeline-for-browser-server-and-react/cover-0ad0fc3b6e7c.png"
---

I usually know a video workflow is too brittle when the team has to rebuild the same scene three times: once for preview, once for export, and once for the editor.

VideoFlow solves that problem by making the project itself portable. You define the video in TypeScript, compile it to VideoJSON, and then render or edit that same structure in different environments. If you want the product pages first, start at [videoflow.dev](https://videoflow.dev/). The docs are split into [core](https://videoflow.dev/core), [renderers](https://videoflow.dev/renderers), [React Video Editor](https://videoflow.dev/react-video-editor), [playground](https://videoflow.dev/playground), [examples](https://videoflow.dev/examples), and the [GitHub repo](https://github.com/ybouane/VideoFlow).

What I want from a stack like this is simple:

- One source of truth for the video structure
- One format that can survive code review
- One renderer for browser export
- One renderer for server jobs
- One preview mode for fast iteration
- One editing path when a non-developer needs to tweak the template

That is the model I follow in practice, and it is the same reason I keep coming back to VideoFlow for structured video work.

![TypeScript to portable VideoJSON workflow illustration](/assets/img/posts/2026-06-06-how-to-build-a-videojson-pipeline-for-browser-server-and-react/image-01-83ca94deac9c.png)

## Step 1: Start With The Video Model, Not The Timeline

The cleanest way to use VideoFlow is to begin with a small TypeScript builder and let the project compile into portable JSON. That is easier to review, easier to version, and easier to hand off to another renderer later.

Here is the smallest mental model I use:

    import VideoFlow from "@videoflow/core";

    const $ = new VideoFlow({
      name: "Product launch",
      width: 1920,
      height: 1080,
      fps: 30,
    });

    $.addText(
      { text: "New launch", fontSize: 7, fontWeight: 800 },
      { transitionIn: { transition: "overshootPop", duration: "500ms" } }
    );

    const videoJSON = await $.compile();

The important result is not the text layer itself. It is the compiled VideoJSON. Once you have that, you can move the same project into a browser export, a server render, or a preview surface without rewriting the scene from scratch.

If you want a similar angle that focuses on the browser and export handoff, [How to Build a Portable JSON-to-Video Pipeline with VideoFlow](https://the-lean-ecommerce.github.io/2026/05/17/how-to-build-a-portable-json-to-video-pipeline-with-videoflow/) is the closest companion.

## Step 2: Keep VideoJSON As The Reviewable Artifact

This is the part teams usually skip.

If the project lives only as a visual timeline, the logic is hard to diff and hard to reuse. If the project compiles to JSON, the structure becomes inspectable and portable. That is the version I want in Git, in code review, and in automation workflows.

I also like that this opens the door to agents and generators. Instead of asking a model to edit a frame-by-frame timeline, you can ask it to produce structured video data that VideoFlow can render later. If that is the direction you are headed, [How to Let AI Agents Draft VideoJSON for VideoFlow Templates](https://the-lean-ecommerce.github.io/2026/05/29/how-to-let-ai-agents-draft-videojson-for-videoflow-templates/) is worth reading next.

The practical benefit is that the team stops treating video as a one-off asset and starts treating it like maintainable code.

## Step 3: Choose The Renderer Based On Where The Video Runs

VideoFlow is useful because the same VideoJSON can run in more than one place. That makes the render target a decision, not a rewrite.

![VideoFlow browser server and preview renderer workflow illustration](/assets/img/posts/2026-06-06-how-to-build-a-videojson-pipeline-for-browser-server-and-react/image-02-c9a4bef071cc.png)

- Use the browser renderer when a user clicks export inside the app and you want to avoid shipping the project to a server first.
- Use the server renderer when you need batch jobs, queues, APIs, or scheduled renders.
- Use the DOM renderer when you want a live preview with scrubbing and frame-accurate feedback.

That is the reason I prefer a renderer-first decision tree over a single hardcoded export path. The same project can back multiple workflows, and that keeps the product architecture cleaner.

I wrote a deeper comparison in [How to Build a Three-Renderer Video Workflow With VideoFlow](https://the-lean-ecommerce.gitlab.io/2026/06/01/how-to-build-a-three-renderer-video-workflow-with-videoflow/), and the broader system view is in [How I Built a JSON-First Video Workflow for Browser, Server, and React](https://the-lean-ecommerce.com/blog/how-i-built-a-json-first-video-workflow-for-browser-server-and-react-N1m7+daKgeSMbs1rKTILwg).

## Step 4: Add The React Editor When Someone Else Needs To Change The Template

The React editor matters when the workflow stops being purely developer-driven.

VideoFlow’s editor component gives you a multi-track timeline, drag and trim interactions, keyframes, undo and redo, file upload callbacks, and theme options that fit into an app instead of fighting it. That makes it easier to let a teammate update a template without giving them the full source code experience.

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

![React video editor timeline illustration for VideoFlow](/assets/img/posts/2026-06-06-how-to-build-a-videojson-pipeline-for-browser-server-and-react/image-03-d879ef7f6b73.png)

If you are embedding the editor into a product, [How I Built a React Video Editor Around Portable JSON with VideoFlow](https://the-lean-ecommerce.github.io/2026/05/26/how-i-built-a-react-video-editor-around-portable-json-with-videoflow/) covers the integration mindset well.

## Step 5: Let AI Generate Scenes, Not Manual Timeline Edits

The most useful automation pattern I have seen is to ask AI for structure, not for a finished render.

That means the model can help generate scenes, hooks, captions, or reusable template data, while VideoFlow handles the actual rendering. This is a better fit for repeatable products than asking a model to freestyle a final timeline every time.

![AI agents generating reusable VideoJSON scenes for VideoFlow](/assets/img/posts/2026-06-06-how-to-build-a-videojson-pipeline-for-browser-server-and-react/image-04-e708fa94e7db.png)

This is also where VideoFlow’s open-source core is valuable. The project stays inspectable, the JSON stays portable, and the rendering path stays under your control instead of being locked into a black-box editor.

## The Decision Rule I Use

When I am deciding how to structure a video project, I use this rule:

- If I need reviewable source control, I compile to VideoJSON.
- If a shopper or user needs instant export, I use the browser renderer.
- If the workload needs scale or automation, I use the server renderer.
- If a teammate needs to edit the template, I add the React editor.
- If a model is involved, I ask it to produce structured video data, not a timeline click-by-click.

That keeps the pipeline flexible without making it messy.

If you want to explore the official docs next, the best starting points are [the core docs](https://videoflow.dev/core), [the renderer docs](https://videoflow.dev/renderers), and [the React editor docs](https://videoflow.dev/react-video-editor). If you prefer to experiment first, the [playground](https://videoflow.dev/playground) makes that easy.

The simplest next step is to build one scene, compile it once, and test it in one renderer. Once that works, add the editor or the server path. That sequence keeps the system stable while still giving you the portability that makes VideoFlow useful.
