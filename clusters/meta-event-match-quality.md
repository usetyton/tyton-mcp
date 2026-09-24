# Meta Event Match Quality

Meta Event Match Quality is Meta's score for how well a server event can be tied to a person Meta already knows. Facebook Event Match Quality and Meta CAPI Event Match Quality are the same screen, on the dataset, next to the event.

People land here from: meta event match quality, facebook event match quality, improve meta event match quality, meta capi event match quality, event match quality facebook.

## What actually moves the score

Match quality rises when the event carries identifiers Meta can use. The useful ones, in roughly this order:

- Email, hashed with SHA-256, lowercase, trimmed (`em`)
- Phone, hashed, digits only with country code (`ph`)
- Click ID from the ad click (`fbc`) and browser ID (`fbp`), passed through to the server event
- Client IP address and user agent, from the request that belonged to that visitor
- External ID, if you have a stable first-party id, hashed

A Purchase with only `value` and `currency` will send, and it will match badly. Meta can still count it. It is worse at attaching it to the person who clicked the ad.

Hash on the way in. Do not store the plaintext email to "improve match quality later."

## Why the score stays low

- The server event is fired from a webhook that no longer has the browser's `fbc`, IP, or user agent, and nobody saved them on the order
- Email is hashed with the wrong normalization (uppercase, or with the name still attached)
- The pixel and CAPI events are deduped, but the server copy is the one missing `user_data`, and the browser copy was blocked
- Test events from your own laptop, with no ad click, scored as if they were traffic

`fbc` only exists if the visitor arrived from a Meta ad and the click id was kept until the conversion. Organic purchases will not have it. That is expected, not a broken pixel.

## Where Tyton fits

Tyton hashes email and phone on arrival and sends the server event with the match fields you actually captured. It does not invent identifiers. If the checkout never stored `fbc` or the email, the score cannot be faked.

Audit and setup: [usetyton.com](https://usetyton.com). Related: [deduplication](meta-event-deduplication.md), [server-side tracking](meta-server-side-tracking.md), [debugging](debug-meta-pixel.md).
