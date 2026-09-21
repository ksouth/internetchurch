---
name: internet-church-requirements
title: Internet Church Requirements
type: specification
version: 0.2.0
status: draft
description: Requirements for a provenance-aware personal web, media, and research archive.
tags:
  - internet-church
  - personal-archive
  - provenance
  - search
  - preservation
requires:
  - architecture.md
applies_to:
  - software-design
  - personal-archive
updated: 2026-09-15
---

# Internet Church — Requirements

Status: Draft 0.1  
Date: 2026-09-15

## 1. Purpose

Create one low-friction place to capture, preserve, describe, retrieve, and export links, webpages, PDFs, images, notes, and historical collections. The system must reduce capture friction without treating a URL as a durable copy of information.

## 1.1 Archive-method requirements

- Preserve the source before interpreting it.
- Distinguish observed/source material, firsthand user context, later interpretation, and model inference.
- Preserve uncertainty exactly; do not invent certainty or doubt.
- Cross-link documentary, personal, and intellectual archive layers without collapsing them into one narrative.
- Keep records additive, recoverable, and provenance-aware.

## 2. Design principles

- Capture first; classify later.
- Preserve originals and provenance.
- Never silently overwrite an existing object.
- Make uncertainty and failed captures visible.
- Keep the data exportable and independently backed up.
- Separate private material from anything intended for publication.
- Support both ordinary bookmarking and documentary/evidence-grade archiving.

## 3. Functional requirements

### Capture

- Accept URLs from iOS Share Sheet, including Chrome and Safari.
- Accept PDFs, images, screenshots, copied text, and local files.
- Support browser extensions and a web interface.
- Provide an inbox for uncategorised captures.
- Import browser bookmarks, Pocket, Evernote, and other exported collections where formats are available.
- Detect duplicates by canonical URL and content hash, while retaining separate capture events.

### Preservation

- Store the original uploaded file unchanged.
- Attempt webpage preservation as HTML, PDF, screenshot, and readable text where technically possible.
- Record capture time, source URL, final URL, HTTP result, and tool/version used.
- Record external archive references when available; submitting to external archives is out of scope for the core system.
- Record failed, partial, and blocked captures rather than presenting them as complete.
- Generate checksums for preserved files.
- Support re-capture and comparison of the same URL over time.

### Description and organisation

- Support title, description, tags, collections/lists, dates, authors, source types, subjects, status, and freeform notes.
- Permit multiple collections without duplicating the underlying object.
- Support highlights and annotations linked to preserved content.
- Allow manual correction of automatically fetched metadata.
- Keep machine-generated summaries/tags clearly distinguishable from user-authored notes.

### Search and retrieval

- Full-text search across metadata, notes, webpage text, PDF text, OCR text, and annotations.
- Filter by date, domain, tag, collection, file type, preservation status, and author.
- Support exact URL/title search as well as natural-language or semantic search if enabled.
- Show whether a result is an original link, local preservation, external archive, or failed capture.
- Open the source and every locally preserved representation from one record.

### Data ownership and export

- Provide complete export without vendor lock-in.
- Preserve stable identifiers and relationships on export.
- Support filesystem-readable formats such as Markdown, JSON, CSV, HTML, and original binary files.
- Permit routine independent backup and restore testing.
- Keep private collections private by default.

## 4. Non-functional requirements

- Mobile capture should take no more than a few taps.
- Search should remain useful with tens of thousands of records.
- The system should be self-hostable on available hosting.
- It should tolerate interrupted jobs and retry safely.
- It should support automation through an API, email intake, webhooks, or another documented interface; the specific interface is optional.
- It should be auditable: imports, edits, captures, and failures need timestamps.
- Sensitive URLs, notes, and documents must not be sent to third-party AI services without explicit configuration.

## 5. Priority

### Must have

Phone capture, PDF/file storage, webpage preservation, full-text search, tags/collections, export, backups, and visible capture failures.

### Should have

Historical imports, annotations, external archive references, duplicate detection, OCR, automation access, and recapture history.

### Could have

Semantic search, local AI tagging/summarisation, collaborative collections, RSS ingestion, and public sharing.

## 6. Archive assistant

The system should support a personal archive assistant that can receive messages by email or another low-friction channel and create properly attributed archive items.

The assistant may accept URLs, attachments, screenshots, copied text, and freeform context; preserve the source; suggest metadata; detect duplicates; and report successes, failures, and pending work.

It must not claim a failed capture succeeded, replace source material with a summary, publish private material, delete or overwrite records, or present generated metadata as user-authored fact. Every action should record the input, timestamp, action, result, tools/models used, and unresolved uncertainty.

## 7. Acceptance test for a first deployment

From Chrome on iPhone, share a page, save it without deciding its final category, later sort it into a list with minimal effort, find it by a word in the page or an attached PDF, open a local preserved copy after the original is unavailable, and export the record with its provenance intact.
