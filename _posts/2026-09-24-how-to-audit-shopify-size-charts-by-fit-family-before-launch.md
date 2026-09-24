---
layout: post
title: "How to Audit Shopify Size Charts by Fit Family Before Launch"
description: "A practical pre-launch audit for Shopify apparel size charts: fit families, measurement instructions, assignment rules, and unit choices."
date: 2026-09-24 18:39:50 +0000
categories: [how-to]
tags: [shopify, size-chart, apparel, ecommerce, product-pages]
canonical_url: ""
image: "/assets/img/posts/2026-09-24-how-to-audit-shopify-size-charts-by-fit-family-before-launch/cover-29d78b13388a.webp"
---

# How to Audit Shopify Size Charts by Fit Family Before Launch

A size chart can look finished and still fail the first shopper who tries to use it. The usual problem is not a missing row. It is that one chart has quietly been asked to explain several different fits, garment constructions, or regions at once.

This guide shows you how to audit your Shopify size charts by fit family before launch. You will need your current charts, a short list of the products in scope, and access to your theme editor. By the end, each product family will have a clear chart, a usable measurement guide, and a reliable way to appear on the right product page.

![Aurora size-chart audit for Shopify apparel products](/assets/img/posts/2026-09-24-how-to-audit-shopify-size-charts-by-fit-family-before-launch/image-01-29d78b13388a.webp)

## 1. Group products by how they fit, not by how they are named

Start with the catalog, not the spreadsheet. Create a small working list and place every product into a fit family: for example, relaxed cotton tees, fitted rib tops, high-rise stretch leggings, or structured outerwear. A collection named "New arrivals" is not necessarily a fit family; two products can share a merchandising collection while needing different measurements.

Look for these signals that a family needs its own chart:

1. The garment is measured in different places, such as inseam versus body length.
2. The fit promise changes: slim, regular, oversized, cropped, or compression.
3. Fabric stretch or construction changes the choice a shopper should make.
4. A product has a different conversion or region-specific convention.

For each family, write one plain-language fit note. For example: "This relaxed tee is designed with 6 cm of ease through the chest; compare the garment chest width to a tee that fits well." That sentence gives the chart context that a grid alone cannot. It also makes the customer-service answer more consistent.

If you are already mapping sizing questions into the product page, this [product-page decision path](https://the-lean-ecommerce.gitlab.io/2026/09/23/i-turned-shopify-sizing-questions-into-a-product-page-decision-path/) is a useful companion: it helps identify the questions the chart needs to answer first.

## 2. Audit the measurements a shopper can actually take

Open one representative product from each fit family. Decide whether the chart is based on garment measurements, body measurements, or both. Do not mix them without saying so. A customer who measures their body against a flat-lay garment chart can make a very reasonable, very wrong choice.

A solid garment chart normally includes only the dimensions that change the decision. A tee might need size, chest width, body length, and sleeve length. Trousers might need waist, hip, rise, and inseam. Add a note explaining whether a width is measured flat and whether the number is a half measurement.

Then test the instruction with a real tape measure. Place a garment flat, follow the directions, and ask: could someone do this at home without knowing your internal terminology? If the answer is no, add a labelled measurement guide. A visual line at the chest, waist, or inseam is often more useful than another paragraph.

![Flat garment measurement workflow](/assets/img/posts/2026-09-24-how-to-audit-shopify-size-charts-by-fit-family-before-launch/image-02-7eac927da5e6.webp)

The goal is not a larger table. It is a smaller decision: "measure this point, compare this value, choose this size." For a broader pre-publish pass, use this [nine-check Shopify size checklist](https://outils-et-tutoriels.gitlab.io/guides/2026/09/24/checklist-taille-shopify-9-verifications-avant-de-publier-un-produit/) alongside your own QA notes.

## 3. Check every chart for missing decision context

Review each family chart with this quick checklist:

1. **Size labels:** Are they written exactly as they appear in the variant selector?
2. **Units:** Is every measurement clearly cm, inches, or another unit?
3. **Fit note:** Does the product page say whether the item is slim, regular, relaxed, or oversized?
4. **Measurement basis:** Does it state garment versus body measurements?
5. **Edge cases:** Is there a short note for stretch, layering, or between-size choices where it matters?
6. **Variant coverage:** Does every sellable size have a row?

Use an illustrative rule for your own store: if two products need different instructions to make a size choice, they probably do not belong in the same chart. This protects you from the tempting but fragile "one universal apparel chart" approach.

## 4. Assign the chart with a durable Shopify rule

Once the fit families are clear, replace product-by-product maintenance with an assignment rule. In [Supra Size Chart](https://supra-size-chart.sktch.io/), create the chart in the spreadsheet-style editor, attach the measurement guide, and assign it by product, collection, product type, vendor, or tag. Use the attribute that best represents the fit family.

For example, a tag such as `fit-relaxed-tee` can cover new and existing relaxed tees without editing the chart assignment again. A product type can work well when your catalog taxonomy is disciplined. Keep a store-wide default only as a fallback, not as a substitute for reviewing the family.

Expected result: a product added to the right collection or given the right tag receives the right chart automatically. That is much safer than pasting a table into each product description, where a later update becomes a catalog-wide cleanup.

![One size chart assigned across a Shopify collection](/assets/img/posts/2026-09-24-how-to-audit-shopify-size-charts-by-fit-family-before-launch/image-03-625884527cd7.webp)

This same "configure once, apply consistently" principle also helps with adjacent product-page media. If you are adding 3D content, read [how to test one Shopify 3D model before scaling product media](https://the-lean-ecommerce.github.io/2026/09/24/how-i-test-one-shopify-3d-model-before-scaling-product-media/) before rolling it across a whole collection.

## 5. Put the chart where the shopper will look for it

In Shopify, open **Online Store > Themes > Customize**, select a representative product template, and add the Supra Size Chart theme app block near the variant picker. Choose an inline table when the chart is compact, an accordion when the product page has dense selling copy, or a modal when the guide deserves focused space.

Preview a product from every fit family on desktop and mobile. The expected result is simple: the right chart appears, the size labels match the variants, the guide is legible, and the block fits the theme without a separate styling project. Supra Size Chart uses theme-native styling and server-rendered markup, so the chart can stay close to the purchase decision without a heavy widget.

## 6. Make metric and imperial a launch decision

If you sell across borders, do not make customers convert measurements in another tab. Decide which unit is primary for the family, then enable a shopper-facing metric/imperial option where it is useful. Check a few values manually after conversion, especially for small accessories where rounding can change the apparent fit.

![Metric and imperial size-chart readiness](/assets/img/posts/2026-09-24-how-to-audit-shopify-size-charts-by-fit-family-before-launch/image-04-889fb2822464.webp)

Before launch, export a copy of your charts as CSV or JSON. With Supra Size Chart, the chart data is stored as your store's Shopify metaobjects and is exportable, so your sizing system remains portable as the catalog evolves.

## Finish with one representative product

Choose one product from each fit family and run the full shopper path: select a size, open the chart, follow the guide, switch units if relevant, and confirm that the wording matches the fit promise. If that experience is clear, roll the rule across the rest of the family.

[Install Supra Size Chart from the Shopify App Store](https://apps.shopify.com/supra-size-chart) to build centrally managed, rule-based charts with measurement guides, display options, unit conversion, and CSV/JSON export. It is free, with unlimited charts and assignment rules.

A launch-ready size chart is not just accurate data. It is a small, repeatable decision tool that appears on the correct product, explains what to measure, and gives shoppers enough confidence to choose once.
