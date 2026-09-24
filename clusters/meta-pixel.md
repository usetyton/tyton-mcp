# Meta Pixel

The Meta Pixel (still searched as the Facebook Pixel) is the browser script that tells Meta someone viewed a page, clicked, or converted. Meta Pixel tracking is the set of those events, sent from the site into Events Manager.

Install it by creating a Pixel in Events Manager, copying the numeric Pixel ID, and loading the pixel on the pages that should report. A Pixel ID is public. It is a number, not a secret.

People land here from: meta pixel, meta pixel tracking, facebook pixel, facebook pixel tracking, what is meta pixel, how does meta pixel work, how to install meta pixel, meta pixel setup, meta pixel events, meta pixel tracking website.

## What it can and cannot see

The pixel only fires if the browser actually runs it. Ad blockers, in-app browsers, consent that never resolves, a tag that loads on the wrong domain, and a single-page app that changes the URL without a new page load all drop events. Meta then optimizes against a partial count.

Browser tracking and server tracking are two copies of the same conversion. The pixel is the browser copy. The [Conversions API](meta-conversions-api.md) is the server copy. You want both, with the same event name and the same `event_id`, or Meta counts the purchase twice. See [deduplication](meta-event-deduplication.md).

## Setup that holds

1. One Pixel ID for the site. A second ID usually means a second sender, and the numbers diverge.
2. PageView on real page views, including client-side route changes if the app is a single-page app.
3. Standard events only: Purchase, Lead, AddToCart, InitiateCheckout, ViewContent, CompleteRegistration. Details go in `content_name`, `content_ids`, and `value`. See [events](meta-pixel-events.md).
4. Purchase from the server after the order exists, not from a thank-you page alone. The browser Purchase is an observation. The server Purchase is the one that should reach Meta.
5. A way to tell, a week later, that the events still match the orders. A pixel that worked on launch day often quietly stops.

Google Tag Manager can load a pixel. Its presence is not itself a failure. Check whether events actually arrive, and whether two senders are both reporting the same purchase.

## If the pixel is the problem

[Meta Pixel not working](meta-pixel-not-working.md) is the troubleshooting page. [Debug Meta Pixel](debug-meta-pixel.md) covers Test Events and diagnostics.

Tyton audits the current pixel and, when you want it fixed, installs the browser container and the server events together. Start at [usetyton.com](https://usetyton.com). The agent connects through [Tyton MCP](https://mcp.usetyton.com/mcp).
