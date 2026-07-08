---
layout: post
title: "How to Build a VideoFlow Project That Keeps Templates and Renderers Separate"
description: "A step-by-step VideoFlow setup for reusable templates, thin renderers, and one portable VideoJSON source of truth."
date: 2026-07-08 11:42:15 +0000
categories: [how-to]
tags: [videoflow, programmatic-video, json-to-video, typescript, react-video-editor]
canonical_url: ""
image: "/assets/img/posts/2026-07-08-how-to-build-a-videoflow-project-that-keeps-templates-and-renderers-se/cover-08b69f83d067.png"
---

If you need a programmatic video pipeline that is easy to change, VideoFlow works best when you treat the video definition as data and the renderers as separate outputs. `@videoflow/core` owns the scene, while browser, server, DOM preview, and the optional React editor consume the same portable VideoJSON. That split keeps templates reusable and makes it easier to ship one workflow to multiple environments. Keep the official [core docs](https://videoflow.dev/core), [renderers docs](https://videoflow.dev/renderers), [React video editor docs](https://videoflow.dev/react-video-editor), and the [GitHub repo](https://github.com/ybouane/VideoFlow) handy while you build.



## 1. Start With One Canonical Template Folder



Keep the template in one place and keep everything else thin. A simple starting layout is `src/scenes`, `src/data`, `src/renderers`, `src/editor`, and `src/tests`. The scene folder holds the reusable composition, the data folder holds the variables that change per video, and the renderers folder holds the environment-specific entrypoints.



![VideoFlow folder structure with a central JSON core](/assets/img/posts/2026-07-08-how-to-build-a-videoflow-project-that-keeps-templates-and-renderers-se/image-01-7ced4e4ea9e0.png)



If you are coming from a more ad hoc setup, the key shift is to stop thinking of a rendered MP4 as the source of truth. The source of truth is the scene definition that can be compiled, reviewed, diffed, and rendered later. That is the same architectural idea used in [How to Build a JSON-First Video Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/06/30/how-to-build-a-json-first-video-workflow-with-videoflow/).



**Expected result:** you can point the same template at different data inputs without rewriting the scene itself.



## 2. Build The Template In `@videoflow/core`, Not In The Renderer



Install the core package first, then define the video in TypeScript before you choose where it will render.



```bash
npm install @videoflow/core
```



```ts
import VideoFlow from "@videoflow/core";

const $ = new VideoFlow({
  name: "Product Launch Teaser",
  width: 1920,
  height: 1080,
  fps: 30,
});

$.addText(
  { text: "Launch faster with one template", fontSize: 7, fontWeight: 800 },
  { transitionIn: { transition: "overshootPop", duration: "500ms" } }
);

const json = await $.compile();
const blob = await $.renderVideo();
```



The important part is the boundary. The core builder creates a portable VideoJSON artifact, and that artifact can live in Git like any other code file. The result is easier review, easier rollback, and fewer surprises when a video changes.



**Expected result:** one scene definition can drive repeatable exports instead of producing one-off renders.



## 3. Keep Browser, Server, And Preview Renderers Thin



Once the scene is stable, wire the renderers around it rather than embedding rendering logic in the scene file. In practice that means a browser export path for client-side MP4 generation, a Node render path for batch jobs and APIs, and a DOM preview for frame-accurate inspection. That pattern is a direct fit for [How to Render One Video JSON in Browser, Node, and React](https://how-to-blog.gitlab.io/2026/07/02/how-to-render-one-video-json-in-browser-node-and-react/).



![VideoFlow renderer split across browser server and React](/assets/img/posts/2026-07-08-how-to-build-a-videoflow-project-that-keeps-templates-and-renderers-se/image-02-841551a92f03.png)



If the same VideoJSON renders in all three places, you have a strong signal that the template is portable and the runtime differences are isolated to the renderers. That is the architecture VideoFlow is designed for.



**Expected result:** preview, export, and batch render all consume the same scene data without drift.



## 4. Add A React Editor Only When Humans Need To Edit



`@videoflow/react-video-editor` is useful when you want users to trim clips, reorder layers, or tweak keyframes without rebuilding the pipeline. Keep it optional. The editor should edit the same JSON the renderers already understand, not invent a parallel format. For a practical example of the integration pattern, see [How to Preview, Edit, and Export the Same Video JSON Everywhere](https://how-to-blog.gitlab.io/2026/07/01/how-to-preview-edit-and-export-the-same-video-json-everywhere/) and [How to Add a React Video Editor Without Rewriting Your Video Pipeline](https://how-to.the-lean-ecommerce.com/2026/06/25/how-to-add-a-react-video-editor-without-rewriting-your-video-pipeline/).



![VideoFlow JSON source of truth flowing into preview and editor outputs](/assets/img/posts/2026-07-08-how-to-build-a-videoflow-project-that-keeps-templates-and-renderers-se/image-03-5de7b0bd6242.png)



If the editor changes the source data cleanly and the same data still renders in the browser and on the server, the workflow is healthy. If it cannot, the editor has become too tightly coupled to one environment.



**Expected result:** users can adjust the template visually without forcing a renderer rewrite.



## 5. Add A Validation Gate Before You Publish



Before you ship a template or hand it to a teammate, run a lightweight review gate. Check the output dimensions, confirm that required assets exist, make sure the scene compiles, and verify that the renderer output still matches the intended layout. This is the same maintenance mindset behind [How I Keep Video Templates Maintainable With VideoFlow](https://the-lean-ecommerce.github.io/2026/07/07/how-i-keep-video-templates-maintainable-with-videoflow/).



![VideoFlow template review and validation checklist](/assets/img/posts/2026-07-08-how-to-build-a-videoflow-project-that-keeps-templates-and-renderers-se/image-04-aba1db94a122.png)



Troubleshooting is usually straightforward at this stage:



- If browser and server output differ, check that both entrypoints share the same template constants.

- If the editor looks correct but export does not, inspect the JSON being saved, not just the visible timeline.

- If a layout change breaks a render, review the template diff before you touch the renderer.



**Expected result:** bad template changes are caught before they become public assets or broken exports.



## 6. Version The Template Like Code



Keep the scene source, data fixtures, and renderer entrypoints in version control. When you change a template, compare the diff, render a known sample, and confirm that the result still matches the intended output. That is the point of a JSON-first workflow: the video definition stays reviewable even as the final output changes.



For nearby workflows and implementation details, the same repo structure also works well with [How I Keep Video Templates Maintainable With VideoFlow](https://the-lean-ecommerce.github.io/2026/07/07/how-i-keep-video-templates-maintainable-with-videoflow/), [How to Build a JSON-First Video Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/06/30/how-to-build-a-json-first-video-workflow-with-videoflow/), and [How to Preview, Edit, and Export the Same Video JSON Everywhere](https://how-to-blog.gitlab.io/2026/07/01/how-to-preview-edit-and-export-the-same-video-json-everywhere/).



If you are starting from scratch, build one template, render it twice, and add the editor only after the output is stable. That gives you a pipeline that stays portable instead of turning into a tangle of renderer-specific hacks.



**Next action:** wire `@videoflow/core` into one sample project, then point both a browser renderer and a server renderer at the same VideoJSON artifact.
