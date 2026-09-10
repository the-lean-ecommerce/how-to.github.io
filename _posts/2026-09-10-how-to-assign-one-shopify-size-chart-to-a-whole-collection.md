---
layout: post
title: "How to Assign One Shopify Size Chart to a Whole Collection"
description: "Create one reusable Shopify size chart, add a measurement guide, and assign it to a collection so new matching products stay covered."
date: 2026-09-10 12:31:12 +0000
categories: [how-to]
tags: [shopify, size-chart, apparel, product-pages]
canonical_url: ""
image: "/assets/img/posts/2026-09-10-how-to-assign-one-shopify-size-chart-to-a-whole-collection/cover-bcb6d7f93ecf.webp"
---

## What You Will Set Up

This guide shows how to create one Shopify size chart, pair it with a measurement guide, and assign it to an entire collection using **Supra Size Chart**. The intended result is simple: every matching product page displays the same trustworthy size information, including products you add to that collection later.

You need a Shopify store, a collection whose products share the same fit logic, and access to [Supra Size Chart](https://apps.shopify.com/supra-size-chart). Before starting, make sure the collection really does deserve one shared chart. A basic tee and an oversized coat may both be apparel, but they often need different garment measurements and fit notes.

![One luminous size chart flowing across a connected apparel collection](/assets/img/posts/2026-09-10-how-to-assign-one-shopify-size-chart-to-a-whole-collection/image-01-bcb6d7f93ecf.webp)

## Step 1: Decide What the Chart Must Explain

Open three representative products from the collection. Write down the measurements a shopper needs to make a good choice. For a top, that might include chest, body length, and sleeve length. For trousers, it may be waist, hip, rise, inseam, and leg opening.

Use garment measurements when the chart describes the item laid flat or measured directly. Use body measurements only when you state that clearly and give the customer a matching instruction. Do not quietly mix the two in one table.

Expected result: you have one coherent column list and one unit system for the collection. This prevents the chart from becoming a copied table with unclear meaning.

## Step 2: Create the Chart in Supra Size Chart

Open Supra Size Chart in Shopify Admin and create a new chart. Use the spreadsheet-style editor to add the size rows and measurement columns. Set the unit for each column, and add a note when a measurement needs extra context, such as “measured across the chest, laid flat.”

If you already maintain sizing in a spreadsheet, import the existing data as CSV rather than retyping it. Keep the source file in your own catalog documentation too. The app supports CSV and JSON import and export, which makes the chart portable rather than trapped in product-description HTML.

Expected result: one centralized chart exists in the app and can be updated without editing every product page.

## Step 3: Add a Measurement Guide

Add a measurement guide that matches the garment type. Choose a relevant silhouette, then place labelled lines where the customer should measure. The guide is not decoration; it resolves the gap between a table of numbers and a real tape measure in a shopper's hand.

For a shirt, show where chest width and body length are taken. For pants, show waist, rise, and inseam. Keep the labels consistent with the column names in the table. If the table says `Chest`, the guide should not call the same line `Bust` unless you explain the distinction.

Expected result: a shopper can understand where each chart value comes from before choosing a size.

![A measurement guide branching clearly into catalog assignment rules](/assets/img/posts/2026-09-10-how-to-assign-one-shopify-size-chart-to-a-whole-collection/image-02-8a6f17888230.webp)

## Step 4: Assign the Chart to the Collection

Create an assignment rule in Supra Size Chart and choose **Collection** as the target. Select the collection you reviewed in Step 1, then choose the chart you created.

Collection rules are useful when a group of products shares a fit system: a core tee collection, a standard denim line, or a footwear category with one manufacturer chart. The rule is matched live on the product page, so a product added to that collection later can be covered automatically.

Expected result: the chart is assigned once at the collection level instead of pasted into every product description.

## Step 5: Choose the Display Mode

Open the theme editor and add the Supra Size Chart app block to the product template. Choose the display mode that fits the product page and the amount of sizing information you need to present:

- **Inline table:** best when sizing is central to the buying decision and the page has enough room.
- **Accordion:** useful when the product page is information-dense but the chart should remain easy to find.
- **Modal trigger:** useful when you want a compact product page, but make sure the button label is clear and keyboard-accessible.

The app block adopts the theme's styling, so it can fit light and dark storefronts without a separate visual rebuild.

Expected result: the chart appears in a predictable place on each matched product page.

![Reusable size-chart data represented as a calm, structured luminous guide](/assets/img/posts/2026-09-10-how-to-assign-one-shopify-size-chart-to-a-whole-collection/image-03-2a33ceb8718c.webp)

## Step 6: Test the Rule With Real Products

Open at least three products in the assigned collection: one existing product, one product with a different size range, and one recently added item if available. Check that the expected chart displays on each product page. Then test a product outside the collection to confirm that it does not inherit the chart by accident.

If you sell internationally, enable and test the metric/imperial toggle. Confirm that the values make sense and that the chart labels explain what is being converted.

Expected result: matching products show the correct chart, while non-matching products follow their own rules or the store-wide default.

## Troubleshooting

**The chart is missing on one product:** Confirm the product is actually in the target collection and that the theme app block is present on the product template it uses.

**The wrong chart appears:** Review the assignment rules. A more-specific rule for a product, vendor, type, or tag may take precedence over the collection rule.

**The table is correct but shoppers still ask about fit:** Improve the measurement guide and add a plain-language fit note. A chart can be accurate and still be hard to interpret.

**A collection is too broad:** Split the collection into fit-consistent groups, or assign more specific rules to the exceptions. One shared chart should reduce catalog maintenance, not erase meaningful differences.

![A disconnected sizing rule being calmly restored to the correct product group](/assets/img/posts/2026-09-10-how-to-assign-one-shopify-size-chart-to-a-whole-collection/image-04-ed099a123185.webp)

## Next Step

Start with the collection that creates the most sizing questions or return risk. Create one chart, add one clear measurement guide, and test it across the actual products your customers see.

[Install Supra Size Chart](https://apps.shopify.com/supra-size-chart) to keep sizing information centralized in Shopify metaobjects, assign it through rules, and give customers a clearer answer before they choose a size.
