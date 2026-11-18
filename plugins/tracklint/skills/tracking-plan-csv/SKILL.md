---
name: tracking-plan-csv
description: Create, repair or validate a tracking plan in Tracklint's CSV format for GA4, Google Ads, Meta Pixel or TikTok Pixel. Use when the user asks for a tracking plan or expected-event list for these platforms, or asks to diagnose or correct a Tracklint CSV plan.
---

# Tracking plan in Tracklint's CSV format

A Tracklint plan is a CSV with one row per event the site is expected to send. The Tracklint browser extension compares a recorded journey against it. This edition of the Tracklint plugin contains skills only: it starts no MCP server. Check the plan yourself against references/plan-format.md and label the result as a manual check. This workflow does not invoke Tracklint MCP tools. Never present a manual check as tool output.

## Steps

Do not run an installation, script or external checker to check the plan, and do not send it to any website or
service. Explain the available options only if the user asks.

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
3. **Check it by hand** against `references/plan-format.md`: header and columns, platforms, counts, rules, consent types
   and directives.
   - An omitted ID is allowed. Check the format of an ID only when one is given. Flag unexpected custom events or
     malformed supplied IDs; ask the user only when their intended meaning cannot be inferred.
   - Check that the `consent_mode` and Google Ads purchase directives match what the user intends.
   - Report what you could not verify (for example an unlisted column alias) instead of claiming the plan is fully
     checked. If the user wants an automatic check, point them to the free web validator:
     https://tracklint.netlify.app/tools/tracking-plan-csv-checker.html
4. **Reply** with the CSV in a code block, then a short list: what each row expects, which placeholders to replace and
   any assumption you made (counts, consent, URLs).

## What a checked plan means

A plan with no errors only means that it is well formed. It doesn't mean the site sends those events or that the IDs
are real. To test the site, the user imports the plan in the Tracklint extension (**Import plan (CSV)** in its DevTools
panel), clicks **Reload and record** and goes through the journey. Tracklint checks the requests the browser sends; it
doesn't verify processing inside Google Analytics, Google Ads, Meta or TikTok, server-to-server integrations or legal
compliance. Say so when the user asks whether their tracking "works".

## Data

The plan passes through this conversation. Suggest keeping customer data, real email addresses and secrets out of
plans, notes included.
