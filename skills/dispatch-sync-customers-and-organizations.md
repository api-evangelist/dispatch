---
name: Sync customers and organizations with external IDs
description: >-
  Keep your own system of record aligned with Dispatch without creating duplicates - use
  the external_ids correlation field to look objects up by your identifiers, rely on
  organization deduplication, and reconcile writes that have no idempotency guarantee.
api: openapi/dispatch-rest-v3-openapi.yml
operations:
  - createToken
  - listCustomers
  - createCustomer
  - updateCustomer
  - deleteCustomer
  - listOrganizations
  - getOrganization
  - createOrganization
  - updateOrganization
  - listJobs
  - listJobContacts
  - updateJobContacts
  - updateJobContact
generated: '2026-07-20'
method: generated
source: https://github.com/DispatchMe/v3-api-docs
---

# Sync customers and organizations with external IDs

Dispatch has no idempotency key. What it has instead is `external_ids` — an array of your
own record identifiers carried on jobs, customers and organizations. Used properly it is
both your lookup index and your duplicate defence. This skill is about using it properly.

**`external_ids` is available to job sources only.** If you authenticate as a single
service provider, this skill does not apply to you.

## 1. Authenticate

`createToken` — `POST /v3/oauth/token` with the `client_credentials` grant. Refresh on
`401`. See `authentication/dispatch-authentication.yml`.

## 2. Always look up before you create

This is the whole discipline. Before creating anything, search by your own ID.

Customers — `listCustomers`, `GET /v3/customers`:

```
GET /v3/customers?filter[external_id_contains]=<your customer id>
GET /v3/customers?filter[email_eq]=<email>
GET /v3/customers?filter[organization_id_eq]=<org>
```

Organizations — `listOrganizations`, `GET /v3/organizations`:

```
GET /v3/organizations?filter[external_ids_contains]=<your org id>
```

Jobs — `listJobs`, `GET /v3/jobs`:

```
GET /v3/jobs?filter[external_ids_contains]=<your job id>
```

If the lookup returns a record, `PATCH` it. Only `POST` when the lookup comes back empty.

## 3. Create with your identifier attached

- `createOrganization` — `POST /v3/organizations`. Required: `name` and `email`. Set
  `external_ids` on the way in. Dispatch **deduplicates organizations on creation** and
  will match an existing one rather than making a second; the documentation describes how
  to bypass deduplication when you genuinely need a separate record, so do not bypass it
  casually.
- `createCustomer` — `POST /v3/customers`. Required: `organization_id` and `first_name`.
  Note the shape constraints: **multiple `phone_numbers` but only a single `email`.** Each
  phone number is `{number, primary, type}` with `number` in RFC 3966 form; mark one
  `primary: true` — that is the number that receives SMS notifications.
- Addresses: `home_address` is used only when a job is created for that customer inside
  the Dispatch application. Jobs arriving through the API use the job's own `address`
  instead. Setting `home_address` and expecting it to drive an API-created job is a common
  mistake.

## 4. Store the Dispatch ID

Dispatch is explicit that external ID support is limited, and recommends you persist the
Dispatch integer ID for every object in your own system. Do that. Treat `external_ids` as
a recovery path, not as your primary key into Dispatch.

## 5. Keep job contacts current

Contacts are the people notified about a job, and they are separate from the customer.

- `listJobContacts` — `GET /v3/jobs/{id}/contacts`
- `updateJobContacts` — `PATCH /v3/jobs/{id}/contacts` for the whole set
- `updateJobContact` — `PATCH /v3/jobs/{id}/contacts/{contact_id}` for one

Contact methods are `{label, value, preferred}` triples under `phone_numbers` and
`email_addresses`. Exactly one of each should be `preferred: true`.

## 6. Reconcile ambiguous writes

When a write fails without a clear response — a timeout, a 5xx, a dropped connection — do
**not** blindly retry. Run the step-2 lookup first. If the record is there, the write
landed; switch to `PATCH`. This is the manual substitute for the idempotency contract
Dispatch does not offer.

## Deletes

`deleteCustomer` (`DELETE /v3/customers/{id}`) and `deleteOrganization` (`DELETE
/v3/organizations/{id}`) are not documented as reversible — unlike users, which deactivate
and can be restored. Confirm with a human before calling either from an automated flow.

## Rules that will bite you

- Paging is `limit`/`offset`, `limit` capped at 100. A lookup that returns a full page may
  have more behind it — page until short.
- Filter predicates differ by resource: customers use `filter[external_id_contains]`
  (singular), jobs and organizations use `filter[external_ids_contains]` (plural). Getting
  this wrong returns an unfiltered list, not an error.
- Locations are US and Canada only. `state` is the two-character abbreviation;
  `postal_code` is 5-digit US or 6-character Canadian.
- A `403` here usually means the organization already receives work from other sources and
  your ACL does not extend to its customers and staff. That is configuration, not a bug.

## Related

- `conventions/dispatch-conventions.yml` — the external-ids and dedupe contract in full
- `data-model/dispatch-data-model.yml` — how customers, organizations and jobs relate
- `errors/dispatch-problem-types.yml` — status catalog and remediation
