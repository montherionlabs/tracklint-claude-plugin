---
type: llm
---

PASS if the reply says that the order ID is not the store's own: the transaction ID starts with GG_, an ID that Google generates when the tag sends none, so the conversion carries no advertiser-provided order ID, and it recommends passing the store's order ID to the conversion tag.
FAIL if the reply says the order ID is fine, if either point is missing, or if it claims to have verified whether Google Ads deduplicated, counted or attributed the conversion.
