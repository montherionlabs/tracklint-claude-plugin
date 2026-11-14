---
type: llm
---

PASS if the reply contains a CSV tracking plan that has: a GA4 purchase row that requires transaction_id; a Meta Purchase row; consent types on the rows (analytics_storage for GA4 rows, ad_storage for Meta rows); and counts written only as once, at_least_once, any or never (or left empty).
FAIL if there is no CSV, if a row uses another count word (such as "twice" or "always") or a platform other than ga4, meta, tiktok or google_ads, or if the GA4 purchase row does not require transaction_id.
