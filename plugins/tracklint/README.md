# Tracklint for Claude

Write and check tracking plans for online stores and lead forms, and read the tracking requests a browser sends to
GA4, Google Ads (conversions), Meta Pixel and TikTok Pixel. Tracklint is a QA aid for analysts, developers and agencies
who need to know which events a site should send and what a given request actually carries.

This skills-only edition is prepared for submission to the Claude directory. It starts no server or program on your computer.

## What the plugin includes

- **`tracking-plan-csv` skill.** Claude writes, fixes or checks a tracking plan in Tracklint's CSV format for GA4,
  Google Ads, Meta Pixel and TikTok Pixel: one row per expected event, with required parameters, rules (`value > 0`,
  `currency is iso4217`, `each item has item_id|item_name`…), counts and consent types. The same plan works in the
  Tracklint browser extension and its web validator.
- **`decode-tracking-request` skill.** Claude reads one GA4, Google Ads conversion, Meta Pixel or TikTok Pixel request
  copied from DevTools and explains what to check: purchases without an order or event ID, Google Ads conversions
  without their own transaction ID, values without a currency, invalid currency codes, unhashed advanced matching
  data, emails, phone numbers, WhatsApp links or credential-like values sent to the platform.

Claude does this work itself, following the skills, and labels it as a manual reading or check. The plugin needs no
account, key, Node.js or environment variable.

## Where it works

Claude Code, Cowork and the Claude apps (web, desktop and mobile).

## Use it

Ask in your own words, for example:

- "Write a tracking plan for a WooCommerce store with GA4 and Meta Pixel, with a cookie banner in Consent Mode advanced."
- "Check this plan" and paste the CSV.
- "What's wrong with this purchase request?" and paste the URL and body of a `/g/collect` request.
- "Does this Google Ads conversion carry the order ID?" and paste the URL of a `/pagead/conversion/` request.

The plugin is free. The browser extension and a Tracklint Pro subscription are not required to use it.

For automatic checks: the free web validator checks a plan in your browser
(https://tracklint.netlify.app/tools/tracking-plan-csv-checker.html), and the Tracklint browser extension for Chrome or
Edge, in the countries where it is available, records a journey on a live site and checks it against the plan:
https://tracklint.netlify.app

## Privacy policy

- **What runs.** Nothing. The plugin is a set of instructions (skills and their reference pages) that Claude reads. It
  starts no MCP server, script or program, and it contains no hooks, commands or agents.
- **What it sends and fetches.** Nothing. The plugin makes no network requests. The skills tell Claude not to send the
  plan or request to any website or service.
- **Data collection, storage and retention.** Montherion Labs does not receive, store or retain the plans, requests or
  conversations you use with the plugin. What you paste is processed by Claude in your conversation. Anthropic may
  receive and retain the conversation under the terms and privacy settings applicable to your account (Anthropic's
  privacy policy: https://www.anthropic.com/legal/privacy). Use test data, or data you are allowed to share, and
  replace real customer emails, phone numbers and names with test values before pasting a request.
- **Links.** The skills mention pages of tracklint.netlify.app; they are opened only if you open them. The privacy
  policy of the Tracklint website and extension is at https://tracklint.netlify.app/privacy
- **Contact.** montherionlabs@gmail.com

## Limits

- One request at a time. This plugin does not audit complete HAR files, validate recorded journeys, export reports or
  connect to the browser extension.
- A manual reading can miss what an automatic decoder would catch, especially in long or heavily encoded requests.
- A request shows what the browser sent. Tracklint does not verify processing inside Google Analytics, Google Ads,
  Meta or TikTok, server-to-server integrations (Conversions API, Events API) or legal compliance, and its results are
  not legal advice. A request does not show whether Google Ads deduplicated or counted a conversion.

## License

The files of this plugin (manifest, skills and documentation) are under the MIT License (`LICENSE`).

## Support

montherionlabs@gmail.com · https://github.com/montherionlabs/tracklint-claude-plugin/issues

For security issues or reports containing private data, email montherionlabs@gmail.com. Do not post raw requests,
customer data or credentials in public issues.

Google Analytics and Google Ads are trademarks of Google LLC. Meta is a trademark of Meta Platforms, Inc. TikTok and
Claude are trademarks of their respective owners. Tracklint is not affiliated with, endorsed or sponsored by Google,
Meta, TikTok or Anthropic.
