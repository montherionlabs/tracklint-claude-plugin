---
name: tracking-plan-csv
description: Create, repair or validate a tracking plan in Tracklint's CSV format for GA4, Google Ads, Meta Pixel or TikTok Pixel. Use when the user asks for a tracking plan or expected-event list for these platforms, or asks to diagnose or correct a Tracklint CSV plan.
---

# Tracking plan in Tracklint's CSV format

A Tracklint plan is a CSV with one row per event the site is expected to send. The Tracklint browser extension
compares a recorded journey against it. The `tracklint_check_plan` tool of the Tracklint MCP server checks the plan
itself: columns, platforms, counts, rules and consent types.

## Steps

Check tool availability before starting. If the Tracklint tool is unavailable, use the manual-reading
instructions below and label the result as manual. Do not claim a tool result or run an alternative
installation or external checker automatically. Explain available options if the user asks.

1. **Scope the plan.** Find out, asking only for what you can't infer:
   - the kind of site (online store, lead form) and the journey to test, for example product page, cart, checkout and
     order confirmation;
   - the platforms in use: `ga4`, `google_ads`, `meta`, `tiktok`;
   - whether a consent banner sets Google Consent Mode, and in which mode (`basic` or `advanced`).
   IDs are optional. When the user doesn't give them, leave the `id` column empty or use clear placeholders such as
   `AW-123456789/PURCHASE-LABEL`, and say which ones to replace. Never ask for real customer data.
2. **Draft the CSV** following `references/plan-format.md`. Start from the closest example in `references/examples.md`.
   Use standard event names: GA4 recommended events, Meta standard events, TikTok standard events, and `conversion`
   for Google Ads rows (with `AW-ID/LABEL` in `id`).
3. **Check it.**
   - If tracklint_check_plan is available, call it with the complete CSV when creating, repairing or validating the
     plan. Correct reported plan errors (codes and lines in `references/plan-format.md`) and recheck only after changing
     the CSV. If rowsCount is 0 and errors is empty, explain that at least one event row is required. On ok: false,
     follow the error guidance; do not retry unchanged input indefinitely.
   - An omitted ID is allowed: id.present: false is not an invalid-ID finding. Review id.formatValid only when
     id.present is true. Flag unexpected custom events or malformed supplied IDs; ask the user only when their intended
     meaning cannot be inferred.
   - Check that consentMode and googleAdsPurchaseActions match the intended directives. Report unresolved
     discrepancies instead of claiming the plan is fully checked.
   - If the tool isn't available (for example in claude.ai chat, where local MCP servers don't run), check the plan by
     hand against `references/plan-format.md` and label the result as manual. If the user asks how to check it
     automatically, explain the options: this plugin in Claude Code or in a Cowork session on their computer, or the
     free web validator at https://tracklint.netlify.app/tools/tracking-plan-csv-checker.html
4. **Reply** with the CSV in a code block, then a short list: what each row expects, which placeholders to replace and
   any assumption you made (counts, consent, URLs).

## What "valid" means

`valid: true` means the plan reader found no errors. It doesn't mean the site sends those events or that the IDs are
real. To test the site, the user imports the plan in the Tracklint extension (**Import plan (CSV)** in its DevTools
panel), clicks **Reload and record** and goes through the journey. Tracklint checks the requests the browser sends; it
doesn't verify processing inside Google Analytics, Google Ads, Meta or TikTok, server-to-server integrations or legal
compliance. Say so when the user asks whether their tracking "works".

## Data

The tool runs on the user's computer and returns codes, positions, counts and lengths, never the text of the plan.
The plan itself still passes through this conversation. Suggest keeping customer data, real email addresses and
secrets out of plans, notes included.
