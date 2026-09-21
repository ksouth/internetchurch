---
name: internet-church-architecture
title: Internet Church Architecture
type: specification
version: 0.2.0
status: draft
description: Proposed architecture for the Internet Church personal web and research archive.
tags:
  - internet-church
  - architecture
  - personal-archive
  - provenance
  - preservation
requires:
  - requirements.md
applies_to:
  - software-design
  - personal-archive
updated: 2026-09-15
---

# Internet Church — Architecture

Status: Draft 0.1  
Date: 2026-09-15

## 1. Recommended direction

Use a self-hosted archive application as the capture, preservation, and search layer. Treat Git as a durable export/version-history layer rather than forcing the live application database to be Git itself. Add a narrowly scoped archive assistant as an intake and organisation layer.

Recommended starting candidate: **Karakeep**, because it best matches the complete workflow: mixed links, notes, images, PDFs, lists, OCR, full-text and semantic search, local AI, mobile capture, broad imports, and full-page archiving. The priority is complete artifact documentation and frictionless later sorting; external archive submission is not a core requirement.

This is a draft design, not an implementation claim. Candidate capabilities must be verified against the running versions and their exports before adoption.

Keep **Linkwarden** as the strongest preservation-focused comparison, and test **ArchiveBox** as a preservation specialist rather than assuming the main application must do every archival job itself.

## 2. Logical components

```text
iPhone Share Sheet / email / browser extension / uploads / imports
                         |
                         v
               Capture inbox + assistant
                         |
                         v
               Metadata + preservation workers
               |       |        |       |
               URL   HTML     PDF   screenshot/text/OCR
                         |
                         v
                  Search index, UI, and audit log
                         |
                         v
        exports + checksums + backups + optional Git mirror
```

## 3. Data model

Each archive item has a stable identifier and may have multiple representations.

The archive keeps three related but distinct layers: documentary material (source files and records), personal context (memory and why an item matters), and intellectual work (research, interpretation, writing, and AI collaboration).

```text
ArchiveItem
  id
  created_at
  source_url
  final_url
  title
  description
  user_notes
  tags[]
  collections[]
  authors[]
  subjects[]
  source_type
  status
  capture_status
  preservation_status
  provenance

Representation
  item_id
  kind: original | html | pdf | screenshot | text | ocr | external_archive
  path_or_uri
  captured_at
  sha256
  byte_size
  source_tool
  status

Provenance includes the original input, capture method, capture time, source and final URLs, processing history, and links to every preserved representation. Generated summaries, tags, OCR, and classifications remain separate from original and user-authored fields.
```

A later capture of the same page should create a new Representation or CaptureEvent, not erase the earlier one.

## 4. Storage and backup

- Live application database: operational metadata, users, tags, collections, artifact descriptions, capture events, and search configuration.
- Application storage: preserved PDFs, HTML, screenshots, images, and extracted text.
- Export bundle: portable records plus original files and a manifest containing checksums.
- Git repository: versioned metadata, notes, manifests, schemas, configuration, and optionally smaller text representations.
- Separate object backup: required for large binary preservation; GitHub alone should not be assumed to be the only backup.

Git should record the archive’s intellectual and editorial history. It should not become a dumping ground for every generated screenshot or large PDF unless repository size and licensing/privacy have been deliberately assessed.

## 5. Git integration

The existing Gitmark fork remains useful as a prototype for direct GitHub capture, but it currently appends links into one Markdown file and stores a GitHub token in browser sync storage. It is not yet the right canonical archive layer.

Possible later integration:

1. Gitmark or an iOS Shortcut sends a URL to the archive API.
2. The archive preserves and indexes it.
3. A scheduled export produces one fully documented record per item plus a manifest.
4. The export is committed to a private Git repository.

This gives fast capture, robust preservation, searchable application data, and inspectable Git history without making the phone perform a fragile read-modify-write against one README.

## 6. Archive assistant

The assistant should operate as a constrained worker, not an autonomous editor of the archive. Email is a particularly good first interface because it supports links, attachments, screenshots, freeform notes, and a durable message record.

```text
Email or message → private intake → immutable inbox event
                 → preserve source → suggest metadata → report result
```

The assistant should preserve the original message, attachment checksums, sender, received time, URLs, and processing log. It can make suggestions automatically, but destructive actions, public sharing, privacy-sensitive classification, and uncertain source interpretation should require confirmation.

The audit record should answer: what did I send, what did the assistant fetch, what did it save, which tools/models were used, what changed, what failed, and where are the resulting files?

## 7. Candidate fit

| Requirement | Karakeep | Linkwarden |
|---|---|---|
| iOS capture | Native iOS app and Safari extension | Native apps and documented iOS Shortcut |
| Mixed links, notes, images, PDFs | Strong | Strong, though more link-centred |
| Full-page preservation | Yes, using Monolith | Yes; screenshot, PDF, HTML |
| Full-text search | Full-text and semantic | Full-text search, filters, sorting |
| OCR | Yes | Not the primary headline feature |
| Local AI | Ollama support | Local AI tagging option |
| Annotations/highlights | Highlights | Reader view, highlights, annotations |
| Historical imports | Chrome, Pocket, Linkwarden, Omnivore, tab sessions | SingleFile and other import routes; verify each legacy source |
| Git-native storage | No | No |
| Preservation/documentary emphasis | Strong, but broader archive app | Strongest conceptual fit |
| Operational complexity | High | High |

## 8. Decision gates before adoption

Before committing to either system, test:

- iPhone Chrome Share Sheet capture.
- A dead or redirected URL.
- A complex JavaScript-heavy page.
- A PDF upload and text search inside it.
- A screenshot/HTML/PDF export and restore.
- Duplicate URL plus later recapture.
- Pocket/Evernote export import.
- Backup restoration on a clean instance.
- Whether private data can be kept completely outside public Git.
- Whether the API can support a future custom capture client.

## 9. Provisional decision

Deploy a private Karakeep instance for a small acceptance test, while preserving the existing Gitmark fork untouched as a separate prototype. Test ArchiveBox alongside it for deeper preservation and compare Linkwarden where its preservation workflow offers a meaningful advantage. Only then decide whether custom development or integration is justified.
