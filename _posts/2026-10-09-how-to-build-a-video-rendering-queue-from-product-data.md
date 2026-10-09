---
layout: post
title: "How to Build a Video Rendering Queue From Product Data"
description: "Build a reviewable product-data-to-video queue with VideoJSON, preview gates, and the right VideoFlow renderer."
date: 2026-10-09 12:31:00 +0000
categories: [how-to]
tags: [video-automation, typescript, product-data, rendering-queue, videoflow]
canonical_url: ""
image: "/assets/img/posts/2026-10-09-how-to-build-a-video-rendering-queue-from-product-data/cover-3cee038709d3.webp"
---

If your catalog already has titles, images, prices, and feature bullets, you have most of the ingredients for a repeatable product-video system. The missing piece is not another editing session. It is a queue that turns one product record into a reviewable video document, then renders it only when it is ready.

This guide shows how to build that path with [VideoFlow Core](https://videoflow.dev/core), portable VideoJSON, a live preview, and a browser or server renderer. You will need a TypeScript service, a source of product records, asset URLs your renderer can reach, and a place to persist jobs. By the end, each product can move through the same reliable sequence: data → VideoJSON → preview → approval → MP4.

## 1. Define the queue contract before writing a template

Start with a small job object. A rendering system is easier to operate when the queue stores a reference to product data and an explicit state, rather than an unstructured “make a video” request.

```ts
type VideoJob = {
  id: string;
  productId: string;
  templateVersion: "product-card-v1";
  state: "queued" | "building" | "review" | "approved" | "rendering" | "complete" | "failed";
  videoJson?: unknown;
  outputUrl?: string;
  error?: string;
};
```

Include the template version from day one. It tells you which composition created an export and lets you re-render a known batch later. Keep source product fields in your catalog or snapshot the exact values alongside the job; do not make the renderer guess a title, price, or image after the job has started.

The expected result is a job record that a worker can pick up safely and a reviewer can understand without opening the codebase.

![Product data cards assembling into a portable video document](/assets/img/posts/2026-10-09-how-to-build-a-video-rendering-queue-from-product-data/image-01-794e0afcd0d1.webp)

## 2. Turn one record into a portable VideoJSON document

Use a VideoFlow template to map stable product fields into layers: a product image, a title, a price treatment, a short benefit, and a CTA. [VideoFlow Core](https://videoflow.dev/core) is useful here because it compiles a TypeScript-authored scene into VideoJSON, a document you can store, inspect, edit, and render later.

Keep the input deliberately narrow. For example, validate that the image is public, the title is present, and the price has already been formatted for the target locale. A product record should produce a complete document or a clear validation failure—never a partly invented scene.

```ts
const product = await catalog.get(job.productId);
assertPublicUrl(product.heroImage);
assertNonEmpty(product.title);

const $ = new VideoFlow({ name: product.handle, width: 1080, height: 1080, fps: 30 });
// Add image, title, price, benefit, and CTA layers from validated product fields.
const videoJson = await $.compile();

await jobs.update(job.id, { state: "review", videoJson });
```

The expected result is a deterministic VideoJSON artifact for every valid product. That artifact is the source of truth—not the first MP4. If your team already reviews structured documents before exporting, this complements the workflow in [I Review VideoJSON Before It Reaches the Render Queue](https://the-lean-ecommerce.gitlab.io/2026/10/05/i-review-videojson-before-it-reaches-the-render-queue/).

## 3. Add a preview-and-approval gate

Rendering is the expensive, irreversible-looking step from a stakeholder’s perspective. Put the approval gate before it.

Mount the saved document in a live preview using the DOM renderer, or open it in the [React Video Editor](https://videoflow.dev/react-video-editor) when a marketer needs to adjust copy, timing, or media. The key is that the preview and editor consume the same VideoJSON your worker will render. A reviewer should see the exact title, price, image crop, and CTA that the queue will export.

Give reviewers only two actions:

1. **Approve** moves the job to `approved`.
2. **Request changes** returns it to `building` with a concrete note, such as “use the square image” or “shorten the benefit to one line.”

The expected result is that no product video reaches the renderer because someone noticed a typo after export. For an adjacent embedded-editor implementation, see [Comment intégrer un éditeur vidéo React dans votre application SaaS](https://outils-et-tutoriels.github.io/2026/10/07/comment-integrer-un-editeur-video-react-dans-votre-application-saas/).

![A review checkpoint before an automated render](/assets/img/posts/2026-10-09-how-to-build-a-video-rendering-queue-from-product-data/image-02-6499413d431b.webp)

## 4. Choose the renderer based on where the work belongs

Once a job is approved, choose one renderer boundary and keep it consistent for that class of work.

Use the browser renderer when a user is exporting a short, private video from inside your app and you want to avoid uploading source project data. Use the server renderer when a worker must process catalog batches, scheduled campaigns, API requests, or retries. The [VideoFlow Renderers documentation](https://videoflow.dev/renderers) describes both paths; they share the same VideoJSON input.

For a server worker, lock a job before rendering and make completion idempotent:

```ts
const job = await jobs.claimNext({ state: "approved" });
if (!job) return;

try {
  await jobs.update(job.id, { state: "rendering" });
  const mp4 = await renderOnServer(job.videoJson);
  const outputUrl = await storage.put(`${job.id}.mp4`, mp4);
  await jobs.update(job.id, { state: "complete", outputUrl });
} catch (error) {
  await jobs.update(job.id, { state: "failed", error: String(error) });
}
```

The expected result is one export per approved job, with a visible terminal state. Do not delete the VideoJSON after completion; it is what makes a correction or a new locale a re-render instead of a rebuild.

## 5. Design retries around data problems, not blind repetition

Separate transient render failures from invalid inputs. A timeout or temporary storage error can retry with backoff. A missing image, unsupported media type, or absent title should fail fast and return to the data owner.

Give every job an idempotency key such as `productId + templateVersion + locale + campaignId`. This prevents a worker restart from producing three identical videos for the same product. Record the renderer version and output URL too. Those details make batch investigations much shorter.

When you need more human confidence before scaling, start with ten products and compare the previewed JSON to the delivered MP4. The same review-first principle appears in [I Made AI Video Drafts Reviewable Before They Hit the Render Queue](https://the-lean-ecommerce.github.io/2026/10/08/i-made-ai-video-drafts-reviewable-before-they-hit-the-render-queue/).

![A calm batch-rendering system with completed video outputs](/assets/img/posts/2026-10-09-how-to-build-a-video-rendering-queue-from-product-data/image-03-0c2ba6f7108f.webp)

## 6. Ship one narrow batch, then widen the template

Your first success metric should be operational: every eligible product creates one reviewable document, every approved job renders once, and every completed job has a retrievable MP4. Only then add localized copy, alternate hooks, aspect ratios, or campaign variants.

This is where a JSON-first pipeline earns its keep. One template can produce many constrained variations while preserving a reviewable source of truth. You can add more fields over time without turning the queue into a collection of manual timelines.

## Common pitfalls

- **Rendering directly from a live catalog query:** snapshot the fields used for a job so a mid-render price change does not create an untraceable result.
- **Treating the MP4 as the source:** keep VideoJSON with the job so revisions can be previewed and re-rendered.
- **Using one retry rule for everything:** retry infrastructure failures; send bad inputs back for correction.
- **Skipping the first-batch review:** a small approved sample exposes crop, copy, and timing problems before they multiply.

## Next action

Create one `product-card-v1` job for a single real product, persist its VideoJSON, and require approval before calling a renderer. Once that loop is boring and observable, point it at a controlled catalog batch. [Explore VideoFlow’s Core and renderer options](https://videoflow.dev/core) to build the first template.
