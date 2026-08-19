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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Push (PUSHTech) is a hospitality CRM and customer data platform for hotels, now operating as
**Cendyn CRM** following its acquisition by Cendyn. It unifies the guest database and orchestrates
pre-stay, during-stay and post-stay communication across email, SMS and web/app push.

Backed by: 500-global — https://pushtech.com

## The API surface

- **REST API** — 67 operations across 15 resources, token-authenticated, served from two
  independent data centers (`api.eu.cendyncrm.com`, `api.us.cendyncrm.com`). An account lives in
  exactly one; the base URLs are not interchangeable.
- **Webhooks** — five event groups (activities, deliveries, contacts, bulk contacts, incoming
  SMS), HMAC-SHA256 signed.
- **Web SDK** — a first-party JavaScript library for browser tracking and web push, distributed
  by `<script>` tag from a vendor CDN.

Reference: <https://developers.cendyncrm.com/api/reference>

## What is NOT published by Cendyn CRM

Recorded so absence reads as deliberate rather than as something this profile failed to find.
Every item below was probed on 2026-08-13 and missed:

- **No OpenAPI.** The reference is server-rendered HTML (Apitome / rspec_api_documentation).
  `openapi/push-cendyn-crm-openapi.yml` in this repo is **derived by API Evangelist** from that
  reference — every path, method, parameter, description and example is copied from it, and the
  document carries an `info.x-provenance` block saying so. It is not a Cendyn artifact.
- **No AsyncAPI.** `asyncapi/push-webhooks-asyncapi.yml` is likewise derived from the published
  webhook reference and carries the same provenance stamp.
- **No MCP server** (`/mcp` 404s on every host), **no A2A agent card**, and **no `/.well-known/`
  document of any kind**. No `a2a/` artifact is written and no `MCPServer`, `AgentCard`,
  `WellKnown` or `SecurityTxt` pointer is emitted — see `x-pointers-deliberately-absent` in
  `apis.yml`.
- **No idempotency contract**, **no pagination**, and **no published rate limits or rate-limit
  response headers** on any of the 67 operations.
- **No status page, no changelog, no SLA, no deprecation policy, no public pricing.**
- **No server-side SDK** in any language. The only client library is the browser Web SDK, whose
  newest build is 2.9.0, last modified 2024-06-26.

## A lifecycle note

The developer portal moved from `developers.pushtech.com` to `developers.cendyncrm.com` **without
a redirect** — the old hostnames simply stopped resolving, so every inbound link and integration
doc pointing at them is dead. The only breadcrumb to the new location is the API host's own 404
body. The portal's own sign-in link, pointing at `www.cendyncrm.com`, is itself broken.
