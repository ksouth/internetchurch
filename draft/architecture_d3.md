---
name: internet-church-architecture
title: Internet Church Architecture
type: specification
version: 0.4.0
status: draft
description: Organisational and data architecture for a searchable, auditable personal archive of networked life.
tags:
  - internet-church
  - architecture
  - personal-archive
  - digital-archive
  - provenance
  - imports
  - automation
  - search
requires:
  - requirements_d2.md
applies_to:
  - software-design
  - personal-archive
  - digital-art-archive
updated: 2026-09-15
---

# Internet Church — Architecture

## 1. Role of this document

The requirements document defines the desired behaviour and safeguards. This document defines the proposed organisation of the archive, the lifecycle of an item, the outputs produced for each item, and the boundaries between capture, preservation, enrichment, search, export, and backup.

This is a draft design, not an implementation claim. Candidate software must be tested against this structure before adoption.

## 2. Architectural idea

Internet Church is an **archive-of-record with a searchable front end**. Every incoming thing becomes an auditable archive item with a stable record number. A URL is one possible source, not the whole record.

```text
many inputs
  ↓
one immutable intake event
  ↓
one stable archive item number
  ↓
many preserved representations and metadata derivatives
  ↓
one searchable, exportable record package
```

The archive must allow rapid, messy intake while ensuring that later processing is additive, traceable, and reversible.

## 3. Organisational hierarchy

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
│       ├── record.csv
│       ├── record.txt
│       ├── source/
│       ├── representations/
│       ├── versions/
│       ├── notes/
│       ├── provenance/
│       └── logs/
├── 02-collections
│   ├── by project
│   ├── by subject
│   ├── by era
│   ├── by medium
│   └── saved searches
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
│   ├── browser-bookmarks
│   └── other
├── 04-indexes
│   ├── full-text
│   ├── metadata
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
│   ├── backup-verification
│   └── decision-log
└── 07-system
    ├── schemas
    ├── connector definitions
    ├── automation definitions
    └── configuration
```

This is a logical hierarchy. A live application may store the same structure in a database and object storage, but its exports must be able to reproduce it.

## 4. Archive item identity

Every item receives a stable human-readable record number, for example:

```text
IC-0000001
```

The number identifies the archive record, not a particular file or URL. One item may have many captures and representations. Separate intake events may later be linked to one item when they refer to the same underlying object, but the original events remain preserved.

Recommended identifiers:

- `record_id`: stable archive number, e.g. `IC-0000001`
- `intake_id`: unique incoming event
- `representation_id`: unique source or derivative
- `version_id`: unique capture/version of a representation
- `import_id`: one source-system import run
- `audit_event_id`: one recorded action

## 5. Record package produced for every item

Each item should produce a portable record package, whether it originated as a link, PDF, screenshot, email attachment, social export, or personal note.

```text
IC-0000001/
├── record.yaml
├── record.csv
├── record.txt
├── record.md
├── source/
│   ├── original input or attachment
│   └── source manifest
├── representations/
│   ├── webpage.html
│   ├── readable-text.txt
│   ├── capture.pdf
│   ├── screenshot.png
│   ├── extracted-text.txt
│   ├── ocr.txt
│   └── transcript.txt
├── versions/
│   └── versions.csv
├── notes/
│   ├── user-notes.md
│   ├── context.md
│   └── open-questions.md
├── provenance/
│   ├── source.json
│   ├── relationships.csv
│   └── generated-fields.json
└── logs/
    ├── intake.jsonl
    └── processing.jsonl
