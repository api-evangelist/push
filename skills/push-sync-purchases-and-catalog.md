---
name: Sync products and guest purchases into Cendyn CRM
description: Load a product catalog into Cendyn CRM (PUSHTech) and attach purchase history to guest contacts, either operation-by-operation or through the bulk Sync Data ingest — the flow that turns the CRM into a CDP.
api: openapi/push-cendyn-crm-openapi.yml
operations: [createProduct, listProduct, showProduct, updateProduct, createPurchase, listPurchase, updatePurchase, deleteGroupPurchase, createSyncData, showSyncData, createBulkContact]
generated: '2026-08-13'
method: generated
source: openapi/push-cendyn-crm-openapi.yml
---

# Sync products and guest purchases into Cendyn CRM

Purchases are what make this a customer data platform rather than a contact list — they are what
campaign segmentation and the `product_view` / `add_cart` / `product_refund` activity events hang
off. There are two ways in, and picking the wrong one is the main mistake here.

## Choose your path first

| | Per-resource | Sync Data |
|---|---|---|
| Operations | `createProduct`, `createPurchase`, … | `createSyncData` |
| Path prefix | `/v2/…` | `/account/{account_id}/sync_data` — **no version prefix** |
| Shape | One object per call | `contacts[]` + `purchase[]` in one payload |
| Result | Synchronous `201` | `201`, then poll `showSyncData` |
| Use when | Real-time, low volume, you need per-object error detail | Backfill, migration, high volume |

**Sync Data is the only resource on this API published without a `/v2` prefix.** That is not a
typo in this document — it is how the reference publishes it, and it means the resource sits
outside whatever the v2 versioning line means. Treat it as a legacy-shaped bulk endpoint.

## Path A — per resource

### 1. Load the catalog

`createProduct` (`POST /v2/account/{account_id}/products`) returns `201`. Products are keyed by
a **UUID**, not the 24-character hex id used by contacts — `showProduct`, `updateProduct` and
`deleteProduct` all address `/products/{UUID}`.

Discover account-specific product attributes first with `listProductCustomFields`
(`GET /v2/account/{account_id}/product_custom_fields`); custom fields are sent as plain
top-level members of the product JSON under their field *name*.

### 2. Attach purchases to a contact

`createPurchase` (`POST /v2/account/{account_id}/contact/{contact_id}/purchases`) — note the
purchase is created **under the contact**, and returns `201`.

The purchase body links out to the rest of the graph:

- `products[UUID]` — the products bought, by UUID from step 1.
- `campaign_id` — the campaign the purchase is attributed to, if any.

You need the contact id first; see the `push-onboard-guest-contact` skill.

### 3. Read and correct

Purchases are addressed at **account** level once created, not under the contact:

- `listPurchase` — `GET /v2/account/{account_id}/purchases`, filterable by `contact_id`,
  `product_UUID` and `user_id`.
- `updatePurchase` — `PUT /v2/account/{account_id}/purchases/{purchase_id}`.
- `deletePurchase` — `DELETE /v2/account/{account_id}/purchases/{purchase_id}`.
- `deleteGroupPurchase` — `DELETE /v2/account/{account_id}/purchases/delete_group`, a bulk
  delete by filter. **This one deletes many records from a filter expression with no dry-run and
  no confirmation step.** Run the equivalent `listPurchase` filter first and count the rows
  before you call it.

## Path B — bulk Sync Data

`createSyncData` (`POST /account/{account_id}/sync_data`) accepts `contacts[]` and `purchase[]`
in one payload — roughly 50 documented members covering contact identity, consent, social ids and
the purchase with its products. It returns `201` immediately; the work is asynchronous.

Poll `showSyncData` (`GET /account/{account_id}/sync_data/{id}`) for the outcome.

For contacts alone, `createBulkContact`
(`POST /v2/account/{account_id}/contact/bulk`) is the narrower equivalent — retrieve its result
with `getBulkContact` (`GET /v2/account/{account_id}/contact/bulk/{bulk_id}`), or receive it as a
`bulk_contacts` webhook carrying `contacts_valid_count`, `contacts_failed_count`,
`failed_contacts[]` and `valid_contacts[]`.

## Rules an agent must follow

- **No idempotency, again.** A retried `createPurchase` creates a second purchase and inflates
  the guest's spend history. On a timeout, call `listPurchase` filtered by `contact_id` and check
  before retrying.
- **No pagination.** `listPurchase` and `listProduct` publish filters but no page, cursor or
  limit parameter. On a large account you cannot tell whether you have read everything. Filter by
  `contact_id` or a narrow window rather than listing the whole account.
- **Bulk failures are per-row and awkwardly shaped.** The `bulk_contacts` webhook field
  dictionary names `failed_contacts[error]` in the singular, but the provider's own published
  example emits `"errors"` as an array. Handle both.
- **Products referenced in purchases must exist first.** Load the catalog before the purchase
  history, or the `products[UUID]` references have nothing to point at.
