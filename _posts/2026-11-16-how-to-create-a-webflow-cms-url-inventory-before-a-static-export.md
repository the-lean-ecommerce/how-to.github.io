---
layout: post
title: "How to Create a Webflow CMS URL Inventory Before a Static Export"
description: "Create a complete Webflow CMS URL inventory before a static export so you can verify routes, assets, forms, and redirects before changing hosting."
date: 2026-11-16 15:37:54 +0000
categories: [how-to]
tags: [webflow, cms, static-hosting, website-migration, exflow]
canonical_url: ""
image: "/assets/img/posts/2026-11-16-how-to-create-a-webflow-cms-url-inventory-before-a-static-export/cover-5b9bd3204166.webp"
---

# How to Create a Webflow CMS URL Inventory Before a Static Export

A Webflow CMS static export is only trustworthy when you can prove which public URLs were supposed to come across. Before you export, make a small URL inventory that separates core pages, collection-item routes, and utility pages; then use it as the acceptance test for the exported copy. You need access to the published site and, ideally, its sitemap.

[ExFlow’s Webflow exporter](https://exflow.site/webflow) is useful here because it can crawl a published Webflow site, include public pages and CMS item pages, and rewrite downloaded assets to local paths. It does not replace the inventory: native form handling, Ecommerce checkout, site search, password-protected pages, and the Webflow Editor still need a replacement or a deliberate exception.

![Glowing route inventory flowing into a verified manifest](/assets/img/posts/2026-11-16-how-to-create-a-webflow-cms-url-inventory-before-a-static-export/image-01-7111569bae6d.webp)

## 1. Set the boundary for this export

Start by writing one sentence that defines the job: for example, “Move the public marketing site and its resource library to static hosting; keep checkout on its existing platform.” This prevents a common mistake: treating every URL that exists in Webflow as a page that static HTML can reproduce.

Include these decisions in the header of a spreadsheet or Markdown file:

- **Source:** the live custom domain or published `.webflow.io` domain you will export.
- **Scope:** public site pages and public CMS collections; state whether language variants are included.
- **Out of scope:** checkout, authenticated areas, search, password-protected content, and server-side form handling unless you have a replacement.
- **Target:** the host and URL pattern you expect after deployment.

The expected result is a scope statement another teammate can read without opening the Webflow Designer. If this is part of a redesign, pair it with a [content-freeze archive](https://how-to.the-lean-ecommerce.com/2026/11/05/how-to-create-a-webflow-cms-content-freeze-archive-before-a-redesign/) so you know which published state the inventory represents.

## 2. Seed the inventory from the public sitemap

Open the published sitemap in a browser—for example, `your-domain.example/sitemap.xml`—or use Webflow’s published sitemap if your domain points elsewhere. Copy each public URL into an `expected_url` column. Do not infer collection URLs from a template name alone: the live sitemap is the better starting point because it reflects what visitors can currently reach.

Make four columns alongside it: `route_type`, `must_keep`, `owner`, and `notes`. A compact route taxonomy works well:

| Route type | Example | What to check |
| --- | --- | --- |
| Core page | `/about` | Navigation, title, canonical URL |
| CMS item | `/resources/example-guide` | One output file per live item, images, related links |
| CMS listing | `/resources` | Pagination or load-more behavior, card links |
| Utility route | `/404`, `/privacy` | Host-specific handling, redirects, legal copy |

Mark only customer-facing, indexable, or contractually required routes as `must_keep`. A staging page that should never ship is more dangerous in the inventory than absent from it. The expected result is a list that is small enough to review but complete enough to catch a missing collection item.

## 3. Add a representative test set for dynamic-looking pages

A URL inventory does not have to be a full crawl report. Add a focused test set that makes breakage visible:

1. Pick at least one current and one older item from each CMS collection.
2. Add an item with a long title, an image-heavy item, and one with optional fields populated.
3. Add the deepest navigation path and a page containing embeds or custom code.
4. Record any page that links to an external form, booking tool, checkout, or analytics endpoint.

This is the step that turns “the homepage loaded” into evidence. If you are unsure where exports tend to fail, the earlier guide on [auditing a Webflow CMS site before static hosting](https://how-to.the-lean-ecommerce.com/2026/10/01/how-to-audit-a-webflow-cms-site-before-moving-to-static-hosting/) provides a useful companion checklist.

## 4. Export the published site and preserve the receipt

Run the export from the same published URL used in your inventory. In ExFlow, select the assets your site needs—CSS, JavaScript, and images/media—and enable all pages when the goal includes CMS routes. The product documents that it crawls the published site, follows public internal links and the sitemap, and exports CMS collection pages as static files.

Download the ZIP or sync the output to Git, S3, FTP, or a chosen static host. Keep the exported folder, the export time, and the inventory together. For a Git-based handoff, commit the inventory alongside the output or in the release documentation; that gives you an auditable definition of “complete.”

Do not promise that the export carries server behavior. Plan a form endpoint, keep checkout off the static host, or mark the route as out of scope. This is the same practical distinction covered in [exporting Webflow CMS pages to static HTML for GitHub Pages](https://how-to.the-lean-ecommerce.com/2026/09/20/how-to-export-webflow-cms-pages-to-static-html-for-github-pages/).

![Static export verification checks for routes assets metadata and forms](/assets/img/posts/2026-11-16-how-to-create-a-webflow-cms-url-inventory-before-a-static-export/image-02-dda9e9b1902c.webp)

## 5. Compare the export against the inventory before deployment

Serve the exported folder locally or on a private preview URL. For every `must_keep` row, record one of three outcomes: **pass**, **replacement needed**, or **excluded by design**. Check the following in order:

- The route resolves at the expected path, with the expected trailing-slash or `.html` behavior.
- The page title, meta description, canonical reference, and share image still describe the page.
- Images, fonts, scripts, and internal links load without requests back to the old Webflow asset domain.
- CMS cards lead to real exported item pages, not a template or a 404.
- Forms, search, authentication, checkout, and embeds either work through an intentional external service or appear in the exception column.
- Redirects from important old URLs have an owner and a host-specific configuration.

For a large collection, test the representative set first, then compare the count of inventory CMS URLs against the generated route files. A count match is not proof, but a mismatch tells you exactly where to investigate.

## 6. Turn exceptions into deployment work

Every **replacement needed** row should become a specific task: “point contact form to endpoint,” “configure 301 from old campaign URL,” or “retain checkout on platform domain.” Avoid a vague “test forms later” note. It cannot be accepted by the person moving DNS.

This also gives you a clean decision point for hosting. The static files may fit Git-based deployment, object storage, or a managed host; choose after you know which integrations stay external. If the site is Framer rather than Webflow, the same principle applies, but the details differ—see [how to choose static hosting for a Framer export](https://productivity-tech-business.sktch.io/home/how-to-choose-static-hosting-for-a-framer-export-without-guessing-Qam7+daKgTqV_9SsEcGp_A).

![Versioned static site release prepared for hosting handoff](/assets/img/posts/2026-11-16-how-to-create-a-webflow-cms-url-inventory-before-a-static-export/image-03-27339ad97978.webp)

## 7. Approve the inventory, then change hosting

Do not use “the ZIP downloaded” as the approval signal. Approve the release only when every must-keep URL has a pass, a named replacement, or a documented exclusion. Save that version of the inventory with the export date and commit SHA if you use Git.

Your next action is simple: copy your live sitemap into an inventory, select representative CMS items, and run the comparison on one preview export. When the inventory is clean, start an [ExFlow Webflow export](https://exflow.site/webflow) and deploy the static output with a clear record of what remains platform-dependent. ExFlow also has dedicated exporters for [Framer](https://exflow.site/framer) and [Squarespace](https://exflow.site/squarespace), but keep the inventory platform-specific.
