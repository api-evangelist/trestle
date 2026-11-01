# Trestle

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

Trestle is the real estate data distribution platform operated by CoreLogic (rebranded Cotality in 2025), sitting between roughly 500 US Multiple Listing Services and the technology providers, brokers and aggregators that consume their listing data. Its home market is the United States. Trestle occupies the licensing-and-transport layer of the residential real estate value chain: it maps each participating MLS into the RESO Data Dictionary and republishes it through a RESO Web API 2.0 / OData 4.0 endpoint and a legacy RETS 1.8 endpoint, plus a bidirectional Direct Web API into the Matrix MLS CRM and a Participant Reporting API used to prove broker relationships back to the MLSs. Its API posture is unusually honest for this sector: the documentation portal at trestle-documentation.corelogic.com is fully public and needs no login, the OAuth2 client-credentials flow and every OData query convention are published openly, and the OData service document at api.cotality.com/trestle/odata answers anonymously with a 200. But nothing behind it is reachable. The $metadata document and every entity set return 401 Bearer, and credentials are only issued after a developer registers a Technology Provider or Broker account, requests a connection to a specific multiple listing organization, and completes an e-signed data licence contract that all parties sign — a contract most MLSs will only ratify if a licensed broker or agent sponsors it or the technology provider files periodic participant reports. Trestle is RESO-certified and publicly documented, and still effectively uncallable without a signed licence. Certification is not reachability.

**Areas:** Real Estate · United States · MLS · RESO · Property Listings · IDX · PropTech · Data Distribution · OData · RETS · Listing Syndication

## Trestle on APIs.io

Everything profiled for Trestle is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/trestle/](https://apis.io/providers/trestle/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [Trestle RESO Web API](https://apis.io/apis/trestle/trestle-reso-web-api/) — The primary Trestle interface — a RESO Web API 2.0 / OData 4.0 endpoint that republishes MLS data mapped to the RESO Data Dictionary. The anonymously…
- [Trestle Direct Web API](https://apis.io/apis/trestle/trestle-direct-web-api/) — A bidirectional OData interface into the Matrix MLS database, separate from the RESO feed. The CRM reference documents Contacts, EmailHistory, Lists,…
- [Trestle Participant Reporting API](https://apis.io/apis/trestle/trestle-participant-reporting-api/) — A compliance API that lets technology providers report to each MLS which brokers they hold contracts with — the mechanism by which the licence relationship…
- [Trestle RETS](https://apis.io/apis/trestle/trestle-rets/) — Trestle's legacy Real Estate Transaction Standard interface, documented as compliant with RETS 1.8. RESO no longer updates the RETS specification, but…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [trestle-mcp.yml](https://apis.io/servers/trestle/trestle-mcp-yml/)

#### Collections (2)

Postman and Open Collection request collections.

- [API Collection](https://apis.io/collections/trestle/open-trestle-odata-service-document/)
- [Trestle WebAPI Demonstration](https://apis.io/collections/trestle/postman-trestle-webapi/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Trestle Authentication](https://apis.io/security/trestle/trestle-authentication/)
- [Trestle Domain Security](https://apis.io/security/trestle/trestle-domain-security/)
- [Trestle Vulnerability Disclosure](https://apis.io/security/trestle/trestle-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Trestle Scopes](https://apis.io/scopes/trestle/trestle-scopes/)

#### Rate limits (1)

Published request limits.

- [Trestle Rate Limits](https://apis.io/rate-limits/trestle/trestle-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.cotality.com/products/trestle)
- [Documentation](https://trestle-documentation.corelogic.com/)
- [Getting started](https://trestle-documentation.corelogic.com/webapi.html)
- [API reference](https://trestle-documentation.corelogic.com/webapi-reference.html)
- [Sign up](https://trestle.corelogic.com/SubscriptionWizard)
- [Support](https://www.cotality.com/support)
- [Changelog](https://trestle-documentation.corelogic.com/)
- [Blog](https://www.cotality.com/insights)
- [Security](https://www.cotality.com/legal/responsible-disclosure-policy)
- [Compliance](https://www.cotality.com/resources/article/corelogic-achieves-reso-data-dictionary-v2-0-vendor-certification)
- [Terms of service](https://www.cotality.com/legal/terms-of-use)
- [Privacy policy](https://www.cotality.com/legal/privacy-policy)
- [Status](https://status.cotality.com)

## API Evangelist rating

**Kin Score 45.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 62.5 |
| Contract quality | 11.9 |
| Contract governance | 18.2 |
| Operational transparency | 73.7 |
| Developer ergonomics | 60.1 |
| Access clarity | 42.1 |

**Agent readiness 18.3** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | — |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/trestle/](https://apis.io/providers/trestle/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Trestle: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/trestle/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
