---
name: arena-api-vetting
title: Are.na API Vetting for Internet Church
type: specification
version: 0.1.0
status: draft
description: Evidence-bounded assessment of Are.na as a source, interface, and automation target for Internet Church.
tags:
  - internet-church
  - are.na
  - api
  - imports
  - provenance
  - privacy
requires:
  - architecture_d4.md
applies_to:
  - software-design
  - personal-archive
  - data-import
updated: 2026-09-16
---

# Are.na API Vetting for Internet Church

## Executive finding

Are.na is an excellent conceptual and visual reference, and a plausible manual or carefully scoped source connector. It is **not currently cleared as the automatic bulk-mirroring backend** for Internet Church.

The current API documentation states that the API is intended for integrations rather than scraping or bulk data collection, and says automated crawling, systematic downloading, and structured data harvesting are prohibited. Bulk access for research or other purposes should be discussed with Are.na directly. This is a decisive constraint for a recurring 935-block mirror.

## What the current API supports

The v3 documentation exposes endpoints for:

- channels
- blocks
- connections
- comments
- feeds
- groups
- search
- users
- uploads
- authentication

It supports JSON responses, pagination, ETags/HTTP caching, read/write scopes, personal access tokens, OAuth with PKCE, and custom key/value metadata. Personal access tokens grant full account access and must not be placed in client-side code or public repositories.

## Strong fit

- Blocks can represent links, files, images, PDFs, embeds, and text.
- Channels are overlapping collections rather than exclusive folders.
- Channels can contain channels, allowing recursive organisation.
- Block connections preserve associative context and “learning trails.”
- Channel ordering carries curatorial meaning.
- Titles, descriptions, alt-text, source attribution, and context are first-class practices.
- Public, closed, and private channel states exist.
- The API can read and write content for authorised personal integrations.
- Are.na now offers official API documentation, SDK/MCP resources, and custom metadata.

## Gaps and risks

- API policy currently conflicts with unattended full-account harvesting or systematic downloading.
- A personal access token has full account access; it is not a narrowly scoped archive token.
- The API documentation does not by itself establish complete export of every comment, connection, media derivative, private object, or historical state.
- The API is not a preservation guarantee for the original webpage or uploaded file.
- Channel export is documented as a user-triggered download; exact ZIP/HTML/PDF contents need direct testing.
- Deletion, renamed channels, changed visibility, and disconnected blocks require careful reconciliation.
- Rate limits and pagination make naive complete-account polling unsafe and explicitly contrary to the documented guidance.

## Recommended status in Internet Church

| Role | Status |
|---|---|
| Design and interaction reference | Strong yes |
| Manual source import | Yes, after export testing |
| User-triggered targeted sync | Possible, subject to policy and rate limits |
| Unattended full-account mirror | Not cleared |
| Sole preservation source | No |
| Future assistant destination | Possible, with explicit user action and safe credentials |
| Future public/community integration | OAuth required |

## Safe next step

Before writing an importer that traverses all channels and blocks, contact Are.na with a precise request: a private, user-authorised archival export of one account for personal preservation, including blocks, channels, connections, ordering, metadata, comments, visibility, and available media. Ask whether scheduled incremental reads are permitted and what endpoint/export they recommend.

Until that is answered, preserve any user-triggered Are.na channel downloads as original imports and build the connector around explicit, bounded exports rather than an automatic crawler.

## Sources checked

- [Are.na API documentation](https://www.are.na/developers/explore)
- [Are.na developers overview](https://www.are.na/developers)
- [Personal access tokens](https://www.are.na/developers/personal-access-tokens)
- [Channel export documentation](https://help.are.na/docs/getting-started/channels/settings-and-export)
- [Blocks and attribution guidance](https://help.are.na/docs/guides/handling-blocks-with-care)
