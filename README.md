# Trestle (trestle)

Trestle is the real estate data distribution platform operated by CoreLogic (rebranded Cotality in 2025), sitting between roughly 500 US Multiple Listing Services and the technology providers, brokers and aggregators that consume their listing data. Its home market is the United States. Trestle occupies the licensing-and-transport layer of the residential real estate value chain: it maps each participating MLS into the RESO Data Dictionary and republishes it through a RESO Web API 2.0 / OData 4.0 endpoint and a legacy RETS 1.8 endpoint, plus a bidirectional Direct Web API into the Matrix MLS CRM and a Participant Reporting API used to prove broker relationships back to the MLSs. Its API posture is unusually honest for this sector: the documentation portal is fully public and needs no login, the OAuth2 client-credentials flow and every OData query convention are published openly, and the OData service document answers anonymously with a 200. But nothing behind it is reachable. The `$metadata` document and every entity set return 401 Bearer, and credentials are only issued after a developer registers a Technology Provider or Broker account, requests a connection to a specific multiple listing organization, and completes an e-signed data licence contract that all parties sign — a contract most MLSs will only ratify if a licensed broker or agent sponsors it or the technology provider files periodic participant reports. Trestle is RESO-certified and publicly documented, and still effectively uncallable without a signed licence. Certification is not reachability.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/trestle/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/trestle/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United States
- MLS
- RESO
- Property Listings
- IDX
- PropTech
- Data Distribution
- OData
- RETS
- Listing Syndication

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## RESO Posture

- **RESO Web API:** Platinum certification for the Trestle Web API listing transmission service.
- **RESO Data Dictionary:** v2.0 vendor certification, achieved 2024-10-11.
- **RESO UPI:** first multiple listing data distributor certified on UPI v2.0.
- **Reachability:** the OData service document at `https://api.cotality.com/trestle/odata` is anonymously readable (200). The `$metadata` document and every entity set return **401 Bearer**. Certified per the public record, but no endpoint is reachable without an MLS data licence agreement.

## Access Gate

**Licence agreement.** Register a Technology Provider or Broker account at `https://trestle.corelogic.com`, then request a connection to each individual multiple listing organization. Trestle walks the applicant through an e-signing contract collecting signatures from all parties and authorizing billing of that MLO's licence fee. Most MLOs additionally require a licensed broker or agent to ratify the contract, or periodic participant reports naming which members use the service. Credentials are tied to a specific product/feed type. No sandbox is documented.

## APIs

### Trestle RESO Web API

The primary Trestle interface — a RESO Web API 2.0 / OData 4.0 endpoint that republishes MLS data mapped to the RESO Data Dictionary. The anonymously readable OData service document advertises 18 entity sets: Property, Office, Member, Media, OpenHouse, CustomProperty, PropertyRooms, PropertyUnitTypes, Teams, TeamMembers, Field, Lookup, Model, PropertyGreenVerification, Building, HistoryTransactional, DataSystem and Enumeration. Authentication is OAuth2 `client_credentials` against `/trestle/oidc/connect/token` with `scope=api`, returning an 8-hour Bearer token. Documented quotas are 7,200 queries/hour and 180 queries/minute for the Web API and 18,000/hour and 480/minute for media URLs.

