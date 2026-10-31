# Push

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Push (PUSHTech, now operating as Cendyn CRM after its acquisition by Cendyn) is a CRM and customer data platform specialised in hospitality — the guest journey for hotels — with additional use in retail, ecommerce and marketplace. The platform unifies the guest database and orchestrates pre-stay, during-stay and post-stay communication across email, SMS and web/app push. Its token-authenticated REST API exposes 67 operations across 15 resources (contacts and contact custom fields, companies, products, purchases, coupon lists, hotel data, audience lists, campaigns, deliveries, account balance and a bulk sync-data ingest) from two independent data centers, api.eu.cendyncrm.com and api.us.cendyncrm.com. An HMAC-signed webhook surface publishes five event groups — activities, deliveries, contacts, bulk contacts and incoming SMS — and a first-party JavaScript Web SDK handles browser tracking and web push. The developer portal moved from developers.pushtech.com to developers.cendyncrm.com without a redirect; the old hostnames no longer resolve. Cendyn CRM publishes no OpenAPI, no AsyncAPI, no MCP server, no agent card and no /.well-known/ documents; the machine-readable artifacts in this repo are derived by API Evangelist from the provider's own published HTML reference.

**Areas:** Company · CRM · Customer Data Platform · Marketing Automation · Hospitality · Hotels · Guest Experience · Email · SMS · Push Notifications · Webhook · Segmentation

## Push on APIs.io

Everything profiled for Push is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/push/](https://apis.io/providers/push/)** — or go straight to one of the APIs or properties below.

### APIs (15)

- [Push Account API](https://apis.io/apis/push/push-account-api/) — The Account API from Push — 1 operation(s) for account.
- [Push Audience List API](https://apis.io/apis/push/push-audience-list-api/) — The Audience List API from Push — 2 operation(s) for audience list.
- [Push Campaigns API](https://apis.io/apis/push/push-campaigns-api/) — The Campaigns API from Push — 3 operation(s) for campaigns.
- [Push Company API](https://apis.io/apis/push/push-company-api/) — The Company API from Push — 2 operation(s) for company.
- [Push Company custom fields API](https://apis.io/apis/push/push-company-custom-fields-api/) — The Company custom fields API from Push — 2 operation(s) for company custom fields.
- [Push Contact API](https://apis.io/apis/push/push-contact-api/) — The Contact API from Push — 5 operation(s) for contact.
- [Push Contact Custom fields API](https://apis.io/apis/push/push-contact-custom-fields-api/) — The Contact Custom fields API from Push — 2 operation(s) for contact custom fields.
- [Push Coupon lists API](https://apis.io/apis/push/push-coupon-lists-api/) — The Coupon lists API from Push — 3 operation(s) for coupon lists.
- [Push Deliveries API](https://apis.io/apis/push/push-deliveries-api/) — The Deliveries API from Push — 5 operation(s) for deliveries.
- [Push Hotel Data API](https://apis.io/apis/push/push-hotel-data-api/) — The Hotel Data API from Push — 4 operation(s) for hotel data.
- [Push Hotel Data custom fields API](https://apis.io/apis/push/push-hotel-data-custom-fields-api/) — The Hotel Data custom fields API from Push — 2 operation(s) for hotel data custom fields.
- [Push Product API](https://apis.io/apis/push/push-product-api/) — The Product API from Push — 2 operation(s) for product.
- [Push Product custom fields API](https://apis.io/apis/push/push-product-custom-fields-api/) — The Product custom fields API from Push — 2 operation(s) for product custom fields.
- [Push Purchase API](https://apis.io/apis/push/push-purchase-api/) — The Purchase API from Push — 4 operation(s) for purchase.
- [Push Sync Data API](https://apis.io/apis/push/push-sync-data-api/) — The Sync Data API from Push — 2 operation(s) for sync data.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (2)

Multi-step workflows that chain operations into a business task.

- [Load a product catalog and attach guest purchase history](https://apis.io/arazzos/push/push-load-catalog-and-purchase-history/)
- [Onboard a guest and send a welcome message](https://apis.io/arazzos/push/push-onboard-and-welcome-guest/)

#### AsyncAPI (2)

Event-driven and webhook contracts.

- [Push Webhooks](https://apis.io/asyncapis/push/push-webhooks/)
- [Cendyn CRM (PUSHTech) Webhooks](https://apis.io/asyncapis/push/push-webhooks-asyncapi/)

#### Event channels (5)

Individual events and webhooks the provider emits.

- [/](https://apis.io/channels/push/activities/)
- [/](https://apis.io/channels/push/bulkcontacts/)
- [/](https://apis.io/channels/push/contacts/)
- [/](https://apis.io/channels/push/deliveries/)
- [/](https://apis.io/channels/push/incomingsms/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Push Agentic Access](https://apis.io/agentic-access/push/push-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Push Cendyn Crm Overlay](https://apis.io/overlays/push/push-cendyn-crm-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Push Authentication](https://apis.io/security/push/push-authentication/)
- [Push Domain Security](https://apis.io/security/push/push-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Push Plans Pricing](https://apis.io/plans/push/push-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Push Rate Limits](https://apis.io/rate-limits/push/push-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://pushtech.com)
- [Documentation](https://developers.cendyncrm.com/api)
- [Getting started](https://developers.cendyncrm.com/api/authentication)
- [API reference](https://developers.cendyncrm.com/api/reference)
- [Support](https://pushtech.com/contact)
- [Blog](https://www.cendyn.com/resources/)
- [Terms of service](https://pushtech.com/terms)
- [Privacy policy](https://pushtech.com/privacy_policy)
- [GitHub](https://github.com/pushtech)

## API Evangelist rating

**Kin Score 32.1** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 21.8 |
| Contract governance | 4.5 |
| Operational transparency | 10.5 |
| Developer ergonomics | 66.1 |
| Access clarity | 27.6 |

**Agent readiness 30.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 20.8** — Telecommunications

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/push/](https://apis.io/providers/push/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Push: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/push/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
