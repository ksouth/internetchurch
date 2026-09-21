---
name: internet-church-requirements
title: Internet Church Requirements
type: specification
version: 0.3.0
status: draft
description: Broad requirements for a provenance-aware personal web, media, research, and self-archive system.
tags:
  - internet-church
  - personal-archive
  - intellectual-archive
  - web-archiving
  - provenance
  - search
requires:
  - architecture_d2.md
applies_to:
  - software-design
  - personal-archive
  - digital-art-archive
updated: 2026-09-15
---

# Internet Church — Requirements

## 1. Purpose and continuity

Internet Church is a searchable external memory and documentary apparatus for things found, made, saved, discussed, researched, remembered, and lost online. It continues a long-running practice of examining how identity, memory, desire, attention, authorship, bodies, images, interfaces, and social systems become entangled through networked technology.

The system must support both ordinary personal capture and serious documentation of unstable digital culture. It is allowed to be messy at intake and highly rigorous at preservation.

## 2. Archive-method requirements

- Recover existing material before rebuilding or importing it again.
- Preserve source material before interpretation.
- Preserve originals byte-for-byte where possible; never silently replace them.
- Record provenance for every factual claim, transformation, import, and generated field.
- Distinguish observed/source material, firsthand memory or context, contemporaneous third-party material, later interpretation, and model inference.
- Preserve uncertainty exactly; do not invent certainty or doubt.
- Record what happened before trying to explain why.
- Keep documentary, personal, and intellectual archive layers distinct but cross-linkable.
- Treat photographs, video, audio, interfaces, and web ephemera as primary historical sources, not decoration.
- Preserve contradiction, unfinishedness, aliases, mixed registers, and strange associations instead of tidying them away.

## 3. Scope of archive objects

The system should represent:

- URLs, webpages, domains, feeds, and link collections
- PDFs, papers, books, scans, screenshots, images, video, audio, and other files
- Search terms and queries as records of curiosity, without implying endorsement or belief
- Notes, emails, messages, comments, captions, posts, and conversations
- Web artworks, exhibitions, writing, research, drafts, and project documentation
- Personal photographs, memories, people, places, events, and timelines
- Social-platform exports and future imports from Instagram, Twitter/X, Tumblr, Pocket, Evernote, Arena, Pinterest, and browser bookmarks
- Captures of the user’s own websites and online presence
- Related objects, versions, recaptures, references, influences, and consequences

The unfinished Instagram-comment and long-Twitter-post projects are future ingestion sources, not current implementation dependencies.

## 4. Capture and intake

- Accept URLs from iOS Share Sheet, including Chrome and Safari.
- Accept PDFs, images, screenshots, copied text, audio/video, email, and local files.
- Provide a private email or message intake for an archive assistant.
- Support browser extensions, a web interface, and batch imports.
- Provide an inbox where capture does not require immediate classification.
- Retain the original incoming message, attachment, filename, sender, timestamp, and supplied context.
- Detect likely duplicates without deleting separate capture events.
- Permit a single object to belong to multiple lists, projects, eras, subjects, or conceptual groupings.

## 5. Preservation and provenance

- Store original uploaded files unchanged.
- Attempt webpage preservation as HTML, PDF, screenshot, readable text, and assets where technically possible.
- Record source URL, final URL, redirects, capture timestamp, HTTP result, capture tool/version, and failure reason.
- Preserve local snapshots and extracted derivatives as separate representations.
- Generate checksums, byte sizes, MIME types, and preservation status.
- Support repeated captures and comparison over time.
- Keep partial, blocked, inaccessible, and failed captures visible.
- Record external archive references when available, but external archive submission is not a core dependency.
- Preserve the relationship between the original, derivatives, annotations, summaries, OCR, and later interpretations.

## 6. Description and interpretation

Each object should support:

- title, alternate titles, URL, dates, locations, authors, creators, people, and organisations
- tags, subjects, collections/lists, projects, eras, source type, status, and significance
- user-authored description, notes, memory, context, and why the item matters
- extracted text, OCR, captions, transcript, and machine-generated summary in separate fields
- relationships such as references, response-to, version-of, depicts, quotes, caused-by, and related-to
- open questions, unresolved provenance, uncertainty, and review state

Generated metadata must be labelled as generated. It must never replace source wording or user-authored context.

## 7. Search and navigation

Search is a primary function, not a convenience feature. The system must support retrieval when the remembered clue is partial, conceptual, visual, temporal, or wrong.

- Full-text search across metadata, notes, webpages, PDFs, OCR, transcripts, captions, and annotations.
- Exact search for URLs, titles, phrases, identifiers, filenames, and source terms.
- Natural-language and semantic search where available.
- Filters by date, domain, person, author, tag, collection, project, medium, file type, status, and preservation state.
- Search within a saved collection or across the whole archive.
- Show why an item matched and which representation contains the match.
- Make failed and unclassified items discoverable.
- Provide chronological, visual, list, relationship, and “searched terms” views.
- Support serendipitous browsing and juxtaposition, not only taxonomic filing.

## 8. Front-end and interaction

- Capture should be possible in a few taps.
- Intake should default to “save now, sort later.”
- Sorting should be lightweight: quick tags, list assignment, keyboard shortcuts, bulk actions, and reversible edits.
- The interface should support both utilitarian retrieval and visually meaningful arrangements.
- Lists should be additive and overlapping, not mutually exclusive folders.
- The system should make the archive’s temporal and associative structure visible.
- Accessibility, low cognitive load, readable typography, and mobile use are core requirements.

## 9. Archive assistant

The assistant may receive links, files, screenshots, queries, and freeform context by email or another private channel. It may preserve sources, extract metadata, suggest tags/lists, summarize, detect duplicates, link related objects, and report pending or failed work.

It must not claim a failed capture succeeded, replace sources with summaries, publish private material, overwrite originals, delete records, or present inference as fact. Every action must record input, timestamp, tools/models, output, status, and uncertainty. Confirmation is required for destructive actions, public sharing, and materially ambiguous privacy or interpretation decisions.

## 10. Ownership, export, and resilience

- Complete export without vendor lock-in.
- Filesystem-readable Markdown, JSON, CSV, HTML, and original binary exports.
- Stable identifiers and relationship-preserving manifests.
- Independent backups and tested restore procedures.
- Git-compatible versioned metadata and exports, while avoiding a repository full of unmanageable binaries.
- Private-by-default handling for personal, medical, legal, unpublished, or sensitive material.

## 11. Boundaries

The first version does not need to solve every social-platform import, perfect web capture, universal semantic understanding, or automatic historical interpretation. It must instead make incomplete work explicit, preserve recoverability, and provide a stable path for future ingestion.

## 12. Acceptance test

From Chrome on iPhone, share a page with no filing decision, preserve and index it, later find it by a remembered phrase or attached PDF, sort it into overlapping lists, inspect its source and local representations, distinguish source text from AI output and personal notes, and export the complete record with provenance intact.
