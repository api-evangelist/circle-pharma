# Circle Pharma

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

Circle Pharma is a clinical-stage biopharmaceutical company in South San Francisco, California,
developing cell-permeable, orally bioavailable macrocycle therapies for cancer. Founded in 2013, the
company applies its proprietary MXMO structure-based design platform to protein-protein interactions
that conventional small molecules cannot reach. Its pipeline targets cyclins: lead program CID-078, a
first-in-class oral cyclin A/B RxL inhibitor, is in Phase 1 for advanced solid tumors, followed by a
preclinical cyclin D1 RxL inhibitor and undisclosed cyclin programs partnered with Boehringer
Ingelheim.

- Website: https://circlepharma.com/
- Contact: 169 Harbor Way, South San Francisco, CA 94080 — info@circlepharma.com
- Secondary market listing: https://forgeglobal.com/circle-pharma_stock/

## API surface

Circle Pharma runs **no developer program** and markets **no API**. It publishes no OpenAPI, no
GraphQL endpoint, no MCP server, no A2A agent card, no webhooks or events, no `llms.txt` and no
`/.well-known/` documents — all probed on 2026-08-01 and recorded in
[`well-known/circle-pharma-well-known.yml`](well-known/circle-pharma-well-known.yml).

The one machine-readable surface the company exposes is the **anonymously readable WordPress REST
content API** behind circlepharma.com, published at `https://circlepharma.com/wp-json/` (349 routes
across 18 namespaces). It serves press releases, publications and in-the-news items, site pages, the
leadership/board roster, the events calendar and site search without credentials.

[`openapi/circle-pharma-content-openapi.yml`](openapi/circle-pharma-content-openapi.yml) is an
OpenAPI 3.1 document **derived by API Evangelist** from that published route index and verified
against live responses. It is not a Circle Pharma specification and the company does not support it
as a product.