- **Human URL:** [https://trestle-documentation.corelogic.com/webapi.html](https://trestle-documentation.corelogic.com/webapi.html)
- **Base URL:** `https://api.cotality.com/trestle/odata`

#### Tags

- RESO
- OData
- Property Listings
- MLS
- Real Estate

#### Properties

- [Documentation](https://trestle-documentation.corelogic.com/webapi.html)
- [API Reference](https://trestle-documentation.corelogic.com/webapi-reference.html)
- [OData Service Document](openapi/trestle-odata-service-document.json)
- [Documentation — RESO Common Format](https://trestle-documentation.corelogic.com/reso-common-format.html)
- [Documentation — Web API at Scale](https://trestle-documentation.corelogic.com/webapi-at-scale.html)
- [Documentation — Web API Libraries](https://trestle-documentation.corelogic.com/webapi-libraries.html)
- [Data Model](https://api.cotality.com/trestle/Documentation/MetaData/Resource/Property)
- [Enumerations](https://api.cotality.com/trestle/Documentation/MetaData/Enums)

### Trestle Direct Web API

A bidirectional OData interface into the Matrix MLS database, separate from the RESO feed. The CRM reference documents Contacts, EmailHistory, Lists, PortalContents, SavedSearches, UserRegistry and an aggregated DashboardAPI with read and write operations; a companion MLO reference covers multiple-listing-organization integration. Authentication is OpenID Connect, with Clareity Single Sign-On authorization and HTTP Basic named as alternatives, and access is further constrained by the acting user's privileges in Matrix. No explicit base URL is published in the reference, so none is asserted here.

- **Human URL:** [https://trestle-documentation.corelogic.com/direct-webapi-crm-reference.html](https://trestle-documentation.corelogic.com/direct-webapi-crm-reference.html)

#### Tags

- Matrix MLS
- CRM
- OData
- Real Estate

#### Properties

- [API Reference — CRM](https://trestle-documentation.corelogic.com/direct-webapi-crm-reference.html)
- [API Reference — MLO](https://trestle-documentation.corelogic.com/direct-webapi-mlo-reference.html)

### Trestle Participant Reporting API

A compliance API that lets technology providers report to each MLS which brokers they hold contracts with — the mechanism by which the licence relationship behind a data feed is evidenced. Documented operations are POST and GET on `/trestle/report/TpParticipantReport`, authenticated with the same OAuth2 `client_credentials` flow as the Web API (`scope=api` for Web API feeds, `scope=rets` for RETS feeds). Not every MLS uses it; some require individual contracts hosted in Trestle instead.

- **Human URL:** [https://trestle-documentation.corelogic.com/participant-reporting-api.html](https://trestle-documentation.corelogic.com/participant-reporting-api.html)
- **Base URL:** `https://api.cotality.com/trestle/report`

#### Tags

- Compliance
- Reporting
- MLS
- Real Estate

#### Properties

- [API Reference](https://trestle-documentation.corelogic.com/participant-reporting-api.html)

### Trestle RETS

Trestle's legacy Real Estate Transaction Standard interface, documented as compliant with RETS 1.8. RESO no longer updates the RETS specification, but Trestle continues to serve it for customers who cannot move to the Web API. Login is `POST /trestle/rets/login`; the service is session-less and supports either HTTP Basic with a Base64-encoded `client_id:client_secret` or an OAuth2 bearer token obtained with `scope=rets`. Digest authentication is explicitly not supported.

- **Human URL:** [https://trestle-documentation.corelogic.com/rets.html](https://trestle-documentation.corelogic.com/rets.html)
- **Base URL:** `https://api.cotality.com/trestle/rets`

#### Tags

- RETS
- Legacy
- MLS
- Real Estate

#### Properties

- [Documentation](https://trestle-documentation.corelogic.com/rets.html)
- [Documentation — RETS Libraries](https://trestle-documentation.corelogic.com/rets-libraries.html)
- [Documentation — RETS Connector](https://trestle-documentation.corelogic.com/rets-connector.html)

## Common Properties

- [Website](https://www.cotality.com/products/trestle)
- [Documentation](https://trestle-documentation.corelogic.com/)
- [FAQ](https://trestle-documentation.corelogic.com/faq.html)
- [Sign Up](https://trestle.corelogic.com/SubscriptionWizard)
- [Login](https://trestle.corelogic.com/Login)
- [Support](https://www.cotality.com/support)
- [Email](mailto:trestlesupport@cotality.com)
- [Certification — RESO Data Dictionary v2.0](https://www.cotality.com/resources/article/corelogic-achieves-reso-data-dictionary-v2-0-vendor-certification)
- [Certification — RESO Platinum Web API](https://www.reso.org/blog/corelogic-trestle-achieves-reso-platinum-certification-2/)

## Maintainers

- Kin Lane — kin@apievangelist.com
