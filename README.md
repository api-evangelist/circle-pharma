# Circle Pharma

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
