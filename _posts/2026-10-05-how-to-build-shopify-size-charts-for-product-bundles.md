---
layout: post
title: "How to Build Shopify Size Charts for Product Bundles"
description: "Build clearer Shopify size charts for coordinated sets and bundles by separating garment measurements, assigning the right chart, and checking the storefront result."
date: 2026-10-05 18:32:17 +0000
categories: [how-to]
tags: [shopify, size-chart, product-bundles, apparel, ecommerce]
canonical_url: ""
image: "/assets/img/posts/2026-10-05-how-to-build-shopify-size-charts-for-product-bundles/cover-df3ee1d48108.webp"
---

If you sell a coordinated hoodie-and-jogger set, a linen shirt-and-short set, or a multi-piece outfit, one generic size grid creates a quiet problem: shoppers cannot tell which numbers belong to which garment. This guide walks through a practical setup for a bundle product in Shopify. You need the final product records, the garment samples you will measure, and access to your theme editor.

By the end, a shopper opening a set will see the measurements that matter for both pieces, instead of being asked to infer fit from a single catch-all table. [Supra Size Chart](https://supra-size-chart.sktch.io/) is a free Shopify app that keeps those charts centrally managed and places them on the right product pages with rules.

## 1. Decide whether the set needs one chart or two

Start with the question the shopper is actually trying to answer: *Will the top fit me, and will the bottom fit me?* A coordinated set may share size labels, but its garments do not share measurements.

Use one chart when every size label maps cleanly to a single, easy-to-read row and both items are intentionally sold as one fit. Split the information when the shopper needs separate garment measurements—such as chest and body length for the top, then waist, hip, rise, and inseam for the bottom.

For example, a size M lounge set can be presented as one size label with two clearly labelled sections: **Top garment measurements** and **Bottom garment measurements**. Do not mix a top chest width and a trouser inseam in a column simply called “Length.” The column label should say what was measured and, when useful, whether it is a flat garment measurement.

If shoppers choose top and bottom sizes independently, do not use a bundle chart as a workaround. Give each independently chosen product its own appropriate chart.

![Separate garment measurements for a coordinated top and bottom](/assets/img/posts/2026-10-05-how-to-build-shopify-size-charts-for-product-bundles/image-01-e2258820cd07.webp)

## 2. Measure the real production samples

Lay a representative sample flat and measure the same points for each size. Record the method before you enter any numbers. That makes the chart reviewable by whoever updates the next production run.

For a simple sweatshirt-and-jogger example, your top section might include chest width, body length, and sleeve length. Your bottom section might include waist width, hip width, rise, and inseam. If a measurement is taken flat, say so near the chart; a shopper should not have to guess whether “34 cm waist” means circumference or the width across the garment.

Include a short tolerance note when production variation matters. Keep it factual—for example, “Measurements are taken flat; allow for normal manufacturing variation”—rather than promising an exact fit. For a deeper decision on whether a number should describe the body or the garment, use the [Supra Size Chart guide to body versus garment measurements](https://supra-size-chart.sktch.io/blog/body-vs-garment-measurements).

## 3. Build the chart in one central place

In Supra Size Chart, create a chart in the spreadsheet-style editor. Add your size rows, set units for each column, and add concise notes where a measurement needs context. You can import an existing CSV when your merchandising team already maintains the source in a spreadsheet; the app also supports JSON export and import for a portable record.

For a set with separate top and bottom sections, make the boundaries obvious in your chart structure. A reliable pattern is to create the top chart first and a second chart for the bottom when the table would otherwise become too wide on mobile. Give both a consistent name, such as “Core Fleece Set — Top” and “Core Fleece Set — Bottom.”

Add a labelled measurement guide when the garment shape benefits from it. A guide helps a shopper connect “chest width” or “rise” to the actual garment, rather than asking them to reverse-engineer a grid of numbers. Supra Size Chart lets you reuse measurement guides across charts, so the same silhouette can stay consistent across a family of sets.

## 4. Choose the assignment rule deliberately

Next, choose the narrowest stable Shopify attribute that identifies these sets. In the app, a rule can target a product, collection, product type, vendor, or tag, and the most specific matching rule wins.

For a small collection of seasonal sets, a dedicated collection rule is often easier to maintain than setting up each product manually. For a single flagship bundle, use a product rule. If every coordinated set carries a durable tag such as `co-ord-set`, a tag rule can cover future products as they arrive—but only if your publishing process applies that tag consistently.

Avoid assigning the same chart broadly to “pants” or “tops” just because the bundle contains both. A bundle needs its own measurement logic. The app’s store-wide default is useful as a fallback, not as a substitute for a chart that answers the product-specific question.

![Rule-based size chart routing for product bundles](/assets/img/posts/2026-10-05-how-to-build-shopify-size-charts-for-product-bundles/image-02-d29aa14edb3a.webp)

## 5. Place the theme app block where the shopper will use it

Open Shopify’s theme editor, select the product template used by the bundle, and add the Supra Size Chart theme app block. Then choose the display treatment that fits the product page: inline table, accordion, or a modal trigger.

For a set with two measurement sections, an accordion can keep the initial product page compact while still letting shoppers open the information before they choose a size. An inline table can work when the product page is already sparse and the chart is short. A modal keeps the page visually quiet, but it adds an extra action. If you are deciding among those treatments, compare them with the [inline, accordion, and modal guide](https://supra-size-chart.sktch.io/blog/size-chart-display-inline-accordion-modal).

Keep the app block close to the size selector. A shopper should not scroll past reviews, shipping copy, and unrelated accordions to answer a sizing question.

## 6. Test the chart like a shopper

Before publishing, open the actual set product on desktop and mobile. Confirm that the expected chart appears, the labels identify the correct garment, and the units make sense for your buyers. If you sell internationally, enable the metric/imperial option only after checking that the source units and their labels are clear.

Test a product that should match the rule and one that should not. Then test the fallback case. This is especially important when you reuse collection or tag rules, because a later catalog change can affect which chart renders. The [Shopify size-chart pre-launch check](https://how-to.the-lean-ecommerce.com/2026/10/03/how-to-run-a-shopify-size-chart-pre-launch-check/) is a useful final pass for the storefront result.

![Final quality assurance check for a bundle size guide](/assets/img/posts/2026-10-05-how-to-build-shopify-size-charts-for-product-bundles/image-03-9e8c553028b5.webp)

## 7. Make the next update easier

When a supplier revises a pattern or a new colorway uses a different fit block, update the central chart rather than editing product-description HTML. That preserves one source of truth and makes it easier to export the data when your team needs a spreadsheet or archive. For larger catalog changes, the same idea—define the product set before touching records—also applies to [bulk Shopify catalog cleanup](https://the-lean-ecommerce.com/blog/i-split-a-shopify-catalog-cleanup-into-three-reversible-bulk-tasks-Psm7+daKgaqBtsyq0Ny8Sw).

A good bundle size guide does not need more numbers. It needs the right numbers, clearly separated by garment, attached to the correct product, and checked in the storefront. Start with one live set, measure it consistently, and publish the chart with [Supra Size Chart](https://apps.shopify.com/supra-size-chart).
