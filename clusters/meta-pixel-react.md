# Meta Pixel for React

A Meta Pixel in a React app fires from the client. Create React App, Vite, and similar setups behave like a single-page app: the first load can send PageView, and later screens often do not, unless you send it on route change.

People land here from: meta pixel react, facebook pixel react, react meta pixel, meta conversions api react, facebook pixel react app.

## Browser events

Initialize once, near the root, after the pixel script is available. On each route change, send PageView. Send ViewContent when a product or pricing screen is actually shown.

Map clicks to official names. A "Book a demo" button is `Lead` or `Schedule`, not a custom event called `BookDemo`. A pricing view is `ViewContent` with `content_name: "pricing"`. The list is on [events](meta-pixel-events.md).

## Server events

React itself has no server. If the app talks to an API you can edit (Next, a Node backend, Supabase edge functions, your own webhook), Purchase, Lead, and CompleteRegistration go there, after the write succeeds. That is the [Conversions API](meta-conversions-api.md) side. Share `event_id` with the browser call. See [deduplication](meta-event-deduplication.md).

If the React app is only a frontend and the backend is closed, Tyton can still add the browser container. It should not promise server events it cannot install.

## A failure that looks like "React ate the pixel"

The pixel script is in `index.html`, the effect that calls `fbq` runs before the script loads, and every event throws or no-ops. Or Strict Mode double-invokes an effect and you send two PageViews with two random ids. Initialize once, and use a stable `event_id` for anything that represents a real conversion.

Broken setups: [Meta Pixel not working](meta-pixel-not-working.md). Next.js, including server components, has its own page: [Meta Pixel for Next.js](meta-pixel-nextjs.md).

Audit and setup: [usetyton.com](https://usetyton.com).
