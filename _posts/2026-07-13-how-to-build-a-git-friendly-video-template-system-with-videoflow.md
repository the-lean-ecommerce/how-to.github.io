---
layout: post
title: "How to Build a Git-Friendly Video Template System With VideoFlow"
description: "Keep VideoFlow templates in Git, compile to portable VideoJSON, and render the same project in browser, Node, or React."
date: 2026-07-13 11:36:57 +0000
categories: [how-to]
tags: [videoflow, typescript, json, react, github-pages, open-source]
canonical_url: ""
image: "/assets/img/posts/2026-07-13-how-to-build-a-git-friendly-video-template-system-with-videoflow/cover-0c4d6a46c61d.png"
---

# How to Build a Git-Friendly Video Template System With VideoFlow

If your team keeps changing video templates in one place and renderer code in another, the project will drift fast. This guide shows how to keep one template in Git, compile it to portable VideoJSON with [VideoFlow](https://videoflow.dev/), and then render the same artifact in the browser, on the server, or inside a React editor.

You only need a TypeScript project, Node.js 18 or newer, and one repo where template code can live next to assets. If you want the broader product reference, start with the [VideoFlow docs](https://videoflow.dev/docs), then branch out to the [core docs](https://videoflow.dev/core), [renderers docs](https://videoflow.dev/renderers), and [React video editor page](https://videoflow.dev/react-video-editor).

## 1. Set Up A Repository Layout That Separates Source, Assets, And Renders

Start by creating a structure that makes it obvious what is source of truth and what is just output:

```text
video/
  assets/
  templates/
  renders/
```

Then install the packages you actually need:

```bash
npm install @videoflow/core @videoflow/renderer-browser @videoflow/renderer-server @videoflow/renderer-dom @videoflow/react-video-editor
```

If you do not need the editor yet, leave `@videoflow/react-video-editor` out for now. The important part is that the template code sits in Git as code, while renders remain disposable artifacts.

Expected result: `git diff` should show changes in your template files, not in a random renderer scratchpad.

If you want the JSON-first version of this workflow, it pairs well with [How to Build a JSON-First Video Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/06/30/how-to-build-a-json-first-video-workflow-with-videoflow/).

## 2. Author One Reusable Template And Compile It To VideoJSON

VideoFlow's core value is that you describe the video once and then reuse that definition in multiple places. A minimal starting point looks like this:

```ts
import VideoFlow from "@videoflow/core";

const $ = new VideoFlow({
  name: "Launch Clip",
  width: 1920,
  height: 1080,
  fps: 30,
});

$.addText({
  text: "Portable template",
  fontSize: 7,
  fontWeight: 800,
});

$.addText({
  text: "One source of truth for every renderer",
  fontSize: 4.5,
  opacity: 0.85,
});

const videoJSON = await $.compile();
```

From there, you can add the richer VideoFlow primitives the product is built around: `wait`, `parallel`, grouping, transitions, captions, shapes, and effects. The point is not to cram everything into one scene. The point is to keep one durable template that compiles into a portable artifact.

Expected result: `videoJSON` becomes the thing you version, review, and pass to renderers instead of a one-off timeline.

The portability model is the same one discussed in [How to Render One Video JSON in Browser, Node, and React](https://how-to-blog.gitlab.io/2026/07/02/how-to-render-one-video-json-in-browser-node-and-react/).

![Three renderer panels connected to one shared video JSON](/assets/img/posts/2026-07-13-how-to-build-a-git-friendly-video-template-system-with-videoflow/image-01-7d5c49569c2c.png)

## 3. Pick The Renderer When You Publish, Not When You Write

VideoFlow is useful because the same VideoJSON can be executed in different environments without rewriting the template.

- Use `@videoflow/renderer-browser` when you want users to export inside the browser and avoid a server round trip.
- Use `@videoflow/renderer-server` when you need API jobs, queues, scheduled batches, or CI-driven exports.
- Use `@videoflow/renderer-dom` when you need a live preview in a dashboard, editor, or internal tool.

That split keeps the template stable while you change the execution model. If a product manager wants a preview, you do not fork the scene. If operations wants batch rendering, you do not rewrite the timeline. You just point the same JSON at a different renderer.

Expected result: one template, three execution paths, no duplicate motion logic.

This is the same split I used in [How I Pick the Right VideoFlow Renderer for the Job](https://the-lean-ecommerce.gitlab.io/2026/07/11/how-i-pick-the-right-videoflow-renderer-for-the-job/) and [How to Build a VideoFlow Project That Keeps Templates and Renderers Separate](https://how-to.the-lean-ecommerce.com/2026/07/08/how-to-build-a-videoflow-project-that-keeps-templates-and-renderers-se/).

## 4. Add The React Editor Only When Someone Needs To Change The Template

If non-developers need to tweak copy, media, trims, or keyframes, add the React editor on top of the same JSON source. Keep the editor optional so your renderer pipeline stays independent.

```tsx
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
```

The component gives you a multi-track timeline, inspector controls, drag-and-drop editing, undo and redo, file uploads through callbacks, and MP4 export. VideoFlow also ships four built-in themes: `light`, `grey`, `dark`, and `night`, with CSS-variable customization when you need it.

Expected result: editors can work on the same video model without opening the renderer code or the template builder.

If you want the broader architecture behind that decision, read [How to Add a React Video Editor Without Rewriting Your Render Pipeline](https://how-to-blog.gitlab.io/2026/07/12/how-to-add-a-react-video-editor-without-rewriting-your-render-pipeline/).

![React video editor dashboard with timeline, inspector, and live preview](/assets/img/posts/2026-07-13-how-to-build-a-git-friendly-video-template-system-with-videoflow/image-02-eca1fcea2345.png)

## 5. Commit The Template, Review The JSON, And Keep Outputs Disposable

Once the template works, commit the source files and review the resulting JSON the same way you would review any other generated artifact. If the video changes, you should be able to see whether the update came from copy, timing, media, or renderer settings.

A practical review loop looks like this:

1. Change the template in `video/templates/`.
2. Compile to VideoJSON.
3. Render in at least two environments.
4. Inspect the diff before you merge.

That structure keeps the repo honest. It also makes it easier to back out a bad edit because the source of truth is still code, not a baked video file.

Expected result: your Git history explains what changed, why it changed, and which output it affected.

![Git-friendly video template branches feeding a single export](/assets/img/posts/2026-07-13-how-to-build-a-git-friendly-video-template-system-with-videoflow/image-03-df003aa7bd57.png)

## Troubleshooting

- If browser export works but server export fails, compare fonts, assets, and any environment-specific renderer settings.
- If the preview and MP4 do not match, check whether the same transitions, effects, and media are available in both render paths.
- If your Git diffs are noisy, keep generated MP4s out of the main review path and review the template plus JSON instead.

## Conclusion

The simplest stable pattern is one template, one compiled JSON artifact, and multiple renderers. Start with `@videoflow/core`, keep the template in Git, and only add browser export, server batches, or the React editor when a real workflow needs them.

Next, compare this setup with [How to Preview, Edit, and Export the Same Video JSON Everywhere](https://how-to-blog.gitlab.io/2026/07/01/how-to-preview-edit-and-export-the-same-video-json-everywhere/) if you want to tighten the renderer handoff, or revisit [How to Build a JSON-First Video Workflow With VideoFlow](https://how-to.the-lean-ecommerce.com/2026/06/30/how-to-build-a-json-first-video-workflow-with-videoflow/) if you want to go deeper on the portable JSON layer.
