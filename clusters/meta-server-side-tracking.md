# Meta server-side tracking

Meta server-side tracking sends the conversion from your backend after the business event is real. Facebook server-side tracking and Meta Pixel server-side tracking are the same job: the [Conversions API](meta-conversions-api.md), paired with the browser pixel.

People land here from: meta server side tracking, facebook server side tracking, meta pixel server side tracking, server side tracking meta, server side tracking facebook ads, meta browser vs server tracking, meta server events, meta server side events.

## Why the browser count is short

The pixel runs in the visitor's browser. It misses people when:

- An ad blocker or tracking protection stops the script
- The in-app browser (Instagram, Facebook) drops the request
- Consent loads the pixel late, or never
- The thank-you page is skipped, or the app navigates without a full load
- The purchase happens in a webhook, after the tab is already closed

The server still has the order. That is the event Meta should learn from. Browser vs server is not a choice of one. Send both, deduped. See [event deduplication](meta-event-deduplication.md).

## A layout that matches the sale

```
Browser
  Tyton container
    Meta Pixel (browser event)
    a copy to the Tyton gateway
Server
  Tyton SDK, after the order or lead is saved
    POST to the Tyton gateway
      gateway → Meta Conversions API
```

Purchase is a server event. The browser may also observe it. The server copy is the one that owns delivery to Meta. Clicks and form-submit attempts are not purchases and not leads.

PageView and ViewContent can stay on the browser. Lead, CompleteRegistration, Subscribe, and Purchase should fire when the backend confirms success.

Next.js and React apps are the usual place this gets wired. [Next.js](meta-pixel-nextjs.md), [React](meta-pixel-react.md). If the stack is Webflow, Framer, or Shopify and there is no backend the agent can edit, the browser container is the part Tyton can add. Do not promise a server event there.

If events are already missing, start with [Meta Pixel not working](meta-pixel-not-working.md), then [usetyton.com](https://usetyton.com).
