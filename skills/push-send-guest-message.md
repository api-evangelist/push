---
name: Send a guest message and track its delivery
description: Send an email, SMS or push notification to a Cendyn CRM (PUSHTech) contact, check the account has credit first, and follow the message to a terminal delivery status — with the duplicate-send hazard called out.
api: openapi/push-cendyn-crm-openapi.yml
operations: [currentBalanceAccount, sendEmailDeliveries, sendSMSDeliveries, sendPushDeliveries, showDeliveries, listDeliveries]
generated: '2026-08-13'
method: generated
source:
  - openapi/push-cendyn-crm-openapi.yml
  - asyncapi/push-webhooks.yml
---

# Send a guest message and track its delivery

These are the three operations on this API that reach a real human being and spend real money.
Treat them accordingly.

## STOP — read this before the first send

**There is no idempotency contract on this API.** No `Idempotency-Key` header, no request
deduplication, nothing. If `sendEmailDeliveries` times out or returns a network error, the
message may well have been sent. **Retrying sends a second message to the guest and charges the
account twice.**

The correct recovery is to call `listDeliveries` filtered by `contact_id` and a time window and
look for the delivery you just attempted — not to retry the send.

For the same reason, a delivery send should be gated behind human confirmation in any autonomous
agent loop.

## 1. Check the balance

Call `currentBalanceAccount` (`GET /v2/account/{account_id}/balance/current`). It returns:

```json
{"balance": "582.84660", "currency": "eur5"}
```

Every email, SMS and push draws this down. There is no published rate card and no documented
behaviour when the balance reaches zero, so check before a batch rather than discovering the
floor mid-run.

## 2. Send

Pick the channel operation. All three are `POST` and all three return `200` (not `201`):

| Channel | Operation | Path |
|---|---|---|
| Email | `sendEmailDeliveries` | `/v2/account/{account_id}/email/deliveries` |
| SMS | `sendSMSDeliveries` | `/v2/account/{account_id}/sms/deliveries` |
| Push | `sendPushDeliveries` | `/v2/account/{account_id}/push/deliveries` |

Notes per channel, from the reference:

- **Push** takes an `app_id`. Apps are registered in the Cendyn CRM manager UI — **there is no
  app resource in the REST API**, so an agent cannot discover which app to send to. The app id
  must be supplied to you.
- **SMS** publishes a `sender` and a `message_holder` supporting `%{name}`-style substitution,
  plus a `start_now` control.
- Read the exact body members for your channel from
  `openapi/push-cendyn-crm-openapi.yml` — each send operation documents 8 to 16 of them.

## 3. Follow it to a terminal status

Take the delivery id from the send response and call `showDeliveries`
(`GET /v2/account/{account_id}/delivery/{delivery_id}`), or poll `listDeliveries`
(`GET /v2/account/{account_id}/deliveries`) filtered by `contact_id`, `campaign_id`, `app_id`,
`device_id` or a `from_id`/`until_id` range.

The published status vocabulary is:

`none`, `queued`, `sent`, `deliverd`, `opened`, `clicked`, `rejected`, `undefined`,
`undelivered`, `forbidden_country_code`, `failed`, `bounced`, `unsubscribed`, `dropped`,
`complained`

**Match on `deliverd`, not `delivered`.** That misspelling is in the provider's own published
status list and is reproduced verbatim in `asyncapi/push-webhooks-asyncapi.yml`. An integration
matching the correctly-spelled word will silently never see a successful delivery. Note also that
`undefined` is a real value in the enum, not a placeholder in the docs.

A `refunded: true` on the delivery means the account balance was credited back for a message that
did not land.

## 4. Prefer webhooks over polling

Polling `showDeliveries` works but is wasteful. The platform pushes delivery status changes to a
callback URL. See `asyncapi/push-webhooks.yml` for the full contract; the essentials:

- Register the callback in the Webhooks section of the API area of the account manager. **There
  is no subscription API** — this step cannot be automated.
- Verify every callback: HMAC-SHA256 over `concat(timestamp, token)` keyed with the account
  secret, hex-encoded, compared to the `Authorization` header. Note the signature covers only
  those two body fields, **not the event body**.
- Return `200` to acknowledge, or `406` to reject permanently. Anything else is retried for
  4 hours (5m, 5m, 10m, 10m, 30m, 1h, 2h) — **except delivery events, which are never retried at
  all.** If your endpoint is down when a delivery event fires, that event is gone.
