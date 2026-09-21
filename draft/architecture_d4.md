---
name: internet-church-architecture-agent-layer
title: Internet Church Architecture — Agent and Automation Layer
type: specification
version: 0.6.0
status: draft
description: Agent, automation, intake, audit, and derived-output architecture extending the Internet Church archive design.
tags:
  - internet-church
  - archive-assistant
  - automation
  - imports
  - provenance
  - audit
requires:
  - architecture_d3.md
  - requirements_d2.md
applies_to:
  - software-design
  - personal-archive
  - digital-art-archive
updated: 2026-09-16
---

# Internet Church — Agent and Automation Layer

This document extends `architecture_d3.md`. It does not replace it. The purpose of this layer is to make the archive perform the tedious preservation and documentation work after the user sends something once.

## 1. Core experience

```text
User notices something
       ↓
One tap, email, shortcut, or automated import
       ↓
Original input preserved immediately
       ↓
Agent assigns record number and processes the item
       ↓
Archive captures, extracts, indexes, and suggests organisation
       ↓
User receives a plain-language result report
```

The user should not need to decide the final folder, list, title, or metadata at the moment of capture.

## 2. Agent responsibilities

The archive assistant is a constrained worker, not an autonomous owner of the archive.

It may:

- Receive URLs, attachments, screenshots, copied text, media, search terms, and freeform context.
- Preserve the original email, message, file, or submitted payload.
- Assign the next stable `record_id`.
- Detect likely existing records and propose a link or merge for review.
- Fetch webpage metadata and preservation representations.
- Extract text, OCR images/scans, and transcribe audio where configured.
- Suggest title, description, tags, lists, subjects, source type, and relationships.
- Generate searchable derivatives and export records.
- Import from approved connectors and produce reconciliation reports.
- Answer questions using record IDs, source references, and representation paths.
- Report complete, partial, failed, blocked, and pending work.

It must not:

- Claim an artifact was preserved if capture failed or was incomplete.
- Replace source material with OCR, summaries, or inferred metadata.
- Invent dates, authors, senders, provenance, relationships, or certainty.
- Publish, share, delete, overwrite, or merge records without the required confirmation.
- Treat a search query as an endorsement, belief, diagnosis, or conclusion.

## 3. Agent job pipeline

```text
Job received
 ↓
Validate payload and privacy defaults
 ↓
Write immutable intake event
 ↓
Create or identify archive record
 ↓
Preserve originals and calculate checksums
 ↓
Run capture/extraction/OCR/transcription jobs
 ↓
Generate clearly labelled suggestions
 ↓
Apply only safe automatic changes
 ↓
Index and generate exports
 ↓
Send result report and flag review items
```

Each stage must be retryable and independently logged. A failed later stage must not erase a successful earlier stage.

## 4. Job and audit records

```yaml
job_id: JOB-0000001
record_id: IC-0000001
trigger: email | ios-shortcut | browser | ifttt | importer | user-request
received_at: null
started_at: null
completed_at: null
agent_version: null
model: null
privacy_mode: private
status: queued | running | complete | partial | failed | needs-review
stages:
  - name: preserve-original
    status: pending
    started_at: null
    completed_at: null
    outputs: []
    errors: []
```

The append-only agent action log should contain:

```text
agent_action_id,timestamp,job_id,record_id,agent_version,model,trigger,action,input_reference,output_reference,result,confidence,uncertainty,confirmation_required
```

The final user report should state what arrived, what was saved, what was extracted, what was suggested, what failed, and what needs attention.

## 5. Intake adapters

All adapters feed the same intake contract:

```text
intake_id
received_at
method
source_service
sender
original_filename
input_reference
supplied_context
privacy_at_intake
payload_checksum
```

Initial adapters:

- Private archive email address
- iOS Shortcut from Chrome or Safari
- Desktop browser extension
- IFTTT or webhook event
- Manual upload
- Local-folder watcher for PDFs and research files
- Scheduled import of legacy exports
- User-requested Are.na export import

No adapter writes directly to the final record without first preserving an intake event.

## 6. Automation policy

Automations are source-specific, resumable, and conservative.

```text
source event/export
       ↓
unchanged staging copy
       ↓
import manifest
       ↓
mapping and duplicate report
       ↓
archive records
       ↓
search/index/export jobs
```

Potential scheduled sources include email attachments, Twitter/X favourites, RSS, browser exports, Pocket, Tumblr, Facebook, Instagram, Apple Notes, iCloud, Notion, and local PDF folders.

Each import run records source name, export date, connector version, input checksum, input count, created count, linked count, skipped count, failed count, and unresolved count. The original export is never modified.

The unfinished Instagram-comment and long-Twitter-post projects remain future connectors. They are not first-build dependencies.

## 7. Record outputs

For each `record_id`, the system may generate:

```text
record.yaml       canonical structured metadata
record.md         human-readable record
record.txt        plain-text durable/searchable version
record.csv        one-row tabular projection
representations/  original, HTML, PDF, screenshot, OCR, transcript, media
versions.csv      every capture/representation version
relationships.csv linked records and collections
intake.jsonl      incoming events
processing.jsonl  preservation and extraction stages
generated.json    AI/OCR/transcription provenance
```

Only the source, canonical metadata, user decisions, provenance, and audit history are authoritative. CSV, plaintext, indexes, OCR, summaries, tags, embeddings, and previews are regenerable derivatives with their generation provenance retained.

## 8. Confirmation model

The agent may proceed automatically for reversible, private, additive work. It must pause for:

- Public or shared publication
- Destructive deletion or overwrite
- Merging records with disputed identity
- Changing private/restricted visibility
- Sending sensitive content to a new service or model
- Treating ambiguous interpretation as canonical metadata

The confirmation request should identify the exact record, proposed action, affected files, destination, and reason.

## 9. Search and retrieval contract

Every successful or partial job must enqueue indexing for:

- titles and alternate titles
- URLs and domains
- descriptions and user notes
- webpage/PDF/text/OCR/transcript content
- tags, lists, subjects, people, projects, and dates
- relationships and source services
- failures, uncertainty, and preservation status

Search results must return the `record_id`, matching field, matching representation, visibility, and preservation status.

## 10. Implementation boundary

Karakeep remains the provisional front end and operational archive candidate. The agent layer may initially be a small private worker triggered by email or webhook, with Karakeep API access where appropriate. It should not require a custom autonomous platform before the capture-and-report loop is proven.

The first agent test should process one webpage, one PDF, one screenshot, one email attachment, one personal note, and one imported social record. Success means the original inputs remain recoverable, records are searchable, failures are explicit, and the result report is understandable without inspecting logs.
