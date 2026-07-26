---
name: Schedule and track an appointment
description: >-
  Move a job from unscheduled to complete - find the job, assign a technician, schedule the
  appointment, follow it through the enroute/started/complete status machine, and read the
  customer's survey response.
api: openapi/dispatch-rest-v3-openapi.yml
operations:
  - createToken
  - listJobs
  - getJob
  - listUsers
  - createAppointment
  - listAppointments
  - getAppointment
  - updateAppointment
  - createJobTimeWindow
  - listSurveyResponses
generated: '2026-07-20'
method: generated
source: https://github.com/DispatchMe/v3-api-docs
---

# Schedule and track an appointment

This is the service-provider side of Dispatch: work has arrived at your organization and
you need to put a technician on it and follow it to completion.

## 1. Authenticate

Call `createToken` — `POST /v3/oauth/token`. As an individual service provider use the
`password` grant:

```json
{ "grant_type": "password", "username": "<email>", "password": "<password>",
  "client_id": "<public key>", "client_secret": "<secret key>" }
```

Dispatch recommends creating a **separate user account with the `dispatcher` role for the
integration**, so integration actions are distinguishable from real user actions in the
audit trail. Do that rather than reusing a person's credentials.

Send `Authorization: Bearer <access_token>`. On `401`, refresh with the `refresh_token`
grant and retry once.

## 2. Find the work

`listJobs` — `GET /v3/jobs` — with filters:

```
GET /v3/jobs?filter[organization_id_eq]=<org>&filter[status_eq]=unscheduled&limit=100
```

Paging is `limit` and `offset`; `limit` maxes out at 100. Read a single job with `getJob`
— `GET /v3/jobs/{id}`.

## 3. Pick a technician

`listUsers` — `GET /v3/users` — filtered to your organization and the technician role:

```
GET /v3/users?filter[organization_id_eq]=<org>&filter[by_user_roles]=technician
```

A user may hold both `dispatcher` and `technician` roles. Technician-only users cannot
sign in to the desktop application, only the mobile app.

## 4. Offer times, or schedule directly

If the customer should choose, call `createJobTimeWindow` — `POST
/v3/jobs/{job_id}/time_windows` — with `{start_time, end_time}` pairs in ISO 8601.

To schedule directly, call `createAppointment` — `POST /v3/appointments`:

- `job_id` — required, the parent job
- `status` — required, start at `scheduled` (or `draft` if information is still missing)
- `time` — ISO 8601 timestamp
- `duration` — seconds; **defaults to 7200 (two hours)** if you omit it
- `user_id` — the assigned technician

**Side effect:** scheduling an appointment also moves the parent job to `scheduled`. You
do not need a separate `updateJob` call for that.

## 5. Follow the status machine

Appointment statuses are `draft`, `scheduled`, `enroute`, `started`, `complete`,
`canceled`, and they move freely between each other. Technicians normally drive these from
the mobile app; `updateAppointment` — `PATCH /v3/appointments/{id}` — is how an integration
does it.

Poll with `listAppointments` — `GET /v3/appointments` — using the time and status filters:

```
GET /v3/appointments?filter[organization_id_eq]=<org>&filter[time_gteq]=2026-07-20T00:00:00Z&filter[status_in]=scheduled,enroute,started
```

Useful filters: `filter[user_id_eq]` for one technician, `filter[user_id_null]=true` for
unassigned appointments, `filter[job_id_in]` for a batch of jobs. Read one with
`getAppointment` — `GET /v3/appointments/{id}`.

If your account has webhooks configured (account-manager only, not self-service), prefer
them over polling.

## 6. Read the survey

When an appointment or job completes Dispatch surveys the customer. Call
`listSurveyResponses` — `GET /v3/survey_responses?filter[job_id_eq]={id}` — for the `rating`
(0–5) and free-text `message`. `appointment_id` may be null if there was no appointment.

## Rules that will bite you

- **Cancelling a job cancels all of its child appointments.** Do not cancel a job to clear
  a single bad appointment — update or delete that appointment instead.
- **No idempotency key.** A retried `POST /v3/appointments` can double-book a technician.
  Before retrying an ambiguous failure, call `listAppointments` with
  `filter[job_id_eq]` to check whether the first attempt landed.
- **Statuses are unconstrained.** Dispatch permits free movement between statuses, so your
  integration owns the workflow discipline, not the API.
- Times are ISO 8601; durations are seconds. Locations are US and Canada only.
- Capture `X-Request-Id` from every response for support escalation.

## Related

- `conventions/dispatch-conventions.yml` — filter predicates, paging, tracing
- `errors/dispatch-problem-types.yml` — the full status catalog
- `data-model/dispatch-data-model.yml` — the job/appointment/user graph
- `asyncapi/dispatch-webhooks.yml` — the webhook surface and the polling fallback
