---
name: Submit a Trestle participant report
description: Report to each MLS which of its members a technology provider serves — look the members up in the Member resource, build the report payload, POST it to the Participant Reporting API with the right feed credentials, and read back the receipt.
api: Trestle Participant Reporting API (OData)
base_url: https://api.cotality.com/trestle/report
grounded_in:
  - https://trestle-documentation.corelogic.com/participant-reporting-api.html
  - https://trestle-documentation.corelogic.com/webapi.html
operations:
  - POST /trestle/oidc/connect/token
  - GET /trestle/odata/Member
  - POST /trestle/report/TpParticipantReport
  - GET /trestle/report/TpParticipantReport
  - POST /trestle/report/$metadata
---

# Submit a Trestle participant report

Participant reporting is how a technology provider evidences, to each MLS, the
broker/agent relationships behind a data feed. Several MLSs will only ratify a
licence if these reports keep flowing; others do not use the API at all and
require individual contracts hosted in Trestle instead. Check with each MLS
before automating.

## 0. Know which credential to use

Credentials are issued per **product / feed-type** pair, and each pair is
connected to a specific set of MLSs. You must submit each MLS's members using
the credentials of the product/feed-type connection that serves that MLS —
otherwise the report lands against the wrong MLS.

## 1. Get a token

```http
POST /trestle/oidc/connect/token HTTP/1.1
Host: api.cotality.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>&grant_type=client_credentials&scope=api
```

Use `scope=api` for a Web API feed and `scope=rets` for a RETS feed. Same
IdentityServer, same 8-hour Bearer token as the Web API.

## 2. Look up the members you are reporting

```http
GET /trestle/odata/Member?$filter=MemberEmail eq 'someone@example.com' and OriginatingSystemName eq '<MLSID>'
```

Trestle publishes the field mapping from the Member resource into the report
payload:

| Member field | Participant report field |
|---|---|
| `MemberFirstName` | `ParticipantFirstName` |
| `MemberLastName` | `ParticipantLastName` |
| `MemberEmail` | `ParticipantEmail` |
| `MemberType` | `ParticipantType` (fewer values than MemberType) |
| `MemberMlsId` | `ParticipantID` |
| `OfficeMlsId` | `ParticipantOfficeID` |
| `OfficeName` | `ParticipantOffice` |
| `OriginatingSystemName` | `OriginatingSystemName` |

## 3. Build and POST the report

```http
POST /trestle/report/TpParticipantReport
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "ReportDate": "2020-02-02",
  "ReportFrequency": "Daily",
  "Items": [
    {
      "OriginatingSystemName": "ACTRIS2",
      "ParticipantID": null,
      "ParticipantFirstName": "Joe",
      "ParticipantLastName": "Smith",
      "ParticipantEmail": "joesmith@mailinator.com",
      "ParticipantOfficeID": null,
      "ParticipantOffice": null,
      "ParticipantType": "Agent",
      "DisplayURLs": ["https://wwww.joesmithhomes.com"],
      "ServiceStartDate": "2019-08-22",
      "ServiceTerminationDate": null,
      "Notes": ""
    }
  ]
}
```

Field rules, verbatim from the reference:

- `ReportDate` — required, `YYYY-MM-DD`, **must be less than 60 days in the past**.
- `ReportFrequency` — required, `Daily` or `Monthly`, **must match the MLS's
  settings**.
- `Items` — required array of members.
- Each item must identify the member by **either** `ParticipantID` **or**
  `ParticipantFirstName` + `ParticipantLastName`.
- `ParticipantType` — required, `Agent` or `Broker`.
- `DisplayURLs` — required, array of well-formed URLs.
- `ServiceStartDate` — required; `ServiceTerminationDate` optional.

## 4. Read the receipt

A successful submission returns the stored receipt, including
`TpParticipantReportID`, `Recieved` (sic), and `ErrorCode: "OK"`.

## 5. Query past receipts

```http
GET /trestle/report/TpParticipantReport
```

Receipts are queryable with the same OData conventions as the Web API; the
unique key is `TpParticipantReportID`. The schema is at
`POST /trestle/report/$metadata`, returned as XML like the Web API's.

## Errors

A submission that fails any field requirement returns `400 Bad Request` with an
OData error object that names the offending item by index:

```json
{"error": {"code": "", "message": "Items[0]: \n This is not a URL! is not a well formed URL",
 "details": [{"code": "", "target": "Items[0]", "message": "Foo Bar Baz is not a well formed URL"}]}}
```

Read `details[].target` to find which array element failed. Other status codes
behave as on the Web API — see `errors/trestle-problem-types.yml`.

## Notes

- There is **no idempotency key**. Reports are keyed by `ReportDate` +
  `ReportFrequency` for a connection; re-submitting the same day is the
  documented correction path, but Trestle does not describe replay semantics.
- A spreadsheet download/upload alternative exists on the Participation
  Reporting tab in the Connection details area of the Trestle account UI.
