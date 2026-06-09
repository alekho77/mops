# ADR-0013: Lightweight first Outbox and Knowledge Base

## Status: Accepted

## Supersedes

This ADR supersedes the early mobile scope portions of:

- ADR-0005: Sketch, SemanticSketch, and Semantic Workbench MVP.
- ADR-0010: Incremental private mobile pre-1.0 release train.

ADR-0005 remains accepted for canonical terminology and the long-term semantic model. ADR-0010 remains accepted for the private mobile pre-1.0 release train.

## Context

The full Outbox direction can easily become a graph editor: map navigation, tap selection, multi-select, semantic link editing, Bundle areas, merged documents, and bulk merge operations. That is too heavy for the first mobile version where Outbox appears.

The first Knowledge Base can also become overloaded if it ships together with map views, semantic map editing, KnowledgeArea editing, complex delete workflows, and broad semantic workbench controls.

The private mobile train should validate useful user-facing value with narrow list/card workflows before introducing visual graph editing or advanced workspace operations.

## Decision

The first mobile Outbox version must be list/card-first, not map-first.

Required first Outbox surface:

- list of processed `SemanticSketch` records;
- detail view for a processed Sketch;
- embedding/status metadata;
- similar Sketch references;
- suggested Bundle cards.

The first Outbox version must not include:

- graph editor behavior;
- map as the primary Outbox UI;
- pan/zoom/tap/multi-select editing workflows;
- Bundle area editing;
- merged document editing;
- bulk merge operations.

Suggested Bundle cards are suggestions, not durable Bundle truth. They may be accepted, rejected, or deferred only when the relevant milestone supports that action.

The first Knowledge Base version must be list/search/detail-first.

Required first Knowledge Base surface:

- list of `KnowledgeItem` records;
- search;
- open item;
- edit item;
- delete item with confirmation.

The first Knowledge Base version must not include:

- map as the primary Knowledge Base UI;
- KnowledgeArea editing;
- visual graph editing;
- bulk KnowledgeItem operations;
- complex delete propagation across semantic relations.

Semantic Map is an experimental read-only visualization when it first appears. It must not be the primary editor for Outbox, Bundles, KnowledgeItems, or KnowledgeAreas. It must not support create/edit/delete/link/merge operations from the map surface in its first version.

## Consequences

Positive:

- The first Outbox validates semantic processing without shipping a graph editor.
- Suggested Bundle cards make semantic grouping visible without requiring full Bundle operations.
- The first Knowledge Base is usable as a normal personal knowledge list before adding area management or visual maps.
- Semantic Map can be explored without becoming a blocker for core list/search workflows.

Trade-offs:

- Advanced visual semantic editing is deferred.
- Some Bundle and KnowledgeArea operations need later dedicated list/card workflows before any map-based editing is considered.

Obligations:

- Keep early Outbox, Bundle, and Knowledge Base mobile UI list/card-first.
- Treat initial Semantic Map work as read-only experimental visualization.
- Keep destructive and bulk operations explicit, narrow, and confirmed.
