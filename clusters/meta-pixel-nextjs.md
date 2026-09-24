# Meta Pixel for Next.js

A Meta Pixel on Next.js has two problems the pixel snippet does not mention: the App Router does not reload the page on navigation, and the purchase usually completes in a server action, route handler, or webhook. The browser tag alone misses both.

People land here from: meta pixel nextjs, facebook pixel nextjs, nextjs meta pixel, next js facebook pixel, meta conversions api nextjs, facebook conversions api nextjs, meta capi nextjs.

## App Router

Load the pixel once in the root layout. On a client-side navigation, send PageView yourself when the pathname changes. A `next/script` tag in `layout.tsx` does not do that for you.

```tsx
"use client";
import { usePathname } from "next/navigation";
import { useEffect } from "react";

export function MetaPageView() {
  const pathname = usePathname();
  useEffect(() => {
    if (typeof window.fbq === "function") window.fbq("track", "PageView");
  }, [pathname]);
  return null;
}
```

The base pixel script still has to be on the page before this runs. ViewContent belongs on the product or pricing view, with `content_name` and `content_ids`.

## Pages Router

A route change also skips a full load. Fire PageView from `routeChangeComplete` on the router, and keep the base pixel in `_app` or `_document`.

## Purchase and other confirmed events

Do not treat `/checkout/success` as proof. Send Purchase from the place that records the paid order: a Stripe webhook, a server action after the write succeeds, or the route handler that creates the order. Use one `event_id`, `order_<id>`, in that server event and in the browser pixel if the browser also fires.

```ts
// server, after the order is stored
await tyton.track("Purchase", {
  eventId: `order_${order.id}`,
  value: order.total,
  currency: "USD",
});
```

Lead and CompleteRegistration follow the same rule: fire them when the row is saved, not when the button is clicked.

The Conversions API token stays in server environment variables. The Pixel ID may be public and can ship to the client.

## What to read next

[Conversions API](meta-conversions-api.md), [server-side tracking](meta-server-side-tracking.md), [deduplication](meta-event-deduplication.md), [events](meta-pixel-events.md). If events are missing on a Next app you already shipped, use [Meta Pixel not working](meta-pixel-not-working.md).

Tyton's agent reads the Next.js app and places the container plus the server calls. Start at [usetyton.com](https://usetyton.com). MCP: `https://mcp.usetyton.com/mcp`.
