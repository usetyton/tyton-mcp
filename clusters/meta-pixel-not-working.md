# Meta Pixel not working

If the Meta Pixel is not working, Events Manager is quiet while the site is getting orders, leads, or traffic. Facebook Pixel not working is the same problem. The pixel tag may be present and still not tracking.

People land here from: meta pixel not working, meta pixel not tracking, facebook pixel not working, how to fix meta pixel tracking, meta pixel events not showing, meta pixel purchase event not firing, meta pixel pageview not firing, meta pixel not tracking purchases, meta pixel stopped working, meta pixel tracking wrong, meta pixel missing events, meta pixel diagnostics errors, meta pixel not detected, meta pixel not showing activity.

Check these in order. Stop at the first one that explains the gap.

## 1. The browser never ran the pixel

View the page source or the network panel and look for the pixel request (`fbevents.js`, or a call to `facebook.com/tr`). No request means the script is missing, blocked, or waiting on consent that never resolves.

Also check the Pixel ID in the tag against the dataset you are watching in Events Manager. A tag for a different ID looks like "no activity" on the one you have open.

## 2. The page type never fires the event

- Purchase on a thank-you URL the checkout does not visit
- PageView bound to a full load, while the app is client-side routing
- Lead bound to a button click, while the form fails server-side and still "fires"
- AddToCart in a component that unmounts before the call

A Purchase that only exists in the browser will also vanish when the buyer closes the tab before the page loads. That purchase belongs on the server. See [server-side tracking](meta-server-side-tracking.md).

## 3. Events Manager is looking at the wrong place

Test Events shows events sent with a test code. The overview shows live events, often with a delay. Diagnostics lists implementation warnings. "Not showing" sometimes means you are on Test Events with no test code, or on a dataset that is not the one in the tag.

Use [debug Meta Pixel](debug-meta-pixel.md) for that split.

## 4. Two senders, or a dedup miss

The pixel and the Conversions API can both be right and still look wrong: purchases doubled, or purchases missing because one side sends a different event name. See [deduplication](meta-event-deduplication.md). A Google Tag Manager pixel plus a hardcoded pixel is a common second sender. Confirm both IDs before removing either.

## 5. The event is the wrong name

Meta will not treat `Signup` or `ClickedPricing` as a standard event. Use `CompleteRegistration`, `Lead`, or `ViewContent` with `content_name`. The list is on [events](meta-pixel-events.md).

## What Tyton does here

Ask for an audit of the site. Tyton MCP (`https://mcp.usetyton.com/mcp`) inspects the public page and the tracking it can see, without an account. If you want it fixed, the paid setup at [usetyton.com](https://usetyton.com) has the agent install the pixel, the Conversions API, and a shared `event_id`, then check that events arrive.

Price only if you ask: $17 a month for one site, $49 a month for up to three.
