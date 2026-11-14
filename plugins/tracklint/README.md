# Tracklint for Claude

Write and check tracking plans for online stores and lead forms, and decode the tracking requests a browser sends to
GA4, Meta Pixel and TikTok Pixel. Tracklint is a QA aid for analysts, developers and agencies who need to know which
events a site should send and what a given request actually carries.

## What the plugin includes

- **`tracking-plan-csv` skill.** Claude writes, fixes or checks a tracking plan in Tracklint's CSV format for GA4,
  Google Ads, Meta Pixel and TikTok Pixel: one row per expected event, with required parameters, rules (`value > 0`,
  `currency is iso4217`, `each item has item_id|item_name`…), counts and consent types. The same plan works in the
  Tracklint browser extension and its web validator.
- **`decode-tracking-request` skill.** Claude decodes one GA4, Meta Pixel or TikTok Pixel request copied from DevTools
  and explains its warnings: purchases without an order or event ID, values without a currency, invalid currency
  codes, unhashed advanced matching data, emails, phone numbers, WhatsApp links or credential-like values sent to the
  platform.
- **Tracklint MCP server** (`@montherion/tracklint-mcp` 0.2.0). Two read-only tools, `tracklint_check_plan` and
  `tracklint_decode_request`, that the skills call.

## Where it works

| App | Skills | MCP tools |
|---|---|---|
| Claude Code | Yes | Yes |
| Cowork sessions on your computer (desktop app) | Yes | Yes |
| claude.ai chat | Yes | No local MCP tools. Claude can review plans and read supplied requests manually, and labels that work as manual. |

The MCP server needs **Node.js 22** (22.23.4 or later in the 22.x line) or **Node.js 26** (26.9.0 or later in the 26.x
line), with `npx` available. It needs no account, key or environment variable.

## Use it

Ask in your own words, for example:

- "Write a tracking plan for a WooCommerce store with GA4 and Meta Pixel, with a cookie banner in Consent Mode advanced."
- "Check this plan" and paste the CSV.
- "What's wrong with this purchase request?" and paste the URL and body of a `/g/collect` request.

Both MCP tools are free to use. The browser extension and a Tracklint Pro subscription are not required to create or
check plans or decode requests with this plugin.

To test a live site against the plan, import the CSV in the Tracklint browser extension for Chrome or Edge, in the
countries where it is available, and record the journey: https://tracklint.netlify.app

## Privacy Policy: what the plugin runs, sends and fetches

- **What runs.** The plugin starts npx -y @montherion/tracklint-mcp@0.2.0. The requested server version is fixed.
  npx may reuse a matching local installation or download the package into npm's local cache. It uses the configured
  npm registry, normally https://registry.npmjs.org/, and may contact it on subsequent starts. The -y option accepts
  npm's installation prompt. Version 0.2.0 declares no third-party package dependencies or install scripts.
- **npm.** When npm accesses its registry, npm may collect technical request data, including the requested package, IP
  address and software versions, under https://docs.npmjs.com/policies/privacy/. Tracklint does not send the inspected
  CSV or tracking request to npm.
- **What the server does.** It talks to Claude only through standard input and output. Its tools make no network
  requests, read or write no files and keep nothing between calls: each request or plan is processed in memory and
  discarded.
- **What it returns.** Codes, positions, counts, lengths, amounts and currency codes. It never returns the text of a
  plan or request: custom names, text values, URLs, IDs, emails and phone numbers come back only as their position,
  type and length or as a warning code.
- **Data collection, storage and retention.** The MCP tools process the supplied URL, body or CSV in memory to perform
  the requested analysis. They do not save inputs or results to disk or retain them between calls. Montherion Labs
  does not receive this analysis content.

  This does not describe npm's package cache or logs, or storage and retention by your Claude client and Anthropic.
  Anthropic may receive and retain the conversation, tool arguments and results under the terms and privacy settings
  applicable to your account (Anthropic's privacy policy: https://www.anthropic.com/legal/privacy). Use test data, or
  data you are allowed to share.
- **Links.** The skills mention pages of tracklint.netlify.app; they are opened only if you open them.
- **Contact.** montherionlabs@gmail.com. Full policy, including the Tracklint MCP section:
  https://tracklint.netlify.app/privacy#mcp

## Limits

- One request at a time: up to 16 KiB of URL, 256 KiB of body and 50 events. Plans: up to 500 rows and 256 KiB. Each
  limit returns an explicit error; nothing is cut silently.
- These are input ceilings, not a guarantee that every input below them produces a result. The complete response is
  limited to 128 KiB and processing to 5 seconds. The decoder also limits events to 100 parameters and user_data to 50
  keys. This plugin does not audit complete HAR files, validate recorded journeys, export reports or connect to the
  browser extension.
- Google Ads requests are not decoded by the MCP server (version 0.2.0); the Tracklint browser extension reads them.
  Plans do accept `google_ads` rows.
- Tracklint checks what the browser sends. It does not verify processing inside Google Analytics, Google Ads, Meta or
  TikTok, server-to-server integrations (Conversions API, Events API) or legal compliance, and its results are not
  legal advice.

## Licenses

The files of this plugin (manifest, skills and documentation) are under the MIT License (`LICENSE`). The MCP server
package that `npx` downloads, `@montherion/tracklint-mcp`, has its own license: free to use, including for
professional work, and not open source. See its page on npm: https://www.npmjs.com/package/@montherion/tracklint-mcp

## Support

montherionlabs@gmail.com · https://github.com/montherionlabs/tracklint-claude-plugin/issues

For security issues or reports containing private data, email montherionlabs@gmail.com. Do not post raw requests,
customer data or credentials in public issues.

**Troubleshooting.** If the tools are missing in Claude Code or local Cowork, check that the plugin is enabled and that
the documented Node.js version and npx are available to the app, then restart the session. Local tools are unavailable
in claude.ai chat by design. Report persistent failures with the app version and error code, without raw tracking data.

Google Analytics and Google Ads are trademarks of Google LLC. Meta is a trademark of Meta Platforms, Inc. TikTok and
Claude are trademarks of their respective owners. Tracklint is not affiliated with, endorsed or sponsored by Google,
Meta, TikTok or Anthropic.
