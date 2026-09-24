# Meta Conversions API

The Meta Conversions API (Facebook Conversions API, Meta CAPI) sends conversion events from your server to Meta. The browser pixel can miss the event. The server saw the order, the lead, or the signup, so the server reports it.

People land here from: meta conversions api, facebook conversions api, meta capi, meta conversions api setup, facebook conversions api setup, meta capi setup, how does meta conversions api work, meta conversions api example, meta conversions api integration.

## How a CAPI event is built

A server event needs:

- The same Pixel ID (dataset) the browser uses
- A Conversions API access token from Events Manager, generated when you are ready to send live events
- An official event name, such as `Purchase` or `Lead`
- An `event_id` shared with the browser copy, so Meta deduplicates them
- Event time, source URL, and whatever customer fields you actually have (`em`, `ph`, `fbc`, `fbp`, client IP, user agent), hashed where Meta requires it

The token is a secret. It belongs in server config, never in page code and never in an audit paste.

A minimal shape:

```http
POST https://graph.facebook.com/v21.0/{PIXEL_ID}/events
```

```json
{
  "data": [{
    "event_name": "Purchase",
    "event_time": 1710000000,
    "event_id": "order_1001",
    "action_source": "website",
    "event_source_url": "https://example.com/checkout/success",
    "user_data": { "em": ["<sha256 of email>"] },
    "custom_data": { "currency": "USD", "value": 49.00 }
  }]
}
```

The browser pixel must send `eventID: "order_1001"` for that same order. Different IDs mean two purchases in Ads Manager. See [deduplication](meta-event-deduplication.md).

## Where it sits next to the pixel

| Copy | Who sends it | When it is the right sender |
|---|---|---|
| Meta Pixel | The browser | Page views, content views, and a browser observation of the conversion |
| Conversions API | Your server | The conversion is confirmed: order paid, lead saved, signup stored |

Tyton is the path between those two. The site's server SDK posts the confirmed event to Tyton. Tyton forwards it to Meta and keeps the browser copy on the same `event_id`.

Setup walkthrough for the Pixel ID itself: [Meta Pixel](meta-pixel.md). Server-side context: [server-side tracking](meta-server-side-tracking.md). Next.js specifically: [Meta Pixel for Next.js](meta-pixel-nextjs.md).

Start at [usetyton.com](https://usetyton.com). Connect the agent at `https://mcp.usetyton.com/mcp`.
