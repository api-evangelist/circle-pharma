---
name: Assemble a Circle Pharma company brief
description: Pull Circle Pharma's pipeline, science, clinical-trial and investor narrative plus its upcoming events and latest news into one structured brief, using only the site's anonymously readable content API.
api: openapi/circle-pharma-content-openapi.yml
operations: [getRouteIndex, listTypes, listPages, getPage, listEvents, listEventTypes, listPosts, search]
generated: '2026-08-01'
method: generated
source: openapi/circle-pharma-content-openapi.yml
---

# Assemble a Circle Pharma company brief

Everything Circle Pharma publishes about its platform, pipeline and programs is reachable as JSON
from `https://circlepharma.com/wp-json` — no scraping, no credentials.

**Base URL:** `https://circlepharma.com/wp-json`
**Auth:** none.

## 0. Confirm the surface is what you think it is

`getRouteIndex` — `GET /` returns the site name, description and the full route list.
`listTypes` — `GET /wp/v2/types` confirms which content types exist (`post`, `page`, `team`,
`all_events` are the content ones; the rest are WordPress editorial internals — ignore them).

## 1. Narrative pages

`listPages` — `GET /wp/v2/pages?per_page=100&_fields=id,slug,title,link,parent,menu_order`

20 pages as of 2026-08-01. The ones that carry the substance:

| slug | what it holds |
|---|---|
| `about-us` | founding (2013), leadership, board |
| `our-science` | macrocycle chemistry, the MXMO platform |
| `our-pipeline` | CID-078 (cyclin A/B RxL, Phase 1) and the preclinical cyclin programs |
| `clinical-trials` | trial information and the Expanded Access Policy |
| `investors` | investor information |
| `work-with-us` | careers |

`getPage` — `GET /wp/v2/pages/{id}?_fields=title,content,link` for the body. `content.rendered` is
HTML; strip tags for text extraction and keep `link` for citation.

## 2. Events

`listEvents` — `GET /wp/v2/all_events?per_page=100&_fields=id,title,content,link,event_type,date`
`listEventTypes` — `GET /wp/v2/event_type` for the year / "Upcoming Events" buckets.

Small collection (5 records observed); read all of it in one call.

## 3. Recent news for the "what changed" section

`listPosts` — `GET /wp/v2/posts?per_page=10&orderby=date&order=desc&_fields=id,date,title,link,categories`
See `skills/circle-pharma-track-news.md` for the category ids and the incremental sync loop.

## 4. Targeted lookups

`search` — `GET /wp/v2/search?search=CID-078&per_page=20` returns a light
`{id,title,url,type,subtype}` projection across all public content — the cheapest way to find every
page and post mentioning a program code without paging each collection.

## Rules

- **Do not present this as clinical or medical guidance.** For trial status, enrollment and safety,
  the authoritative sources are ClinicalTrials.gov and the company's own clinical-trials page — not a
  cached copy of a CMS record. Always cite `link` and the record's `modified` date.
- **The pipeline changes.** Program stages (Phase 1, preclinical, discovery) move; never report a
  stage without the `modified` timestamp of the page you read it from.
- **Read-only, no rate-limit budget published.** Cache for at least 10 minutes; honor
  `Crawl-delay: 10`.
- **Errors** follow the WordPress envelope; see `errors/circle-pharma-problem-types.yml`.
- **This is not a Circle Pharma developer product.** The OpenAPI in this repo was derived from the
  site's route index by API Evangelist; do not describe it to a user as an API the company supports.
