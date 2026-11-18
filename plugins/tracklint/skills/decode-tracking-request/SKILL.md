---
name: decode-tracking-request
description: Decode or troubleshoot one GA4, Google Ads conversion, Meta Pixel or TikTok Pixel browser tracking request that the user asks to inspect. Work from its URL and optional body copied from DevTools or one selected HAR entry. Supported endpoints include GA4 /g/collect, Google Ads /pagead/conversion/ and /pagead/1p-conversion/, Meta facebook.com/tr and TikTok /api/v2/pixel and /api/v2/pixel/act.
---

# Decode a tracking request

This edition of the Tracklint plugin contains skills only: it starts no MCP server. Read the request yourself, following the steps below, and label the result as a manual reading. This workflow does not invoke Tracklint MCP tools. Never present manual findings as tool output.

The requests covered are GA4 requests to a `/g/collect` path (server-side tagging endpoints included), Google Ads
conversions sent by gtag.js (`www.googleadservices.com/pagead/conversion/`, `www.google.com/pagead/1p-conversion/` and,
with ad storage denied, `pagead2.googlesyndication.com/pagead/conversion/`), Meta Pixel requests to `facebook.com/tr`
and TikTok Pixel requests to `/api/v2/pixel`.

## Steps

Do not run an installation, script or external checker to decode the request, and do not send it to any website or
service. Explain the available options only if the user asks.

1. **Get the request.** Ask for the request URL and, for a POST, its body (in Chrome or Edge DevTools: Network tab,
   select the request, Headers for the URL, Payload for the body). Before they paste it, suggest replacing real
   customer emails, phone numbers and names with test values: the request passes through this conversation.
2. **Identify it.** From the host and path, name the platform. If it isn't GA4, a Google Ads conversion, Meta Pixel or
   TikTok Pixel, say so. Other Google Ads requests (view-through, remarketing in `rmkt/collect`, page measurement in
   `ccm/collect` or the `gtag/js?id=AW-…` library) can be named from their address only; don't decode what they carry.
   A GA4 batch carries one event per line of the body. The same Google Ads conversion can leave through two of the
   addresses above: read each request on its own.
3. **Read each event.** For each event: platform, event name (say whether it is a standard name), whether the
   measurement, pixel or Google Ads account ID is present and looks well formed, and each parameter by position, with
   amounts and quantities as numbers and currencies as ISO 4217 codes. When a GA4 or Google Ads request carries `gcs`
   or `gcd`, describe the consent state the Google tag wrote in the request, which is not necessarily what the user
   chose in the cookie banner.
4. **Check it** against the list in `references/codes.md`, used as a checklist: purchases without an order or event ID,
   Google Ads conversions without their own transaction ID, values without a currency, invalid currency codes, unhashed
   advanced matching data, and emails, phone numbers, WhatsApp links or credential-like values sent to the platform.
   You may use the code names to label what you find, saying that they come from a manual reading.
5. **Explain each finding** in plain language: what it means, why it matters and what to check on the site. Group
   repeated findings. Point to the event and parameter by position.
6. **Recommend next steps.** Recommend only the next steps needed to resolve the user's question. If the user asks to
   test a complete browser journey, explain that this requires separate capture and validation. Mention the Tracklint
   browser extension only when relevant to that request.

## Rules for the reply

- Don't repeat personal data, IDs or text values from the request in your reply, even if the user pasted them. Refer
  to them by position ("parameter 4 of event 1").
- Don't claim what happens inside Google Analytics, Google Ads, Meta or TikTok: a request shows what the browser sent,
  not how the platform processed it, and the reading is no legal assessment. A request does not show whether Google
  Ads deduplicated or counted a conversion.
- Say that the reading is manual. If the user asks how to check requests automatically, explain that the Tracklint
  browser extension for Chrome and Edge records and checks them in DevTools: https://tracklint.netlify.app
