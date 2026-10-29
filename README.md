# Dispatch

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

Dispatch (Dispatch Technologies, Boston MA) is a service-orchestration platform for enterprises that fulfil work through networks of independent contractors and third-party service providers - manufacturer warranty, home and property claims, retail, franchise and proptech. Dispatch gives the brand visibility into work performed by providers who keep using their own field-service tools, and gives the homeowner a tracked, branded service experience. Its public REST API v3 exposes the core business objects - jobs, customers, organizations, appointments, users, sources, brands, survey responses and composite work orders - to job sources such as warranty companies and equipment manufacturers, and to service enterprises running first-party branch networks. The API is secured with OAuth 2.0 bearer tokens, offers a full sandbox environment, and is documented in a public GitHub repository.

**Areas:** Field Service · Service Orchestration · Work Orders · Scheduling · Contractor Networks · Home Services · Warranty · Customer Experience · Logistics · Company

## Dispatch on APIs.io

Everything profiled for Dispatch is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/dispatch/](https://apis.io/providers/dispatch/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [Dispatch Appointments API](https://apis.io/apis/dispatch/dispatch-appointments-api/) — Scheduled times at which an assigned technician performs work for a job.
- [Dispatch Authentication API](https://apis.io/apis/dispatch/dispatch-authentication-api/) — OAuth 2.0 token issuance and refresh.
- [Dispatch Brands API](https://apis.io/apis/dispatch/dispatch-brands-api/) — Brands or divisions controlling logo and copy. Accessible only to job sources.
- [Dispatch Customers API](https://apis.io/apis/dispatch/dispatch-customers-api/) — The homeowner or end customer a job belongs to.
- [Dispatch Files API](https://apis.io/apis/dispatch/dispatch-files-api/) — Files and photos associated with a job.
- [Dispatch Jobs API](https://apis.io/apis/dispatch/dispatch-jobs-api/) — A single body of work for a customer, assigned to an organization.
- [Dispatch Organizations API](https://apis.io/apis/dispatch/dispatch-organizations-api/) — The branch or third-party service provider responsible for doing the work.
- [Dispatch Sources API](https://apis.io/apis/dispatch/dispatch-sources-api/) — Where job information originated.
- [Dispatch Survey Responses API](https://apis.io/apis/dispatch/dispatch-survey-responses-api/) — Customer surveys sent when an appointment or job completes.
- [Dispatch Users API](https://apis.io/apis/dispatch/dispatch-users-api/) — Dispatchers and technicians using the Dispatch applications.
- [Dispatch Work Orders API](https://apis.io/apis/dispatch/dispatch-work-orders-api/) — Composite object creating jobs, customers, organizations and appointments in one call.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Dispatch Webhooks](https://apis.io/asyncapis/dispatch/dispatch-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [dispatch](https://apis.io/servers/dispatch/dispatch/)

#### Collections (11)

Postman and Open Collection request collections.

- [Dispatch Files API v1 Appointments API](https://apis.io/collections/dispatch/open-dispatch-appointments-api/)
- [Dispatch Files API v1 Appointments Authentication API](https://apis.io/collections/dispatch/open-dispatch-authentication-api/)
- [Dispatch Files API v1 Appointments Brands API](https://apis.io/collections/dispatch/open-dispatch-brands-api/)
- [Dispatch Files API v1 Appointments Customers API](https://apis.io/collections/dispatch/open-dispatch-customers-api/)
- [Dispatch API v1 Appointments Files API](https://apis.io/collections/dispatch/open-dispatch-files-api/)
- [Dispatch Files API v1 Appointments Jobs API](https://apis.io/collections/dispatch/open-dispatch-jobs-api/)
- [Dispatch Files API v1 Appointments Organizations API](https://apis.io/collections/dispatch/open-dispatch-organizations-api/)
- [Dispatch Files API v1 Appointments Sources API](https://apis.io/collections/dispatch/open-dispatch-sources-api/)
- [Dispatch Files API v1 Appointments Survey Responses API](https://apis.io/collections/dispatch/open-dispatch-survey-responses-api/)
- [Dispatch Files API v1 Appointments Users API](https://apis.io/collections/dispatch/open-dispatch-users-api/)
- [Dispatch Files API v1 Appointments Work Orders API](https://apis.io/collections/dispatch/open-dispatch-work-orders-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Dispatch Files V1 Overlay](https://apis.io/overlays/dispatch/dispatch-files-v1-overlay/)
- [Dispatch Rest V3 Overlay](https://apis.io/overlays/dispatch/dispatch-rest-v3-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Dispatch Authentication](https://apis.io/security/dispatch/dispatch-authentication/)
- [Dispatch Domain Security](https://apis.io/security/dispatch/dispatch-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://dispatch.me)
- [Documentation](https://github.com/DispatchMe/v3-api-docs)
- [Getting started](https://github.com/DispatchMe/v3-api-docs/blob/master/source/index.html.md#getting-started)
- [API reference](https://github.com/DispatchMe/v3-api-docs/blob/master/source/index.html.md)
- [Support](https://dispatch.me/contact)
- [Blog](https://dispatch.me/resources/blog/)
- [Terms of service](https://dispatch.me/terms-of-service)
- [Privacy policy](https://dispatch.me/privacy-policy)
- [GitHub](https://github.com/DispatchMe)
- [Status](https://status.dispatch.me)

## API Evangelist rating

**Kin Score 38.2** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 23.5 |
| Contract governance | 4.5 |
| Operational transparency | 26.3 |
| Developer ergonomics | 63.7 |
| Access clarity | 27.6 |

**Agent readiness 25.3** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
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

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/dispatch/](https://apis.io/providers/dispatch/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Dispatch: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/dispatch/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
