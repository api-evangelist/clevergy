---
name: Embed a Clevergy microfrontend with a user JWT
description: >-
  Mint a short-lived user token server-side and render a Clevergy web component inside your own app
  or webview, without ever exposing your API key to the browser. This is Clevergy's primary
  integration path for end-user experiences.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/getting-started/authentication and
  https://docs.clever.gy/developer/microfrontends; retrieveUserAccessToken was verified present in
  openapi/clevergy-connect-api-openapi.yml.
operations:
  - retrieveUserAccessToken
---

# Embed a Clevergy microfrontend

## The three-party architecture

Clevergy documents this shape explicitly, and getting it wrong is the security failure mode:

1. **Your app** (browser / webview) — holds the Clevergy custom elements. Never holds the API key.
2. **Your API** (server) — holds the `clevergy-api-key`. Acts as a secure proxy that mints user JWTs.
3. **Clevergy Connect API** — mints the JWT and serves the data the components fetch directly.

## 1. Mint the user token, server-side

`GET /auth/{userId}/token` → **retrieveUserAccessToken**

Header: `clevergy-api-key: <tenant key>`
`{userId}` accepts the Clevergy user id **or** the user's email.

Returns an `AuthUser` carrying the JWT (RFC 7519). **It expires after 1 hour** — mint on demand from
your own authenticated session; do not cache it past expiry and do not hand out one token per
deployment.

Expose this behind your own auth as something like `GET /clevergy-token` on your server. That
endpoint must authorize the caller against the `userId` it is minting for, or you have built a
cross-customer data leak.

## 2. Load the component library

```html
<head>
  <script type="module" src="https://assets.clever.gy/clevergy-modules.js"></script>
</head>
```

One module defines all ~30 elements. Be aware the URL is **unpinned** — no version segment, and the
bundle is rebuilt frequently (observed `Last-Modified` moved the same day it was probed, with
`Cache-Control: max-age=60`). You cannot pin a known-good build, so treat the component layer as
continuously delivered and test against it rather than assuming stability.

## 3. Render the element

```html
<clevergy-energy-chart data-token="<jwt from step 1>" />
```

- `data-token` carries the JWT. Components that only render public data (e.g.
  `clevergy-energy-prices`) do not need it — check the catalog entry per component.
- Other inputs are passed as further `data-*` attributes, documented per component.
- The component fetches its own data straight from the Clevergy API. You do not pass it a payload.

Full catalog, grouped into four families with per-component docs links:
`components/clevergy-components.yml`.

## 4. Style it

CSS is isolated in both directions — your page styles do not leak in and component styles do not
leak out. Customize only through the `--clevergy` custom properties:

```css
:root {
  --clevergy-font-family: Inter, sans-serif;
  --clevergy-color-primary: #e57200;
  --clevergy-color-secondary: #004571;
  --clevergy-color-text: #004571;
  --clevergy-color-subtext: #737373;
  --clevergy-color-border: #d9d9d9;
}
```

## 5. Listen for events

Components can emit DOM events to tell the host app something happened inside them (e.g. a
contracting flow completed). Per-component events are listed in that component's catalog page.

## Alternative: the whole webview

If you do not want to compose components, the same JWT opens the entire Clevergy webview:

```
https://{tenant_name}.clever.gy/login-with-token?token=<jwt>
```

`{tenant_name}` is assigned by Clevergy on request. Same 1-hour expiry. Note the token travels in
the query string, so it will land in browser history, referrer headers and any intermediary logs —
prefer the component path with `data-token` where you have the choice.

## Refresh strategy

There is no refresh-token flow. When the hour is up, call your own token endpoint again and
re-set `data-token`. Build that in from the start — a long-lived dashboard will hit it.
