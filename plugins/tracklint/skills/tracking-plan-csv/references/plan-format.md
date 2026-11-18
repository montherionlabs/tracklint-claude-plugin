# Tracklint plan format

This is the format that the Tracklint extension, its web validator and the `tracklint_check_plan` tool read. This
edition of the plugin doesn't include the tool: check plans by hand against this page. The sections about what the
tool returns describe that tool only.

## File

- CSV separated by commas, semicolons or tabs. The separator is detected from the first line that isn't a comment.
  Excel in Spanish and other European locales saves with semicolons; that works.
- Quote a cell that contains the separator, a quote or a line break: `"currency in MXN,USD"`. A quote
  inside a quoted cell is written twice (`""`).
- Empty lines are ignored. Lines that start with `#` are comments or directives.
- Up to 500 rows and 256 KiB with the tool.

## Header row

The first row that isn't a comment names the columns. Names are case-insensitive, any order, each column once
(aliases count as the same column).

| Column | Common aliases | Required | Content |
|---|---|---|---|
| `platform` | `plataforma` | yes | `ga4`, `google_ads`, `meta` or `tiktok` (also `google analytics`, `ga`, `google ads`, `adwords`, `meta pixel`, `facebook`, `fb`, `tiktok pixel`) |
| `event` | `event_name`, `evento` | yes | The event name: GA4 `purchase`, Meta `Purchase`, TikTok `CompletePayment` or `Purchase`, Google Ads `conversion` |
| `id` | `measurement_id`, `pixel_id` | no | GA4 `G-XXXXXXXXXX`, Meta pixel (digits), TikTok pixel (uppercase letters and digits), Google Ads `AW-123456789` (all actions of the account) or `AW-123456789/LABEL` (one action, its `send_to`) |
| `required` | `required_params`, `obligatorios` | no | Parameters that must be present, separated by `;` (or spaces). In a comma-separated file, don't separate them with commas unless the cell is quoted |
| `rules` | `reglas` | no | Rules separated by `;` (see below) |
| `count` | `veces`, `frecuencia` | no | `once` (exactly once in the recording), `at_least_once` (the default when empty), `any` (optional) or `never` (must not be sent). Also `1`, `0`, `optional` |
| `url` | `page`, `pagina` | no | Pages where the row applies. Without `*`: the page URL must contain the text. With `*`: the whole URL must match, `*` meaning anything |
| `consent` | `consentimiento` | no | Consent types the event needs: `ad_storage`, `analytics_storage`, `ad_user_data`, `ad_personalization`, separated by `;`, `|` or spaces |
| `note` | `notes`, `nota`, `description` | no | Free text shown with the row |

The aliases shown here are not exhaustive. An unlisted spelling is not sufficient evidence that a plan is invalid;
without the tool, report that spelling as unverified.

## Rules

A rule names a parameter and a check. A rule path starts with an ASCII letter or underscore, followed by ASCII
letters, digits, underscores or dots. Several rules go in the same cell, separated by `;`.

| Rule | Example |
|---|---|
| present / absent | `transaction_id present`, `user_data absent` |
| number | `value is number` |
| ISO 4217 currency | `currency is iso4217` |
| comparison (`=`, `!=`, `>`, `>=`, `<`, `<=`) | `value > 0`, `currency = MXN`. `>` and `<` need a number. A quoted value is text: `transaction_id != "0"` |
| text | `coupon contains SUMMER`, `transaction_id starts with WC-`, `ends with`, `like` (with `*` as a wildcard) |
| list | `currency in MXN|USD`, `payment_type not in test|dummy` |
| every item | `each item has item_id|item_name` |

Plans don't accept regular expressions.

## Directives

A comment line of the form `# key: value` is a directive. Unknown keys are ignored, so a plain comment should not
contain a colon.

| Directive | Effect |
|---|---|
| `# name: My store plan` | The plan's name |
| `# consent_mode: advanced` (or `basic`) | Declares the site's intended Google Consent Mode. The MCP tool reports this setting; it does not evaluate consent in a journey. In the extension, for consent types required by a plan row, denied ad_storage or analytics_storage on GA4 is informational unless basic is declared. For Google Ads, basic makes denied ad_storage an error; other denied signals remain informational. A Google Ads cookieless-ping finding additionally requires an observed cookieless transport. |
| `# google_ads_purchase: AW-123456789/LABEL` | The Google Ads conversion actions that are purchases, compared with GA4 purchases. Several can be separated by spaces, commas, `;` or `|` |

Empty directive values are ignored. consent_mode also accepts básico, basico and avanzado. After checking a plan,
verify that consentMode and googleAdsPurchaseActions reflect the intended directives.

With the tool, a comment line is limited to 2 048 characters and can't contain 32 or more spaces in a row.

## Errors returned by `tracklint_check_plan`

Each error has a `code` and a `line`. Fix them all; `valid` is `true` only with no errors and at least one row.
Some row errors stop further checks on that row; multiple invalid rules can produce multiple bad_rule errors on the
same line. Check the plan again after correcting it.

| Code | Meaning | Fix |
|---|---|---|
| `missing_header` | No header with `platform` and `event` | Add the header row |
| `unclosed_quote` | A cell opens a quote that never closes | Close the quote or remove it |
| `extra_cells` | The row has more cells than the header, usually a list written with commas | Separate the list with `;`, or quote the cell |
| `bad_platform` | Unknown platform | Use `ga4`, `google_ads`, `meta` or `tiktok` |
| `missing_event` | Empty event | Write the event name |
| `bad_count` | Unknown count, such as `twice` | Use `once`, `at_least_once`, `any` or `never` |
| `bad_consent` | Unknown consent type | Use the four types above |
| `bad_rule` | A rule that doesn't follow the grammar, or `>`/`<` with text | Rewrite it as in the rules table |
| `bad_directive` | `consent_mode` with a value other than `basic` or `advanced` | Fix the value |
| `bad_ads_purchase` | `google_ads_purchase` without a valid `AW-ID/LABEL` | Use the action's `send_to` |
| `other` | Any other plan error | Re-read the row against this page |

The call itself fails, with `ok: false` and one of these codes, when the plan can't be read as a whole:
`PLAN_DUPLICATE_HEADER` (a column twice, aliases included), `PLAN_TOO_MANY_ROWS`, `PLAN_COMMENT_REJECTED`,
`REQUEST_TOO_LARGE`, `RESPONSE_TOO_LARGE` (split the plan) or `PROCESSING_TIMEOUT`.

Argument errors are INVALID_ARGUMENTS, UNKNOWN_ARGUMENT, MISSING_ARGUMENT, INVALID_ARGUMENT_TYPE and
INVALID_ARGUMENT_VALUE. Processing can also fail with PROCESSING_LIMIT_EXCEEDED, WORKER_FAILED or INTERNAL_ERROR.

## What the tool returns per row

`platform`, `event.name` (only standard names; custom ones come back as `custom: true` and a length), `id.present`
and `id.formatValid` (never the ID itself), required parameters, the type of each rule, `count`, the kind of URL
filter and the consent types. It never returns the plan's text, notes or values.
