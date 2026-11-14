# Codes returned by `tracklint_decode_request`

Each warning has a `code`, the event it belongs to (`hitIndex`, from 0) and, when it comes from one parameter, that
parameter's position (`paramIndex`). The tool never returns the value that triggered it.

## Warnings

| Code | What it means | What to check |
|---|---|---|
| `PURCHASE_WITHOUT_TRANSACTION_ID` | A GA4 `purchase` without `transaction_id` | Send the order ID: without it GA4 can't recognise repeated sends of the same order and reports can't be matched to orders |
| `PURCHASE_WITHOUT_EVENT_ID` | A Meta `Purchase`, or a TikTok `CompletePayment`, `PlaceAnOrder` or `Purchase`, without an event ID | If the same purchase is also sent from the server (Conversions API, Events API), both copies need the same event ID (and event name) to be deduplicated |
| `VALUE_WITHOUT_CURRENCY` | `value` is present and `currency` isn't | Send the currency with every value |
| `CURRENCY_NOT_ISO4217` | `currency` isn't a valid ISO 4217 code | Use codes such as `USD`, `EUR`, `MXN` |
| `VALUE_NOT_NUMBER` | A recognized numeric parameter is not represented as a finite number with absolute value below 1e12. Empty strings do not trigger this warning | Check numeric encoding and formatting, for example `"1,299.00"` |
| `USER_DATA_NOT_HASHED` | user_data contains an em, ph, email, phone_number or phone key whose checked value is not a 64-character hexadecimal string. This can occur in Meta or TikTok requests. This checks hash format, not correct normalization or hashing | Advanced matching data should be normalised and hashed; check how it is set up |
| `EMAIL_IN_VALUE` | A value contains something that looks like an email address, also URL-encoded | Personal data sent to the platform, often in a URL, a coupon or a custom parameter. Platform policies, such as Google Analytics', restrict sending it |
| `PHONE_IN_VALUE` | A parameter named like a phone (`phone`, `tel`, `mobile`, `whatsapp`…) carries seven or more digits | As above |
| `PHONE_IN_LINK` | A `tel:`, `sms:` or `callto:` link, or a WhatsApp link with a number, appears in a value | Usually a click event that records the link: the number is sent to the platform |
| `WHATSAPP_LINK` | A WhatsApp link appears in a value | It may also carry the prefilled message |
| `SECRET_LIKE_VALUE` | A value looks like a token, password, API key or `Bearer` credential | Never send credentials in tracking; find where the value comes from |
| `USER_IDENTIFIERS_PRESENT` | The event carries `client_id`, `session_id` or `user_id` | Normal for GA4. Mentioned so the user doesn't share raw requests publicly |
| `CUSTOM_EVENT_NAME` | The event name isn't a standard one for its platform | Fine for custom events. Check for typos, such as `Purchase` in GA4, where the standard name is `purchase` |
| `CUSTOM_PARAMETER_NAMES` | Some parameter names aren't in Tracklint's list of known ones | Usual with custom parameters. Check for typos in standard ones (`transactionid`, `curency`) |

Warnings are targeted checks, not a complete audit. The missing-ID warnings check whether the decoded parameter
exists; they do not prove that a present ID is non-empty, usable or correctly deduplicated. No warnings does not mean
that the request is correct or contains no personal data.

## Status of a successful call (`ok: true`)

| `status` | Meaning |
|---|---|
| `DECODED` | At least one event was decoded |
| `NO_EVENTS_DECODED` | The endpoint is supported, but the request carried no event the tool could read |
| `NOT_A_SUPPORTED_REQUEST` | Not a GA4, Meta Pixel or TikTok Pixel request (`compatible: false`). Google Ads requests are in this group |

## Errors (`ok: false`, code in `error.code`)

| Code | Meaning | What to do |
|---|---|---|
| `INVALID_URL` | The URL can't be parsed | Copy the full URL, starting with `https://` |
| `MISSING_ARGUMENT`, `INVALID_ARGUMENTS`, `INVALID_ARGUMENT_TYPE`, `INVALID_ARGUMENT_VALUE`, `UNKNOWN_ARGUMENT` | The call was malformed, for example a `method` other than `GET` or `POST` | Call again with `url`, optional `body` and `method` |
| `REQUEST_TOO_LARGE` | The URL is over 16 KiB, the body over 256 KiB, or the request is larger than the decoder accepts | Decode a smaller request |
| `TOO_MANY_EVENTS` | More than 50 events in one request | Split the GA4 batch and decode the parts |
| `TOO_MANY_PARAMETERS` | More than 100 parameters in an event, or more than 50 `user_data` keys | Decode a smaller request |
| `INCOMPLETE_REQUEST` | At least one GA4 batch event was decoded, but fewer events were decoded than the number of non-empty body lines. The whole call is rejected. If no event can be decoded, the successful-call status is NO_EVENTS_DECODED | Check that the body was copied whole |
| `RESPONSE_TOO_LARGE` | The result would exceed 128 KiB | Decode fewer events at a time |
| `PROCESSING_TIMEOUT`, `PROCESSING_LIMIT_EXCEEDED` | The request took more than 5 seconds or hit a processing limit | Decode a smaller request |
| `WORKER_FAILED`, `INTERNAL_ERROR` | The server failed | Try again; if it persists, report it with the steps (not the request) |
