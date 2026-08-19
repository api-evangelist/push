---
name: Onboard a guest as a Cendyn CRM contact
description: Create or update a guest contact in Cendyn CRM (PUSHTech) with GDPR consent recorded, subscribe them to an audience list, and confirm the write — the entry point for every other flow on this API.
api: openapi/push-cendyn-crm-openapi.yml
operations: [createContact, listContact, showContact, updateContact, listAudienceList]
generated: '2026-08-13'
method: generated
source: openapi/push-cendyn-crm-openapi.yml
---

# Onboard a guest as a Cendyn CRM contact

Contact is the root entity of this platform. Deliveries, purchases, coupons and campaign
previews all hang off a contact id, so this is the first flow to get right.

## Before you start

- **Base URL.** Two data centers, and they are not interchangeable:
  `https://api.eu.cendyncrm.com` (EU) or `https://api.us.cendyncrm.com` (US). An account lives in
  exactly one. Using the wrong one returns `401` with
  `{"error":"authorization failure for account: ..."}` — the same error as a bad token, so if
  auth "fails" with a token you believe is correct, check the region first.
- **Auth.** `Authorization: Token token={account_secret}`. Not `Bearer`. There is no
  `WWW-Authenticate` challenge on failure, so you cannot discover the scheme from a 401.
- **account_id.** Every path is `/v2/account/{account_id}/…`. The account id is not derivable
  from the token — it must be supplied to you.
- **Content type.** Send and accept `application/json`.

## Steps

### 1. Check whether the guest already exists

Call `listContact` (`GET /v2/account/{account_id}/contact`) filtered by your own backend
identifier using the `user_id` query parameter. `user_id` is the documented de-duplication key:
it is the caller's identifier for the person, and reusing it is how you avoid creating a second
contact for the same guest.

If you hold a device identifier instead, filter on `device_id`.

### 2. Create or update

**If no contact came back**, call `createContact`
(`POST /v2/account/{account_id}/contact`). The API requires **either** `email` **or** the pair
`phone_countrycode` + `phone_number` — a contact with neither is rejected.

Set at minimum:

- `user_id` — your backend identifier, so step 1 works next time.
- `email`, or `phone_countrycode` + `phone_number`.
- `name_first`, `name_last`.
- `language` as an ISO-2 code (e.g. `ES`), `country` likewise.
- `born_date` and any other date in ISO 8601 (`1990-01-01T00:00:00Z`).
- `gender` is constrained to `MALE` or `FEMALE`.

Record consent in the same call rather than a follow-up — the object carries it natively:

- `gdpr_marketing_consent` (boolean)
- `gdpr_accept_terms` (boolean)
- `gdpr_date` (ISO 8601, when consent was given)
- `gdpr_remote_ip` (the IP the guest consented from)

A successful create returns `201` with the contact body including its `id` (a 24-character hex
string, e.g. `550aac3da096734ce5000001`). **Keep that id** — it is the handle for every
downstream flow.

**If a contact came back**, call `updateContact`
(`PUT /v2/account/{account_id}/contact/{contact_id}`) instead. Returns `200`.

### 3. Subscribe them to an audience list

Audience lists are the segmentation unit. Call `listAudienceList`
(`GET /v2/account/{account_id}/lists`) to find the list you want, then set subscriptions on the
contact via the `list_subscriptions` member — `list_subscriptions[list_id]` and
`list_subscriptions[subscribed]` — on the create or update call. There is no separate
subscribe endpoint.

### 4. Verify

Call `showContact` (`GET /v2/account/{account_id}/contact/{contact_id}`) and confirm the fields
you set came back. This matters more here than on most APIs, because of the retry rule below.

## Rules an agent must follow on this API

- **There is no idempotency.** No `Idempotency-Key` header, no idempotent-retry contract, on any
  of the 67 operations. If `createContact` times out, **do not blind-retry it** — re-run step 1
  and check whether the contact landed. Retrying blind creates a duplicate guest.
- **Custom fields are open.** Any custom field defined on the account is sent as a plain
  top-level member of the contact JSON, using the field's *name*, not its label. The documented
  property list is therefore not exhaustive for a given account. Call
  `listContactCustomFields` (`GET /v2/account/{account_id}/custom_fields`) to discover them.
- **List responses are unbounded.** No page, cursor, offset or limit parameter exists on any list
  operation, and no pagination contract is published. You cannot tell whether a `listContact`
  response is complete. Filter narrowly rather than listing broadly.
- **Errors are a bare string.** Failures return `{"error": "<message>"}` — no code, no field
  pointer, no RFC 9457 problem document. Log the `x-request-id` response header (it is returned
  on every response, though undocumented) so a support conversation has something to reference.
- **Email validation costs money.** `emailValidationContact` draws down the account balance. The
  `email_validation` block on create exposes `create_if_invalid` and `create_if_no_credit` to
  control what happens when validation fails or credit runs out. Decide those explicitly.
