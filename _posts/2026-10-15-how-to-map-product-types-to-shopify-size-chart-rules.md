---
layout: post
title: "How to Map Product Types to Shopify Size Chart Rules"
description: "Build a clear Shopify size-chart rule map so each product type gets the right guidance without manual product-page edits."
date: 2026-10-15 06:28:42 +0000
categories: [how-to]
tags: [shopify, size-chart, apparel-ecommerce, product-pages]
canonical_url: ""
image: "/assets/img/posts/2026-10-15-how-to-map-product-types-to-shopify-size-chart-rules/cover-37ece80b637b.webp"
---

A Shopify catalog rarely has one fit logic. A relaxed cotton tee, a tailored trouser, and a running shoe may all use familiar size labels, but shoppers need different measurements and different instructions to choose confidently. This guide shows how to turn that reality into a small, maintainable set of Shopify size-chart rules. You need access to your products, collections, or tags and a theme template where you can add an app block.

[Supra Size Chart](https://supra-size-chart.sktch.io/) is useful here because you can create charts centrally, add a reusable measurement guide, and target each chart with rules rather than pasting tables into product descriptions. The expected result is simple: a newly added product receives the right chart when its catalog data matches the rule.

## 1. List the fit families before touching a chart

Start with product behavior, not every individual SKU. Open **Products** in Shopify and make a short list of fit families such as standard-fit tops, oversized tops, denim, tailored bottoms, dresses, footwear, and accessories. A fit family can share one chart only when the meaningful measurements and the shopper's measuring method are the same.

For example, a tee chart may need chest width and body length, while a trouser chart needs waist, hip, rise, and inseam. Do not put both into one broad “apparel” chart just because they share a collection. If a shopper has to guess which columns apply, the chart is doing less than it should.

The expected result is a compact map: each product family has a named chart, a measurement guide, and a catalog signal that identifies it. If you are uncertain whether a product deserves a separate chart, this earlier guide on [choosing the right measurements](https://tools-and-how-tos.github.io/2026/10/11/how-to-choose-the-right-measurements-for-a-shopify-size-chart/) gives a useful sanity check.

![Product categories mapped to separate size-chart rules](/assets/img/posts/2026-10-15-how-to-map-product-types-to-shopify-size-chart-rules/image-01-16d3c927de37.webp)

## 2. Choose one reliable rule signal per family

Next, decide what Shopify data should activate each chart. In Supra Size Chart, rules can target a product, collection, product type, vendor, or tag. Prefer the signal your team already maintains consistently.

- Use **product type** when it expresses the fit family cleanly, such as `T-Shirts` or `Running Shoes`.
- Use a **tag** when the fit distinction cuts across types, such as `fit-oversized` or `denim-stretch`.
- Use a **collection** when a merchandised group reliably shares a chart, such as a uniform program.
- Use a **product** rule only for a genuine exception: a one-off silhouette, a technical item, or a special collaboration.

Avoid stacking several vague signals for the same decision. The chart assignment should be explainable in one sentence: “Products with the `fit-oversized` tag use the oversized-top chart.” The expected result is a rule your merchandising team can apply while creating a product, without opening the theme editor. For the collection approach, see [how to assign one Shopify size chart to a whole collection](https://how-to.the-lean-ecommerce.com/2026/09/10/how-to-assign-one-shopify-size-chart-to-a-whole-collection/).

## 3. Build the chart and measurement guide together

Create the chart in the spreadsheet-style editor and add only the columns a shopper can use. Use a consistent unit in each column and include a brief note if the values are garment measurements rather than body measurements. Then pair it with a labelled measurement guide.

A guide is where the chart becomes actionable: show exactly where chest, waist, hip, or inseam is measured. For footwear, explain whether the reference is foot length or an insole measurement. Keep the instruction beside the chart rather than expecting a shopper to infer it from a number.

Supra Size Chart supports reusable guides, so the same trouser silhouette can serve several trouser charts without copying artwork or instructions. The expected result is that each table row has a visual measuring context, not only a grid of values.

![Glowing garment measurement guide with labeled measurement lines](/assets/img/posts/2026-10-15-how-to-map-product-types-to-shopify-size-chart-rules/image-02-6567cc3f37a6.webp)

## 4. Add the rule and define an intentional fallback

Create the rule for the catalog signal you chose, then decide what should happen when a product is not matched. A store-wide default chart can be useful for a tightly focused catalog, but it is risky for a mixed catalog because a wrong chart looks more authoritative than no chart.

For a store selling tops, bottoms, and accessories, it is usually better to use explicit rules for the size-sensitive groups and leave accessories without a chart unless they need one. If you do use a default, test it against a product outside your main collection. The expected result is that an unmatched product does not silently inherit misleading guidance.

This same discipline matters for kits and multipacks; use the [size-chart approach for product bundles](https://how-to.the-lean-ecommerce.com/2026/10/05/how-to-build-shopify-size-charts-for-product-bundles/) when components do not share a single fit story.

## 5. Turn on the theme app block and test real products

In **Online Store → Themes → Customize**, open the product template and add the Supra Size Chart theme app block where shoppers make their decision—usually near the variant picker or add-to-cart area. Choose inline, accordion, or modal display based on the amount of information. Then preview one product from every fit family, plus one deliberately unmatched product.

Check four things: the intended chart appears, the measurement guide matches the garment, metric and imperial conversion behaves as expected when enabled, and the chart stays readable on a phone. Run the same check before a collection launch using this [Shopify size-chart pre-launch checklist](https://how-to.the-lean-ecommerce.com/2026/10/03/how-to-run-a-shopify-size-chart-pre-launch-check/).

The expected result is a chart that appears automatically where it belongs, with no manual HTML pasted into product descriptions.

![Size-chart rule quality check across a product catalog](/assets/img/posts/2026-10-15-how-to-map-product-types-to-shopify-size-chart-rules/image-03-f2eb36e7e491.webp)

## 6. Keep the map portable as your catalog changes

Document the chart name, the rule signal, and the owner for each fit family in a simple operating note. When a new collection launches, decide which existing family it belongs to before publishing products. Export charts as CSV or JSON when you want an external record; because Supra Size Chart stores charts in Shopify metaobjects, the sizing system remains part of your store data.

Start with the three families that generate the most sizing questions, build one rule for each, and preview the results on mobile. Then [install Supra Size Chart](https://apps.shopify.com/supra-size-chart) to replace repeated product-description tables with an owned, rule-based system.
