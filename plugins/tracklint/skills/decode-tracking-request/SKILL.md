---
name: decode-tracking-request
description: Decode or troubleshoot one GA4, Meta Pixel or TikTok Pixel browser tracking request that the user asks to inspect. Work from its URL and optional body copied from DevTools or one selected HAR entry. Supported endpoints include GA4 /g/collect, Meta facebook.com/tr and TikTok /api/v2/pixel and /api/v2/pixel/act.
---

# Decode a tracking request

The `tracklint_decode_request` tool of the Tracklint MCP server decodes one request without sending it anywhere. It
reads GA4 requests to a `/g/collect` path (server-side tagging endpoints included), Meta Pixel requests to
`facebook.com/tr` and TikTok Pixel requests to `/api/v2/pixel`.

## Steps

Check tool availability before starting. If the Tracklint tool is unavailable, use the manual-reading
instructions below and label the result as manual. Do not claim a tool result or run an alternative
installation or external checker automatically. Explain available options if the user asks.

1. **Get the request.** Ask for the request URL and, for a POST, its body (in Chrome or Edge DevTools: Network tab,
   select the request, Headers for the URL, Payload for the body). Before they paste it, suggest replacing real
   customer emails, phone numbers and names with test values: the request passes through this conversation.
2. **Call the tool** with `url`, `body` (if any) and `method` (`GET` or `POST`). A GA4 batch carries one event per
   line of the body; pass the whole body.
3. **Read the result.**
   - `compatible: false` with `NOT_A_SUPPORTED_REQUEST`: the request isn't GA4, Meta Pixel or TikTok Pixel. Google Ads
     requests (`googleadservices.com`, `/pagead/`) are not decoded by this tool; the Tracklint browser extension reads
     them.
   - `ok: false`: the call failed with the code in `error.code`; see `references/codes.md`.
   - For each event: platform, standard event name (custom names come back only as `custom: true` and a length),
     whether the measurement or pixel ID is present and has the expected format, and each parameter by position, with
     amounts and quantities as numbers and currencies as ISO 4217 codes.
   - GA4 events also carry `consent` with `source: "request_signals"`: the consent state written in the request by the
     Google tag (`gcs`/`gcd`), not what the user chose in the cookie banner. Meta and TikTok requests carry no consent
     state (`source: "none"`).
4. **Explain each warning** in plain language with `references/codes.md`: what it means, why it matters and what to
   check on the site. Group repeated warnings. Point to the event and parameter by position, since the tool doesn't
   return the values.
5. **Recommend next steps.** Recommend only the next steps needed to resolve the user's question. If the user asks to
   test a complete browser journey, explain that this requires separate capture and validation. Mention the Tracklint
   browser extension only when relevant to that request.

## Rules for the reply

- Don't repeat personal data, IDs or text values from the request in your reply, even if the user pasted them. Refer
  to them by position ("parameter 4 of event 1").
- Don't claim what happens inside Google Analytics, Meta or TikTok: the tool reads what the browser sent, not how the
  platform processed it, and it is no legal assessment.
- When the tool isn't available (for example in claude.ai chat, where local MCP servers don't run), say so. You can
  still describe the parameters you can read, labelled as a manual reading and without repeating personal data. If the
  user asks how to check it automatically, explain the options: this plugin in Claude Code or in a Cowork session on
  their computer, or the Tracklint browser extension.
