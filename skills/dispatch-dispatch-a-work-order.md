---
name: Dispatch a work order
description: >-
  Send a job into the Dispatch network as a job source - authenticate, create a work order
  that carries the customer, the location, the contacts and the candidate appointment
  windows, choose an orchestration algorithm, and confirm the resulting job.
api: openapi/dispatch-rest-v3-openapi.yml
operations:
  - createToken
  - createWorkOrder
  - listJobs
  - getJob
  - updateWorkOrder
  - cancelWorkOrder
generated: '2026-07-20'
method: generated
source: https://github.com/DispatchMe/v3-api-docs
---

# Dispatch a work order

This is the recommended entry point for a job source — a warranty company, an equipment
manufacturer, or a lead-generation platform. A work order carries everything needed to
create the job, its customer, its organization and its appointment windows in a single
call, so prefer it over assembling the objects individually.

## Before you start

- Work in sandbox first: `https://api-sandbox.dispatch.me`. Production is
  `https://api.dispatch.me`. There is no key prefix or mode flag — the environment is the
  hostname.
- Credentials are not self-service. A Dispatch account manager issues the public and
  secret key pair, per environment.
- Locations are supported in the United States and Canada only.

## 1. Get a bearer token

Call `createToken` — `POST /v3/oauth/token` — with the `client_credentials` grant:

```json
{ "grant_type": "client_credentials", "client_id": "<public key>", "client_secret": "<secret key>" }
```

The response carries `access_token`, `expires_in` and `refresh_token`. Send the token as
`Authorization: Bearer <access_token>`.

Tokens are temporary. Treat any `401` as "token expired" — call `createToken` again with
`{"grant_type": "refresh_token", "refresh_token": "..."}` and retry the original request
once. Do not loop.

## 2. Create the work order

Call `createWorkOrder` — `POST /v3/work_orders`. Required at minimum: `title` and
`location`. Populate as much as you have:

- `title`, `description` (markdown is supported), `service_type`
- `location` — `street_1`, `city`, `state` (two-character abbreviation), `postal_code`,
  and `timezone` in IANA form. If you omit `timezone` Dispatch derives it from the postal
  code.
- `external_id` — **always set this to your own record ID.** Dispatch has no idempotency
  key, so this is the only correlation handle you get if a request times out and you have
  to reconcile.
- `contacts[]` — the people to notify. Each contact takes `phone_numbers[]` and
  `email_addresses[]` as `{label, value, preferred}` triples. Mark exactly one of each
  `preferred: true`.
- `organizations[]` — who does the work. Give each an `external_id` too; Dispatch
  deduplicates organizations on creation and will match on it.
- `appointment_windows[]` — optional `{start_time, end_time}` pairs for the organization
  to choose from, in ISO 8601.
- `orchestration` — `direct_offer` offers the job to a single organization and the job
  starts in "offered" status; `direct_assign` assigns it and the job starts in
  "unscheduled". Both `direct_*` algorithms permit exactly one organization.

## 3. Confirm the job

The work order creates a job. Find it with `listJobs` — `GET /v3/jobs` — filtering on the
identifier you supplied:

```
GET /v3/jobs?filter[external_ids_contains]=<your id>
```

Then read it with `getJob` — `GET /v3/jobs/{id}`. Job status is one of `unscheduled`,
`scheduled`, `paused`, `complete`, `canceled`.

## 4. Amend or withdraw

- `updateWorkOrder` — `PATCH /v3/work_orders/{id}` to amend.
- `cancelWorkOrder` — `POST /v3/work_orders/{id}/cancel` to withdraw. This cancels the job
  **and** every child appointment. It is a real-world action: a technician visit already
  promised to a homeowner disappears. Confirm with a human before calling it.

## Rules that will bite you

- **No idempotency.** A retried `POST /v3/work_orders` may create a second work order.
  Before retrying a write that failed without a clear response, search by
  `filter[external_ids_contains]` to see whether the first attempt landed.
- **422 means validation.** Read the response body — it carries the per-field detail. Do
  not retry a 422 unchanged.
- **403 is an ACL result, not a bug.** Your access to organizations, their customers and
  their staff depends on your network relationship with them. Escalate to the account
  manager rather than working around it.
- **429 has no headers.** Dispatch documents the status but publishes no limit, remaining
  or reset headers. Use exponential backoff.
- Capture `X-Request-Id` from every response. It is the only handle Dispatch support has
  when you report a failure.

## Related

- `conventions/dispatch-conventions.yml` — paging, filtering, tracing, envelope
- `errors/dispatch-problem-types.yml` — the full status catalog
- `authentication/dispatch-authentication.yml` — both credential models
- `sandbox/dispatch-sandbox.yml` — environment map