```

Not every file will exist for every item. The record must state what was attempted, what exists, and what failed.

## 6. Canonical record fields

The canonical structured record should include at least:

```yaml
record_id: IC-0000001
record_type: webpage | pdf | image | video | audio | note | message | query | collection | other
title: Human-corrected title
description: Human-authored description
source_url: https://example.com/original
final_url: https://example.com/final
file_type: html | pdf | png | jpg | mp4 | mp3 | txt | json | other
mime_type: text/html
tags: []
lists: []
subjects: []
projects: []
status: inbox | preserved | needs-review | complete | failed | private
visibility: private | restricted | shared | public
source_kind: direct-url | email | browser | social-export | local-file | screenshot | message | unknown
source_service: chrome | safari | pocket | tumblr | facebook | instagram | twitter-x | evernote | notion | apple-notes | icloud | other
submitted_by: Krystal South
sent_by: null
received_at: 2026-09-15T00:00:00+10:00
captured_at: null
original_filename: null
content_sha256: null
record_created_at: 2026-09-15T00:00:00+10:00
record_updated_at: 2026-09-15T00:00:00+10:00
preservation_status: pending
search_status: pending
user_notes: null
generated_summary: null
generated_tags: []
uncertainty: []
open_questions: []
related_records: []
```

`description`, `user_notes`, `generated_summary`, source text, and later interpretation are separate fields. A generated field must include its model/tool and generation timestamp in `generated-fields.json`.

## 7. Plaintext and CSV outputs

### Plaintext record

`record.txt` is a durable human-readable summary for grep, terminal search, offline reading, and future migration. It should contain the record ID, title, all URLs, source details, tags, lists, status, provenance summary, representation inventory, user notes, and generated fields.

### CSV record

`record.csv` is a one-row export for spreadsheet analysis. It should flatten stable scalar fields and use a documented delimiter for arrays such as tags and lists. Repeating information must not be silently discarded; representations, relationships, and audit events belong in their own CSV files.

Recommended export tables:

- `records.csv`: one row per archive item
- `representations.csv`: one row per stored source/derivative
- `versions.csv`: one row per capture/version
- `relationships.csv`: one row per relationship
- `intake_events.csv`: one row per incoming event
- `audit_events.csv`: one row per action
- `imports.csv`: one row per source-system import run

## 8. Representation and version model

The archive distinguishes an item from its representations and versions.

```text
Archive item: “A webpage about X”
├── source URL record
├── original downloaded HTML
├── readable text extraction
├── PDF capture
├── screenshot capture
├── Internet Archive reference, if any
└── later recapture on a different date
```

`versions.csv` should record:

```text
record_id,version_id,representation_id,kind,captured_at,path,sha256,bytes,mime_type,tool,status,notes
```

No later capture replaces an earlier capture. A changed webpage is evidence of change, not a correction to history.

## 9. Intake and upload method log

Every item must have an intake event, even when the input is incomplete.

```text
intake_id,record_id,received_at,method,source_service,sender,original_name,input_reference, supplied_context,privacy_at_intake,result,error
```

Examples of `method` include `ios-share-sheet`, `email`, `browser-extension`, `ifttt`, `manual-upload`, `pocket-import`, `local-folder-scan`, `api`, and `social-export`.

The intake log records what arrived. The processing log records what the system subsequently attempted. These must not be collapsed into one success/failure flag.

## 10. Import and automation architecture

Imports are connector-specific and resumable. Each connector creates an immutable import run and maps source records into archive items without destroying the source export.

```text
source service/export
       ↓
connector staging area
       ↓
import manifest + mapping decisions
       ↓
deduplication review
       ↓
archive records and representations
       ↓
search index + export tables
```

Potential automated inputs:

- IFTTT or similar rules for Twitter/X favourites, email attachments, feeds, and other supported events
- private email address for the archive assistant
- iOS Shortcut for links, screenshots, and files
- scheduled scans of local PDF and research folders
- scheduled imports from exported social-media data
- browser bookmark exports or synchronisation

Automation should never write directly into the canonical archive without an intake event and source-preserving staging step. Failed or malformed source data should remain in staging for review.

## 11. Social-media and legacy data strategy

Pocket, Tumblr, Facebook, Instagram, Twitter/X, Evernote, Apple Notes, iCloud, Notion, and browser exports are treated as separate source systems. Each gets:

- a source inventory
- an import date
- original export preservation
- a connector version
- a mapping report
- duplicate and unresolved-item report
- record-count reconciliation
- a retryable import status

The unfinished Instagram-comment and long-Twitter-post projects are future connectors. Their current code and data should be preserved separately and not made dependencies of the first deployment.

Notion’s poor export behaviour is itself an archival risk. The system should record attempted export methods and failures rather than treating absent data as evidence that no data existed.

## 12. Search indexes

The search system should maintain separate but related indexes:

- metadata index: IDs, titles, dates, domains, tags, lists, people, file types, visibility
- full-text index: webpage text, PDFs, OCR, transcripts, notes, captions, messages
- relationship index: links between records, projects, sources, people, and versions
- failure index: inaccessible URLs, incomplete imports, failed OCR, missing attachments
- duplicate index: canonical URLs, content hashes, probable matches, and review decisions

Search results must point back to the record ID and the exact representation or field that matched.

## 13. Privacy and publication boundaries

Visibility is a record field and a processing boundary, not merely a front-end preference. Private by default applies to personal messages, medical/legal material, unpublished work, sensitive URLs, and archive-assistant intake.

Public exports must be generated as a deliberate derivative after review of prose, filenames, metadata, links, identities, locations, payment details, and embedded media. The private archive remains the source; publication is never an accidental side effect of indexing.

## 14. Backup and verification

Backups must include:

- original source exports
- application database
- object storage
- record packages
- indexes or reproducible index inputs
- schemas and connector versions
- intake, processing, import, and audit logs

Backup verification must periodically restore a sample or clean instance and confirm record counts, checksums, searchable text, representation links, and provenance relationships.

## 15. Adoption boundary

Karakeep can serve as the first operational front end if it can represent or export the record model sufficiently. Any missing fields or audit tables should be handled by an integration/export layer before considering a bespoke replacement.

The first implementation should prove the record package with a small mixed corpus: a webpage, dead URL, PDF, screenshot, email attachment, social export row, personal note, and one historical art artifact. The architecture is intentionally wider than the first build; the point is to prevent early convenience from becoming permanent data loss.
