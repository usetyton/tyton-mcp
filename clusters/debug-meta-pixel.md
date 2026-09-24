# Debug Meta Pixel

Debugging a Meta Pixel means proving one specific event left the site and arrived on the dataset you think you are watching. The Meta Pixel debugger, Facebook Pixel debugger, Test Events, and diagnostics are different screens. They answer different questions.

People land here from: meta pixel debugger, facebook pixel debugger, meta pixel test events, meta pixel diagnostics, debug meta pixel, meta pixel event testing, how to test meta pixel events, how to verify meta capi events.

## Test Events

Events Manager → your dataset → Test Events. You get a test event code like `TEST12345`.

- Browser: open the site with the pixel helper, or send the test code the way your setup documents it, and browse the page from that same browser.
- Server: include `test_event_code` on the Conversions API request.

Events show up on that screen within seconds. They do not represent live ad optimization. If Test Events is empty, you are either not sending the code, sending it to another Pixel ID, or the request never left the machine.

## Live overview

The dataset overview is live traffic. It lags. A successful test does not appear there. "No activity" on the overview, while Test Events is moving, means the live path is fine to keep testing. "No activity" on both means the event is not being sent.

## Diagnostics

Diagnostics lists Meta's complaints: missing parameters, dropped events, domain mismatches, low match quality. Read the named event and the named parameter. A Purchase missing `value` or `currency` is a different fix from a pixel that never loads. Match quality has its own page: [Event Match Quality](meta-event-match-quality.md).

## A check that matches the money

Pick one real order id.

1. Confirm the backend stored the order.
2. Confirm the server event used `event_id` `order_<that id>` and event name `Purchase`.
3. Confirm the browser used the same id, if it sent a copy.
4. Find that id in Test Events or in your own gateway log.
5. Only then look at Ads Manager totals. Totals hide one bad order inside a day of traffic.

Duplicate rows on the test screen can still be one conversion. See [deduplication](meta-event-deduplication.md). A missing purchase that exists in Stripe is [Meta Pixel not working](meta-pixel-not-working.md), usually the server event.

Tyton keeps the event long enough to compare browser and server copies, and the audit tells you what the public page is actually loading. Run it from [usetyton.com](https://usetyton.com) through `https://mcp.usetyton.com/mcp`.
