---
name: Build the Circle Pharma people roster
description: Assemble Circle Pharma's leadership, board of directors, senior management and scientific advisory board from the site's WordPress REST content API, segmented by the member_type taxonomy, with headshots.
api: openapi/circle-pharma-content-openapi.yml
operations: [listMemberTypes, listTeamMembers, getTeamMember, getMediaItem]
generated: '2026-08-01'
method: generated
source: openapi/circle-pharma-content-openapi.yml
---

# Build the Circle Pharma people roster

`team` is a custom content type on `https://circlepharma.com/wp-json` holding one record per person,
segmented by the hierarchical `member_type` taxonomy. It is the machine-readable form of the About Us
page.

**Base URL:** `https://circlepharma.com/wp-json`
**Auth:** none. Note that `/wp/v2/users` is **401** anonymously — `team` is the published roster, the
WordPress user list is not.

## 1. Resolve the segments

`listMemberTypes` — `GET /wp/v2/member_type?per_page=100&_fields=id,name,slug,count`

Observed 2026-08-01: Our Team `22` (52), Board of Directors `39` (7), Senior Management `54` (4),
Scientific Advisory Board `40` (3). Resolve by `slug`; note the board slug is misspelled upstream
(`about-board-of-directiors`) — match on `name` if the slug looks wrong.

## 2. List the people in a segment

`listTeamMembers` — `GET /wp/v2/team?member_type=39&per_page=100&orderby=title&order=asc&_embed&_fields=id,slug,link,title,content,member_type,featured_media,_links,_embedded`

- `title.rendered` is the person's name; `content.rendered` is their bio as HTML (it carries the role
  and credentials — there is no separate structured `title`/`role` field, so parse it or keep the HTML).
- `member_type` is an array of term ids; a person can appear in more than one segment.
- 61 records total as of 2026-08-01.

## 3. Headshots

With `_embed` on the list call, the image is at
`_embedded['wp:featuredmedia'][0].source_url` (with `alt_text` and `media_details.sizes` for
renditions). Without `_embed`, resolve `featured_media` via `getMediaItem` — `GET /wp/v2/media/{id}`.
`featured_media: 0` means no headshot.

## 4. Single person

`getTeamMember` — `GET /wp/v2/team/{id}` (or find by slug:
`GET /wp/v2/team?slug=shawn-cox`). A wrong or non-public id returns `404 rest_post_invalid_id`.

## Rules

- **Do not send `context=edit`** — it returns `401 rest_forbidden_context`. The default `view` context
  is what you want.
- **Cap `per_page` at 100.**
- **Treat this as public relations content, not an HR system.** Records reflect what the company chose
  to publish on its site; departures may lag.
- **Personal data.** These are published corporate bios. Use them for company research; do not
  enrich, cross-reference or redistribute them as a personal-data product.
- Cache aggressively — the roster changes a few times a year, and no rate-limit budget is published.
