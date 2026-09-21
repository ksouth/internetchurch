---
name: internet-church-architecture
title: Internet Church Architecture
type: specification
version: 0.3.0
status: draft
description: Broad architecture for a searchable, provenance-aware personal web and research archive.
tags:
  - internet-church
  - architecture
  - personal-archive
  - digital-art-archive
  - provenance
  - preservation
requires:
  - requirements_d2.md
applies_to:
  - software-design
  - personal-archive
  - digital-art-archive
updated: 2026-09-15
---

# Internet Church — Architecture

## 1. Architectural position

Internet Church should be a searchable external memory and archive of networked life, not merely a bookmark database. It needs a pleasant capture and sorting surface, a serious preservation layer, and an auditable record of how each object entered and changed within the archive.

The provisional platform leader is **Karakeep** because its current feature set aligns with mixed media, lists, OCR, full-text and semantic search, AI-assisted metadata, mobile capture, imports, and full-page archiving. This is an adoption hypothesis, not an implementation claim; running versions, exports, failure modes, and privacy behaviour must be tested.

ArchiveBox remains a preservation specialist to evaluate alongside Karakeep. Linkwarden remains a comparison candidate. Gitmark remains a lightweight capture prototype. `ksouth/internetchurch` is the project/archive identity and should not be confused with application source code.

## 2. Historical design context

The project has precedents in the user’s existing work, including `Searched Terms`, `Identify Yourself`, `A Mirror Unto Itself`, `QR/ART`, social-media research, website preservation, and the long-term archive of an online self. These works establish that chronology, search, juxtaposition, identity, interface, networked memory, and the unstable relationship between source and interpretation are functional design requirements.

The archive should be able to show both a precise search result and a meaningful collision between unrelated objects. It must not reduce the practice to generic folders.

## 3. System shape

```text
Phone Share Sheet / email / browser / uploads / imports / local files
                             |
                             v
                    Immutable intake event
                             |
                             v
                    Capture inbox + assistant
                             |
                             v
              Preservation and extraction workers
        URL | HTML | PDF | image | OCR | audio | video | text
                             |
                             v
              Artifact record + relationship graph
                             |
                             v
            Search index + lists + visual/temporal UI
                             |
                             v
       Export bundles + manifests + Git history + independent backup
```

## 4. Core data model

```text
ArchiveObject
  id
  object_type
  created_at
  title / alternate_titles
  source_context
  user_context
  generated_context
  dates / locations
  people / authors / organisations
  tags / subjects / collections / projects
  status / significance / review_state
  uncertainty / open_questions

CaptureEvent
  id
  object_id
  input_kind
  original_input_reference
  received_at
  sender_or_source
  capture_method
  processing_status
  failure_reason

Representation
  id
  object_id
  kind: original | html | pdf | screenshot | text | ocr | transcript | media | external_reference
  path_or_uri
  captured_at
  sha256
  byte_size
  mime_type
  source_tool_and_version
  status

Relationship
  subject_id
  predicate
  object_id
  source_or_basis
  confidence
  created_at

AuditEvent
  timestamp
  actor: user | assistant | importer | preservation_tool
  action
  input_reference
  output_reference
  tool_or_model
  result
  uncertainty
```

The three archive layers remain distinct but cross-linkable:

- Documentary: source files, captures, records, transcripts, and preserved representations.
- Personal: memory, context, people, places, life events, and why something matters.
- Intellectual: research, writing, analysis, art projects, interpretation, and AI collaboration.

## 5. Preservation strategy

Karakeep should be tested as the primary operational application. Preservation workers may be supplemented by ArchiveBox or other tools where Karakeep’s capture is incomplete. A failed or partial representation is still recorded as evidence of the attempt.

The original input and every derivative must remain separately addressable. A later recapture creates a new representation or capture event; it does not silently replace the old one.

## 6. Search and front-end strategy

The search index should cover source text, extracted text, OCR, transcripts, notes, annotations, tags, relationships, and generated summaries. Results should identify the matching field and representation.

The interface should provide:

- an inbox for rapid capture
- one-tap or keyboard-assisted list/tag assignment
- overlapping lists rather than rigid folders
- chronological and “Searched Terms” views
- visual browsing for image/video/web-art objects
- relationship and provenance views for documentary work
- bulk review and reversible metadata edits
- explicit capture-status and uncertainty indicators

## 7. Assistant architecture

Start with a private email intake because it naturally accepts URLs, attachments, screenshots, freeform notes, and a durable original message.

```text
Private message
     ↓
Immutable intake record
     ↓
Preserve attachments and linked sources
     ↓
Extract text and metadata
     ↓
Suggest tags, lists, links, and summary
     ↓
Index and send an auditable report
```

The assistant is a constrained worker. It may organise and suggest, but cannot silently publish, delete, overwrite, or collapse source, memory, interpretation, and inference into one voice.

## 8. Storage and Git

- Application database: operational records, lists, relationships, audit events, and search configuration.
- Object storage: original PDFs, images, audio, video, HTML, screenshots, and other large files.
- Export bundle: one portable record per object plus representations and checksum manifest.
- Git repository: versioned metadata, notes, manifests, schemas, decision logs, and smaller text derivatives.
- Independent backup: separate protection for application data and large binary objects.

Git is valuable for provenance and intellectual history, but the live database should not be forced into one README or a fragile direct phone-side read-modify-write workflow.

## 9. Candidate evaluation

Test Karakeep, ArchiveBox, and Linkwarden with the same corpus:

- iPhone Chrome capture
- PDFs, scans, screenshots, images, audio, and video
- complex and dead webpages
- duplicate and recaptured URLs
- Pocket/browser exports and representative historical files
- OCR and full-text retrieval
- list assignment and later reorganisation
- export, checksum verification, and clean-instance restore
- privacy controls and local-AI configuration
- assistant intake and audit reporting

## 10. Explicit non-goals for first implementation

Do not begin by finishing the Instagram-comment or long-Twitter-post tools, rebuilding every historical importer, or creating a bespoke archive engine. Preserve their existence as future ingestion paths and first prove that the Karakeep-centred architecture can capture, document, search, sort, export, and restore representative Internet Church objects.

## 11. Decision gate

Adopt Karakeep as the operational front end only if it passes the acceptance corpus and produces exports with enough provenance to remain useful outside the application. Add ArchiveBox-style preservation where needed. Build custom components only for demonstrated gaps in capture, documentation, sorting, search, auditability, or export.
