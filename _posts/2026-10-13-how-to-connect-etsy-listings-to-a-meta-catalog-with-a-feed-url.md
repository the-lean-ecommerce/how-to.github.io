---
layout: post
title: "How to Connect Etsy Listings to a Meta Catalog With a Feed URL"
description: "Connect Etsy listings to a Meta catalog with a scheduled feed URL, then validate the setup before tagging products on Instagram and Facebook."
date: 2026-10-13 10:33:39 +0000
categories: [how-to]
tags: [etsy, meta-catalog, instagram-shopping, product-feed, ecommerce]
canonical_url: ""
image: "/assets/img/posts/2026-10-13-how-to-connect-etsy-listings-to-a-meta-catalog-with-a-feed-url/cover-a71d3ecd879b.webp"
---

If you sell on Etsy and want your listings to appear in a Meta catalog, the hard part is not adding every item. It is creating one dependable connection that can refresh as your listings change. In this guide, you will connect an Etsy shop to a Meta catalog with a feed URL, set a refresh schedule, and test the result before relying on product tags.

You will need an Etsy shop, a Meta business setup connected to the Facebook Page and Instagram account you use for the shop, and access to the Catalog Generator app. Catalog Generator creates a URL Meta can retrieve as a data feed, so the catalog does not depend on a manual spreadsheet upload.

![Domain verification checkpoint for an Etsy catalog connection](/assets/img/posts/2026-10-13-how-to-connect-etsy-listings-to-a-meta-catalog-with-a-feed-url/image-01-02e1898b0941.webp)

## 1. Confirm the accounts and shop domain first

Open Meta Business settings and confirm that your Facebook Page and Instagram professional account belong to the same business. If Instagram is still a personal account, switch it to a business account before you continue; this is the account type that gives you the commerce and insight tools needed for the workflow.

Next, identify the domain that points to your Etsy shop. In Meta Business settings, go to **Brand Safety** and then **Domains**, choose **Add**, and enter your shop-specific Etsy domain in the format shown by your Etsy shop URL. Meta will provide a verification meta tag. In Etsy Shop Manager, open **Settings** and then **Facebook Shops**, paste the tag, and complete the connection. Return to Meta and select **Verify**.

**Expected result:** the domain shows as verified in Business settings. Do not create the catalog feed until this is settled: product destinations and product tagging depend on a domain Meta can approve.

## 2. Generate the catalog feed URL

Sign in to [Catalog Generator for Etsy](https://catalog-generator.webyze.com/) and connect the Etsy shop you want to publish. The app provides a catalog URL for that shop. Treat this URL like an integration credential: copy the exact URL, keep it in your setup notes, and do not try to download a one-time CSV in its place. The advantage of the URL is that Meta can retrieve the current listing data on a schedule.

If you are choosing between a URL feed and manual uploads, use the URL when listings, prices, inventory, or images change regularly. The same principle applies if you are planning a broader social catalog rollout; our guide to [syncing Etsy listings to an Instagram catalog with a feed](https://how-to.the-lean-ecommerce.com/2026/10/10/how-to-sync-etsy-listings-to-an-instagram-catalog-with-a-feed/) explains why a refreshable source is easier to operate than repeated file imports.

**Expected result:** you have one feed URL from Catalog Generator and can identify which Etsy shop it represents.

![Product data feed moving from an Etsy storefront into a catalog grid](/assets/img/posts/2026-10-13-how-to-connect-etsy-listings-to-a-meta-catalog-with-a-feed-url/image-02-cf43a78a4483.webp)

## 3. Add the URL as a Meta data feed

In Meta Commerce Manager, open or create the catalog that will contain the Etsy products. Go to **Data sources**, choose **Add items**, then select **Data feed**. When asked how you want to add items, choose the URL option rather than uploading a file. Paste the Catalog Generator URL into the feed field.

Use a clear feed name, such as `Etsy shop catalog feed`, so it remains obvious later which source is safe to edit. Keep optional fields empty unless your specific setup requires them. Then choose a daily retrieval schedule. A daily schedule is a sensible default for an active Etsy shop: it avoids stale catalog data without turning a routine listing edit into a manual catalog task.

If you need a walk-through of the screens, Catalog Generator also provides a [setup video](https://www.youtube.com/watch?v=2ChLE92Cu4s).

**Expected result:** Commerce Manager accepts the URL and displays a scheduled data source instead of an uploaded file.

## 4. Wait for the first import, then inspect a small sample

Save the feed and let Meta retrieve it. The first import can take several minutes, especially for a larger shop. When the job finishes, open the catalog's **Items** section and inspect a small, deliberate sample: one newly added listing, one listing with variations if you use them, and one older listing with established images. Check that the title, price, availability, image, and destination link describe the same Etsy item.

This is the moment to catch catalog drift before it reaches a product tag. If an Etsy listing changes after the feed is running, use the same sample check after the next refresh rather than assuming the integration repaired itself. That lightweight habit is the practical version of a [catalog drift check after Etsy listing changes](https://the-lean-ecommerce.gitlab.io/2026/10/10/i-run-a-drift-check-after-etsy-listings-change/).

**Expected result:** the sample items look complete and their destinations lead to the intended Etsy listings. If prices or images are missing, fix the source listing first, then wait for the next scheduled fetch instead of patching the catalog item by item.

![Quality gate reviewing product cards before catalog sync](/assets/img/posts/2026-10-13-how-to-connect-etsy-listings-to-a-meta-catalog-with-a-feed-url/image-03-8752fce3d612.webp)

## 5. Submit the domain and test product tagging

In Commerce Manager, open the catalog settings and submit the verified Etsy domain for approval if it is not already approved. Once Meta has approved it, create one test post or story with a catalog item. Confirm that the tag resolves to the same listing you inspected in the catalog. Do this with a low-stakes item first, not a seasonal best seller.

For a useful operating rhythm, write down the feed URL, the daily refresh time, and the three-item sample you checked. If you later need to change refresh behavior, [schedule the Etsy catalog feed deliberately in Commerce Manager](https://how-to-blog.gitlab.io/2026/10/12/how-to-schedule-an-etsy-catalog-feed-refresh-in-meta-commerce-manager/) rather than guessing which setting controls it.

## Common setup mistakes

- **Using a file upload instead of the feed URL.** A file may import once but will not give you the automatic refresh that makes this workflow valuable.
- **Verifying the wrong domain.** Use the Etsy shop-specific domain that redirects to your storefront, not an unrelated website.
- **Testing only one perfect listing.** Include a new item and a variant-heavy item in your review sample.
- **Tagging before approval.** A catalog can contain items before the domain is approved for the shopping experience you want.

## Next action

Your first goal is not a huge social-commerce launch. Generate the Catalog Generator feed URL, add it to Meta as a scheduled data source, and verify three items after the first import. Once those items are accurate and the domain is approved, you have a repeatable path for product tags that keeps pointing customers back to your Etsy shop.
