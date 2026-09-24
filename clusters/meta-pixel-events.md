# Meta Pixel events

Meta only treats its own event names as standard events. `Signup`, `ClickedPricing`, and `OrderComplete` do not count as Purchase or Lead. Put the extra detail in fields: `content_name`, `content_ids`, `value`, `currency`.

People land here from: meta pixel purchase event, meta pixel lead event, meta pixel add to cart, meta pixel initiate checkout, meta capi purchase event, and the same phrases with "facebook pixel" or "meta conversions api" in front of the event name.

Use the same name on the pixel and on the Conversions API. Dedup compares name plus `event_id`. See [deduplication](meta-event-deduplication.md).

## Which event, and who sends it

| What happened | Event name | Send it from |
|---|---|---|
| A page loaded, including a client-side route change | PageView | Browser |
| A product, pricing, or article view | ViewContent | Browser. Include `content_name` and `content_ids` |
| A search on the site | Search | Browser |
| Added a product to the cart | AddToCart | Browser, or server if the cart is stored server-side |
| Started checkout | InitiateCheckout | Server, when checkout is actually created. A button click is not checkout |
| A contact, demo, or lead form was saved | Lead | Server, after the save succeeds |
| An account was created | CompleteRegistration | Server, after the account exists |
| A newsletter or paid plan started renewing | Subscribe | Server, after the subscription is confirmed |
| Money was captured for an order | Purchase | Server, after payment is confirmed. Include `value` and `currency` |

`Schedule` and `Contact` exist too. Use them when they match the outcome more closely than Lead. Ask which outcome is which before merging "signup" into one event. A newsletter signup and an account registration are different.

## Purchase

```js
fbq("track", "Purchase", { value: 49, currency: "USD", content_ids: ["sku_1"] }, { eventID: "order_1001" });
```

The server sends `event_name: "Purchase"` and `event_id: "order_1001"` after the payment is confirmed. A failed or cancelled payment does not send Purchase. The thank-you page can observe it. The server owns delivery. See [server-side tracking](meta-server-side-tracking.md).

## Lead

Fire Lead when the enquiry is stored, not on click and not because the URL contains `?success=1`. A query parameter is not proof.

## AddToCart and InitiateCheckout

AddToCart is the cart change. InitiateCheckout is the start of payment, and it still counts if the person never pays. Do not send Purchase for either.

If an event is not showing up, the cause is usually the trigger, not the name. [Meta Pixel not working](meta-pixel-not-working.md). To test one event end to end, [debug Meta Pixel](debug-meta-pixel.md).

Tyton's tracking plan is this table, filled in for the site you actually have. The agent maps buttons and backend success to these names, then installs the calls. [usetyton.com](https://usetyton.com).
