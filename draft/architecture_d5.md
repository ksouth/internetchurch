---
name: internet-church-architecture
title: Internet Church Architecture
type: specification
version: 0.7.0
status: draft
description: Balanced architecture for a searchable, provenance-aware personal archive.
tags:
  - internet-church
  - architecture
  - personal-archive
  - preservation
  - search
  - imports
  - provenance
requires:
  - requirements_d2.md
applies_to:
  - software-design
  - personal-archive
updated: 2026-09-16
---

# Internet Church — Architecture

This version supersedes the emphasis of `architecture_d4.md` while preserving the broader detail in `architecture_d3.md`. The archive is the centre. Search, preservation, organisation, imports, export, and backup are primary. The archive assistant is one documented supporting subsystem.

## 1. System layers

```text
Capture and imports
        ↓
Intake and source preservation
        ↓
Archive records and representations
        ↓
Metadata, relationships, and versions
        ↓
Search and front end
        ↓
Exports, Git history, and independent backups
```

## 2. Organisational hierarchy

```text
Internet Church
├── 00-inbox
│   ├── incoming messages
│   ├── unprocessed URLs
│   ├── uploaded files
│   └── failed or uncertain intake
├── 01-records
│   └── IC-0000001/
│       ├── record.yaml
│       ├── record.md
│       ├── record.txt
│       ├── record.csv
│       ├── source/
│       ├── representations/
│       ├── versions/
│       ├── notes/
│       ├── provenance/
│       └── logs/
├── 02-collections
│   ├── projects
│   ├── subjects
│   ├── eras
│   ├── mediums
│   ├── lists
│   └── saved-searches
├── 03-imports
│   ├── pocket
│   ├── evernote
│   ├── tumblr
│   ├── facebook
│   ├── instagram
│   ├── twitter-x
│   ├── apple-notes
│   ├── icloud
│   ├── notion
│   ├── arena
│   └── browser-bookmarks
├── 04-indexes
│   ├── metadata
│   ├── full-text
│   ├── OCR
│   ├── relationships
│   ├── duplicates
│   └── failures
├── 05-exports
│   ├── csv
│   ├── plaintext
│   ├── markdown
│   ├── json
│   └── manifests
├── 06-audit
│   ├── intake-log
│   ├── processing-log
│   ├── import-ledger
│   ├── agent-actions
│   ├── backup-verification
│   └── decision-log
└── 07-system
    ├── schemas
    ├── connector definitions
    ├── automation definitions
    ├── agent policies
    └── configuration
```

## 3. Record identity and outputs

Each conceptual item receives a stable number such as `IC-0000001`. The number identifies the archive item, not a URL or a file. Each item produces a portable package:

```text
IC-0000001/
├── record.yaml          canonical structured metadata
├── record.md            human-readable presentation
├── record.txt           durable plaintext projection
├── record.csv           one-row tabular projection
├── source/              unchanged incoming material
├── representations/     HTML, PDF, image, OCR, transcript, media
├── versions/            capture/version inventory
├── notes/               user context and open questions
├── provenance/          sources, relationships, generated fields
└── logs/                intake and processing events
```

Canonical data is the original source, stable identity, provenance, user-authored decisions, version inventory, relationships, and audit history. Plaintext, CSV, indexes, OCR, transcripts, summaries, tags, embeddings, and previews are derived projections with generation provenance.

## 4. Minimum record fields

Every record should support:

```text
record_id, record_type, title, description, source_url, final_url,
file_type, mime_type, tags, lists, subjects, projects, people, authors,
visibility, source_kind, source_service, submitted_by, sent_by,
received_at, captured_at, original_filename, content_sha256,
preservation_status, search_status, user_notes, generated_summary,
generated_tags, uncertainty, open_questions, related_records
```

Separate tables/files should record representations, versions, relationships, intake events, processing events, imports, and audit events. A changed webpage creates a new version; it does not replace history.

## 5. Preservation, search, and front end

The system should preserve originals and attempt HTML, readable text, PDF, screenshot, OCR, transcript, and relevant media derivatives where appropriate. Missing, blocked, partial, failed, and not-attempted representations remain visible.

Search indexes should cover metadata, URLs, notes, webpage/PDF/OCR/transcript text, relationships, source services, failures, and preservation status. The front end should provide a low-friction inbox, overlapping lists, quick reversible sorting, chronological browsing, visual browsing, associative browsing, saved searches, and a “Searched Terms” view.

## 6. Imports and automation

Connectors for Pocket, Evernote, Tumblr, Facebook, Instagram, Twitter/X, Apple Notes, iCloud, Notion, Are.na, browser bookmarks, email attachments, IFTTT events, RSS, and local PDF folders must use source-preserving staging:

```text
source event/export
        ↓
unchanged staging copy
        ↓
import manifest and mapping report
        ↓
duplicate/reconciliation review
        ↓
archive records and representations
        ↓
indexes, exports, and audit report
```

The unfinished Instagram-comment and long-Twitter-post projects remain future connectors, not first-build dependencies.

## 7. Archive assistant

The assistant is an optional supporting subsystem. It may receive an email, iOS Shortcut submission, browser capture, or automation event; preserve the input; extract metadata; suggest tags, lists, and relationships; create derived search/export outputs; and report status.

It does not own the archive, define its ontology, or replace the front end. The archive must remain usable if the assistant, model, API, or hosting service disappears.

Assistant actions are recorded in `06-audit/agent-actions` with trigger, input, output, tool/model, timestamp, result, uncertainty, and confirmation requirement. Publishing, sharing, deletion, overwriting, disputed merging, privacy changes, and sending sensitive data elsewhere require confirmation.

## 8. Storage, Git, and backup

- Database: records, collections, relationships, audit events, and search configuration.
- Object storage: originals and large representations.
- Export bundles: record packages and checksum manifests.
- Git: schemas, metadata exports, notes, manifests, decision logs, and manageable text derivatives.
- Independent backup: database, object storage, imports, exports, and logs.

Git records archive and editorial history but is not required to hold every large binary or act as the sole backup.

## 9. Candidate boundary

Karakeep remains the provisional operational front end. ArchiveBox and Linkwarden remain preservation comparisons. Are.na is a historical import and design reference unless a permitted export route is established.

The first implementation should test a mixed corpus before narrowing the architecture: webpage, dead URL, PDF, screenshot, email attachment, social-export row, personal note, historical art artifact, and Are.na export sample.
