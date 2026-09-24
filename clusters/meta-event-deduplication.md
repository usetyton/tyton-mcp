# Meta event deduplication

Meta event deduplication keeps a browser pixel event and a Conversions API event from counting as two conversions. Duplicate Meta purchase events almost always mean the two copies did not share an `event_id`.

People land here from: meta event deduplication, meta pixel duplicate events, facebook pixel duplicate events, meta capi deduplication, meta pixel capi deduplication, meta event id, meta duplicate purchase events.

## The rule

Same `event_name` and same `event_id` within Meta's dedup window equals one conversion. Either mismatch equals two.

Derive the id from the business object, on both sides, with the same formula:

- Order `1001` → `order_1001` in the pixel and in CAPI
- Lead row `884` → `lead_884` on both
- Do this independently. Do not generate a random id in the browser and hope the server hears it
- Do not rely on `fbp` or `external_id` to collapse two Purchase events

The browser call:

```js
fbq("track", "Purchase", { value: 49, currency: "USD" }, { eventID: "order_1001" });
```

The server event uses `"event_id": "order_1001"` and `"event_name": "Purchase"`. `eventID` in the pixel and `event_id` in CAPI are the same value.

When there is no business object (a PageView with nothing to attach), the container may generate an id. Purchases, leads, and subscriptions always have an object. Use it.

## What people misread in Ads Manager

- Two rows in Test Events can still be one ad conversion, if the names and ids match. Test Events shows both copies on purpose.
- Doubled purchases in reporting means dedup failed, or a second pixel ID is also sending Purchase.
- A missing purchase, after you "fixed" duplicates by deleting the server event, means you turned off the copy that actually survives ad blockers.

Pixel + CAPI without duplicate conversions is the goal. Dropping one side is how the count goes quiet again. See [Meta Pixel not working](meta-pixel-not-working.md) and [Conversions API](meta-conversions-api.md).

Tyton's gateway and browser container are built around this rule: one derived `event_id`, official event names, Purchase owned by the server. Set that up from [usetyton.com](https://usetyton.com).
