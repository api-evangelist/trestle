---
name: Replicate Trestle listings and media
description: Authenticate to the Trestle RESO Web API, do an initial load of Property records, pull the associated Media, then keep the copy current with incremental updates and periodic reconciliation — all inside the published quota.
api: Trestle RESO Web API (OData 4.0 / RESO Web API 2.0)
base_url: https://api.cotality.com/trestle/odata
grounded_in:
  - postman/trestle-webapi.postman_collection.json
  - https://trestle-documentation.corelogic.com/webapi.html
  - https://trestle-documentation.corelogic.com/webapi-at-scale.html
  - data-model/trestle-data-model.yml
operations:
  - POST /trestle/oidc/connect/token
  - GET /trestle/odata/Property
  - GET /trestle/odata/Media
  - GET /trestle/odata/$metadata
---

# Replicate Trestle listings and media

Trestle has no OpenAPI and no operationIds. Every request below is a documented
HTTP method + path taken verbatim from Trestle's own documentation and its
published Postman collection. Do not invent entity sets or query parameters —
the 18 available entity sets are listed in
`openapi/trestle-odata-service-document.json`.

## 0. Precondition — you cannot do this without a licence

Trestle credentials are not self-service. A Technology Provider or Broker
account must be registered at <https://trestle.corelogic.com/SubscriptionWizard>,
a connection requested to each individual multiple listing organization, and an
e-signed data licence contract executed by all parties. Without that, every
request below returns `401` with `www-authenticate: Bearer`. If you do not have
`client_id` / `client_secret`, stop — there is no sandbox and no trial key.

## 1. Get a token

```http
POST /trestle/oidc/connect/token HTTP/1.1
Host: api.cotality.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>&grant_type=client_credentials&scope=api
```

- `scope=api` for the Web API. Use `scope=rets` only for a RETS feed — mixing
  them returns `400`.
- The response is `{access_token, expires_in: 28800, token_type: "Bearer"}`.
  Cache the token and its expiry; refresh only when it is about to lapse.
  Send it as `Authorization: Bearer <access_token>` on every later request.

## 2. Read the metadata for your feed

```http
GET /trestle/odata/$metadata
```

This CSDL document is the authoritative list of resources, fields and
enumerations **for your licence** — it differs per subscriber. Field-level
documentation is also published anonymously in HTML at
`https://api.cotality.com/trestle/Documentation/MetaData/Resource/<Resource>`.

## 3. Initial load of Property

```http
GET /trestle/odata/Property?$top=1000&$expand=Rooms,Units,OpenHouse,CustomProperty,Media
```

- `$top` maxes out at **1000**. Follow `@odata.nextLink` in the response rather
  than tracking `$skip` yourself.
- For a full initial load, use the replication extension —
  `GET /trestle/odata/Property?Replication=true` — which is documented as able
  to return more than 1,000,000 records.
- `$expand` is the main quota-saving lever: every expanded resource you fetch
  inline is a query you do not spend. See `data-model/trestle-data-model.yml`
  for the exact navigation-property names (`Rooms`, `UnitTypes`, `Media`,
  `ListAgent`, `ListOffice`, `Building`, …).

## 4. Fetch media

Media is not embedded in the listing; it is a separate resource joined by key.

```http
GET /trestle/odata/Media?$filter=ResourceRecordKey eq '<ListingKey>'&$orderby=Order
```

Each record carries a `MediaURL`, a publicly fetchable image URL. Those
downloads are metered against a **separate, larger** quota (18,000/hour,
480/minute) than API queries — replicate images on their own budget, or lazily.

## 5. Keep up with changes

```http
GET /trestle/odata/Property?$filter=ModificationTimestamp gt <last_seen>&$top=1000
```

- Track `ModificationTimestamp` for Property (and Rooms/Units/OpenHouse/
  CustomProperty); track `PhotosChangeTimestamp` — which lives on the **Property**
  record, not on Media — for photo changes.
- Store the newest timestamp **you received from Trestle**, not your own clock,
  so clock skew cannot make you skip records.
- Run every few minutes to every hour.

## 6. Reconcile deletions

Incremental pulls never tell you a listing went away. Periodically pull the key
list and delete anything local that is missing:

```http
GET /trestle/odata/Property?$select=ListingKey&$top=300000
```

Key-only (`InKeyIndex`) queries lift the 1,000-record ceiling to 300,000. The
InKeyIndex field list is per-resource — see `data-model/trestle-data-model.yml`.
Above 1,000,000 records, use the replication endpoint instead.

## Rules that apply to every step

- **Quota.** Baseline 7,200 queries/hour and 180/minute for the Web API. Read
  `Minute-Quota-Limit`, `Hour-Quota-Limit` and `Hour-Quota-ResetTime` off every
  response (they are returned even on a `401`) and pace against them. See
  `rate-limits/trestle-rate-limits.yml`.
- **Backoff.** On `429`, wait and retry with exponential backoff (1, 2, 4, 8,
  16, 32 s), not a fixed sleep.
- **Errors.** `400` bad credentials/connection type, `401` missing or expired
  token, `404` resource not present *or not licensed to you*, `429` quota,
  `500` Trestle error, `504` query too slow — narrow the `$filter` or reduce
  `$expand`. See `errors/trestle-problem-types.yml`.
- **No idempotency contract.** This surface is read-only; there is no
  `Idempotency-Key` and no retry-safety guarantee for writes.
- **No events.** There are no webhooks and no AsyncAPI. Polling is the only
  change-capture mechanism.
- **Mass updates happen.** Source MLSs can touch every row (the Data Dictionary
  2.0 rollout alone was an expected 200m+ row update). Do not assume a small
  delta.
- **Host migration.** Use `api.cotality.com`. `api-prod.corelogic.com` and
  `api-trestle.corelogic.com` still answer but are announced as deprecated;
  see `lifecycle/trestle-lifecycle.yml`.
