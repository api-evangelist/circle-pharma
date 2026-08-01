---
name: Track Circle Pharma news and publications
description: Incrementally pull Circle Pharma press releases, publications and in-the-news items from the site's WordPress REST content API, segmented by category, without scraping HTML.
api: openapi/circle-pharma-content-openapi.yml
operations: [listCategories, listPosts, getPost, listMedia]
generated: '2026-08-01'
method: generated
source: openapi/circle-pharma-content-openapi.yml
---

# Track Circle Pharma news and publications

Circle Pharma publishes no developer program. Its corporate news is nonetheless available as JSON
through the WordPress REST API the site serves at `https://circlepharma.com/wp-json`. Use it instead
of scraping `https://circlepharma.com/press-releases`.

**Base URL:** `https://circlepharma.com/wp-json`
**Auth:** none. Every call below is anonymous; do not send credentials.

## 1. Resolve the category ids (once, then cache)

`listCategories` — `GET /wp/v2/categories?per_page=100&_fields=id,name,slug,count`

The three news streams the site renders as separate sections are categories on one collection. As of
2026-08-01: Press Releases `33`, Publications `32`, In the News `34`. Ids are stable but re-resolve by
`slug` rather than hardcoding.

## 2. Page the stream

`listPosts` — `GET /wp/v2/posts?categories=33&per_page=100&orderby=date&order=desc&_fields=id,date,modified,slug,link,title,excerpt,categories,featured_media`

- `per_page` maximum is **100**; asking for more returns `400 rest_invalid_param`.
- Read `X-WP-Total` and `X-WP-TotalPages` from the response headers, or follow the `Link` header's
  `rel="next"`. Requesting a page beyond the last returns `400 rest_post_invalid_page_number` — stop
  when `page == X-WP-TotalPages`, do not probe past it.
- `title`, `excerpt` and `content` are **objects** with a `rendered` HTML string. Read
  `title.rendered`, not `title`.

## 3. Sync incrementally

Store the highest `modified_gmt` you have seen. On the next run:

`GET /wp/v2/posts?categories=33&modified_after=2026-05-07T15:36:41&orderby=modified&order=asc&per_page=100`

`after` / `before` filter on publication date; `modified_after` / `modified_before` catch edits to
already-published items. Use `modified_after` for a sync loop, `after` for a "what's new" digest.

## 4. Fetch the full body only when you need it

`getPost` — `GET /wp/v2/posts/{id}` returns `content.rendered` as HTML. Prefer requesting
`_fields=id,title,content,link,date` to avoid pulling the whole record.

## 5. Images

Each post may carry `featured_media` (an id; `0` means none). Either resolve it with
`getMediaItem` — `GET /wp/v2/media/{id}?_fields=source_url,alt_text` — or add `_embed` to the list
call and read `_embedded['wp:featuredmedia'][0].source_url` in the same round trip.

## Rules

- **Read-only.** There is no write operation, no idempotency key and no webhook on this surface. Never
  attempt POST/PUT/DELETE; they exist on these routes but are authenticated administrative endpoints.
- **No rate-limit headers are published.** The origin sets `cache-control: max-age=600` and
  `robots.txt` asks for `Crawl-delay: 10`. Poll no more than once every 10 minutes, send
  `If-Modified-Since`, and cache.
- **Errors** are `{"code","message","data":{"status"}}` — not RFC 9457. On `rest_invalid_param`, read
  `data.params` for the exact constraint. See `errors/circle-pharma-problem-types.yml`.
- **Do not use `/wp/v2/comments`** as a company signal; it is anonymously readable but is dominated by
  inbound link spam.
- **Attribution.** This is Circle Pharma's own published content. Quote and link to `link`; do not
  present the derived OpenAPI in this repo as a Circle Pharma specification.
