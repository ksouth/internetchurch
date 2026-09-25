# internetchurch

Repo for the link collecting thing I'm trying to build, a deposit of web artefacts I hope to recall ongoing.

## Why this exists

My browser has become external working memory. Open tabs are not just a reading list: they are evidence, sources, papers, people, government pages, GitHub repos, things to monitor, active projects, rabbit holes, and things I am afraid I will never find again if I close them.

The problem is not simply "too many tabs" or "too much research." The problem is that I need a reliable capture and retrieval system that matches the way I research across many subjects at once.

internetchurch should make it safe to close a tab.

## Core architecture

Use **Karakeep as the storage/retrieval and archival engine**, rather than rebuilding bookmarking, page preservation, full-text search, highlights, attachments, and ingestion from scratch.

internetchurch should be the layer on top that makes the corpus useful to me: classification, relationships, project context, monitoring, retrieval, and eventually synthesis.

Feed/RSS tools such as Feedly can remain inputs if useful, but they should not be the canonical library. The central problem is preserving and retrieving heterogeneous things encountered during research, not only following new posts from known sources.

## Information model

Do not force items into one folder hierarchy. One item can belong to several contexts at once.

Useful facets include:

- **subject:** NDIS, AI, biodiversity, accessibility, etc.
- **project:** MyNDIS, RSF, internetchurch, none, etc.
- **type:** article, paper, government page, repository, person, dataset, thread, etc.
- **state:** inbox, reading, useful, monitoring, archived.
- **reason:** evidence, reference, investigate, inspiration, citation.

These are facets, not mandatory fields that must be completed before saving.

### Capture principle

**Capture first; classify later.**

Saving a link should require as little thought and interaction as possible. If capture requires deciding where something belongs, filling out a form, or maintaining a taxonomy, the system has failed.

The basic interaction should be:

> Find something → save to internetchurch → close the tab.

The system should preserve the page and its metadata, infer likely subjects/projects, identify duplicates and relationships where possible, and allow later correction without blocking capture.

## Retrieval goal

The system should support retrieval by remembered meaning, not just remembered title, folder, or URL.

Examples:

- "Find the thing I saw about AI systems being introduced into old industrial infrastructure."
- "Show me everything I've collected connecting disability assessment with AI or automated decision-making."
- "What was I researching about insect taxonomy three weeks ago?"

The user should not have to remember where an item was filed.

## Initial scope

Do **not** begin by building the entire knowledge-management system.

Phase 1 is deliberately small:

**Capture → preserve → automatically classify → search → reopen.**

Make those five things reliable before adding:

- relationship graphs
- research trails
- monitoring/change detection
- automated synthesis
- AI querying
- visual maps
- other higher-order knowledge features

Reliability and recoverability come before cleverness.

## Design constraint: real research, not an abstract PKM system

On 25 September 2026, a Chrome inventory captured roughly 1,000 open research tabs across 25 windows. They span NDIS/disability policy, Australian government material, AI governance and safety, academic papers, software/GitHub work, biodiversity and taxonomy, health, accessibility, journalism, social media, and other active research.

That inventory is a useful real-world test corpus for internetchurch. Architecture and UX decisions should be tested against whether the system could ingest those links, preserve their context, make them retrievable, and allow the browser tabs to be safely closed.

## Success criterion

internetchurch succeeds when I can close a research tab without anxiety because I trust that:

1. the source has been preserved;
2. I can find it again without remembering exactly what it was called;
3. I can see why I saved it and what it relates to;
4. the system does not require constant manual organization to remain useful.
