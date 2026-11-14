# Example plans

These are Tracklint's ready-made templates. Copy the closest one and adapt the events, the IDs and the notes.

## Online store: GA4 and Meta Pixel

```csv
platform,event,required,rules,count,consent,note
ga4,view_item,currency;value;items,currency is iso4217; value >= 0; each item has item_id|item_name,at_least_once,,Product page
ga4,add_to_cart,currency;value;items,currency is iso4217; value >= 0; each item has item_id|item_name,at_least_once,,Add to cart
ga4,begin_checkout,currency;value;items,currency is iso4217; value >= 0; each item has item_id|item_name,at_least_once,,Checkout start
ga4,purchase,transaction_id;currency;value;items,currency is iso4217; value > 0; each item has item_id|item_name,once,,Order confirmation
meta,ViewContent,content_ids;value;currency,currency is iso4217; value >= 0,at_least_once,,Product page
meta,AddToCart,content_ids;value;currency,currency is iso4217; value >= 0,at_least_once,,Add to cart
meta,InitiateCheckout,value;currency,currency is iso4217; value >= 0,at_least_once,,Checkout start
meta,Purchase,value;currency,currency is iso4217; value > 0,once,,Order confirmation. Add event_id if you also use the Conversions API
```

## Online store with a consent banner (Consent Mode advanced)

Same rows, with the consent each event needs, a `PageView` row and the directive:

```csv
# consent_mode: advanced
platform,event,required,rules,count,consent,note
ga4,view_item,currency;value;items,currency is iso4217; value >= 0; each item has item_id|item_name,at_least_once,analytics_storage,Product page
ga4,purchase,transaction_id;currency;value;items,currency is iso4217; value > 0; each item has item_id|item_name,once,analytics_storage,Order confirmation
meta,PageView,,,at_least_once,ad_storage,Every page
meta,Purchase,value;currency,currency is iso4217; value > 0,once,ad_storage,Order confirmation. Add event_id if you also use the Conversions API
```

## Online store: GA4 and Google Ads

Google Ads rows use the event `conversion` and the action's `send_to` as the ID. Replace the placeholders with the
`send_to` of each conversion action (Google Ads > Goals > Conversions).

```csv
# Replace AW-123456789/PURCHASE-LABEL and AW-123456789/LEAD-LABEL with the send_to of your conversion actions
# google_ads_purchase: AW-123456789/PURCHASE-LABEL
platform,event,id,required,rules,count,consent,note
ga4,purchase,,transaction_id;currency;value;items,currency is iso4217; value > 0; each item has item_id|item_name,once,,Order confirmation
google_ads,conversion,AW-123456789/PURCHASE-LABEL,transaction_id;value;currency,currency is iso4217; value > 0,at_least_once,,Order confirmation. Repeated sends of the same transaction_id are reported separately
google_ads,conversion,AW-123456789/LEAD-LABEL,,,once,,Form sent: the lead conversion action
```

## Online store: TikTok Pixel

```csv
platform,event,required,rules,count,consent,note
tiktok,ViewContent,value;currency,currency is iso4217; value >= 0,at_least_once,,Product page
tiktok,AddToCart,value;currency,currency is iso4217; value >= 0,at_least_once,,Add to cart
tiktok,InitiateCheckout,value;currency,currency is iso4217; value >= 0,at_least_once,,Checkout start
tiktok,Purchase,value;currency,currency is iso4217; value > 0,once,,Order confirmation. Use a stable event_id for repeated copies of the same purchase
```

## Lead form: GA4 and Meta Pixel

```csv
platform,event,required,rules,count,consent,note
ga4,generate_lead,,currency is iso4217; value >= 0,once,,Form sent
meta,Lead,,,once,,Form sent. Add event_id if you also use the Conversions API
```

## Semicolon version

For Excel in Spanish or other European locales, use `;` as the separator. Then lists inside a cell can't use `;`
unless the cell is quoted:

```csv
platform;event;required;rules;count;consent;note
ga4;purchase;"transaction_id;currency;value;items";"currency is iso4217; value > 0";once;;Confirmación del pedido
```
