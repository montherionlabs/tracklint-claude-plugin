---
name: decode-tracking-request
description: Decode or troubleshoot one GA4, Google Ads conversion, Meta Pixel or TikTok Pixel browser tracking request that the user asks to inspect. Work from its URL and optional body copied from DevTools or one selected HAR entry. Supported endpoints include GA4 /g/collect, Google Ads /pagead/conversion/ and /pagead/1p-conversion/, Meta facebook.com/tr and TikTok /api/v2/pixel and /api/v2/pixel/act.
---

# Decode a tracking request

The `tracklint_decode_request` tool of the Tracklint MCP server decodes one request without sending it anywhere. It
reads GA4 requests to a `/g/collect` path (server-side tagging endpoints included), Google Ads conversions sent by
gtag.js (`www.googleadservices.com/pagead/conversion/`, `www.google.com/pagead/1p-conversion/` and, with ad storage
denied, `pagead2.googlesyndication.com/pagead/conversion/`), Meta Pixel requests to `facebook.com/tr` and TikTok Pixel
requests to `/api/v2/pixel`.

## Steps

Check tool availability before starting. If the Tracklint tool is unavailable, use the manual-reading
instructions below and label the result as manual. Do not claim a tool result or run an alternative
installation or external checker automatically. Explain available options if the user asks.

1. **Get the request.** Ask for the request URL and, for a POST, its body (in Chrome or Edge DevTools: Network tab,
   select the request, Headers for the URL, Payload for the body). Before they paste it, suggest replacing real
   customer emails, phone numbers and names with test values: the request passes through this conversation.
2. **Call the tool** with `url`, `body` (if any) and `method` (`GET` or `POST`). A GA4 batch carries one event per
   line of the body; pass the whole body. The same Google Ads conversion can leave through two of the addresses above:
   decode each request on its own (`transport` names the one decoded).
3. **Read the result.**
   - `compatible: false` with `NOT_A_SUPPORTED_REQUEST`: the request isn't GA4, a Google Ads conversion, Meta Pixel or
     TikTok Pixel.
   - `compatible: false` with `DETECTED_ONLY`: another Google Ads request (view-through, remarketing in `rmkt/collect`,
     page measurement in `ccm/collect` or the `gtag/js?id=AW-…` library). The tool names its type in `detected` and
     reads nothing it carries.
   - `NO_EVENTS_DECODED`: the endpoint is supported but no event could be read. For a Google Ads conversion, this is
     also the result when `label` is missing or doesn't have the form of a label.
   - `ok: false`: the call failed with the code in `error.code`; see `references/codes.md`.
   - For each event: platform, standard event name (custom names come back only as `custom: true` and a length),
     whether the measurement, pixel or Google Ads account ID is present and has the expected format, and each parameter
     by position, with amounts and quantities as numbers and currencies as ISO 4217 codes. A Google Ads conversion comes
     back as the event `conversion`, with `transport` (`conversion`, `1p-conversion` or `cookieless`).
   - When a GA4 or Google Ads request carries decodable consent signals, the event has `consent` with
     `source: "request_signals"`: the consent state written in the request by the Google tag (`gcs`/`gcd`), not what
     the user chose in the cookie banner. When neither signal can be decoded, and always for Meta and TikTok, `source` is `"none"`.
4. **Explain each warning** in plain language with `references/codes.md`: what it means, why it matters and what to
   check on the site. Group repeated warnings. Point to the event and parameter by position, since the tool doesn't
   return the values.
5. **Recommend next steps.** Recommend only the next steps needed to resolve the user's question. If the user asks to
   test a complete browser journey, explain that this requires separate capture and validation. Mention the Tracklint
   browser extension only when relevant to that request.

## Rules for the reply

- Don't repeat personal data, IDs or text values from the request in your reply, even if the user pasted them. Refer
  to them by position ("parameter 4 of event 1").
- Don't claim what happens inside Google Analytics, Google Ads, Meta or TikTok: the tool reads what the browser sent,
  not how the platform processed it, and it is no legal assessment. A request does not show whether Google Ads
  deduplicated or counted a conversion.
- When the tool isn't available (for example in claude.ai chat, where local MCP servers don't run), say so. You can
  still describe the parameters you can read, labelled as a manual reading and without repeating personal data. If the
  user asks how to check it automatically, explain the options: this plugin in Claude Code or in a Cowork session on
  their computer, or the Tracklint browser extension.
