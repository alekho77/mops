# Semantic Vector Boundary

## Purpose

MOPS separates semantic vector production from vector storage and search.

## Layers

```text
Source Data
  -> Parser / Chunker
  -> Model Layer
  -> Vector DB Layer
  -> Context Builder
```

## Model Layer

Responsible for producing vectors.

Responsibilities:

- convert text, notes, chunks, code, or future media inputs into vectors;
- support external embedding models, local models, custom linguistic vectors, and future multimodal models;
- expose model id, model version, vector dimension, metric expectation, and embedding timestamp;
- provide batch embedding;
- reject unsupported input types.

Non-responsibilities:

- durable vector storage;
- ANN indexing;
- source file resolution;
- final context assembly.

## Vector DB Layer

Responsible for storing and searching ready vectors.

Responsibilities:

- upsert vectors with ids and metadata;
- maintain local or remote vector indexes;
- execute nearest-neighbor search;
- return ranked candidates with ids, scores, and metadata;
- delete vectors by id or source scope;
- allow index rebuild from the semantic DB.

Non-responsibilities:

- creating embeddings;
- deciding chunking strategy;
- interpreting semantic meaning;
- storing raw source documents as the primary source of truth.

## MOPS Semantic Engine

Responsible for orchestration.

Responsibilities:

- parse source data;
- create semantic chunks;
- choose an embedding model;
- call the vector store;
- maintain source references;
- reconstruct context from vector search results;
- validate model/vector compatibility.

## Required metadata per vector

- vector id;
- source id;
- chunk id;
- model id;
- model version;
- vector dimension;
- distance metric;
- content hash;
- created timestamp;
- source reference;
- domain tags;
- user/project scope when available.

## Search flow

```text
User query
  -> query embedding
  -> vector search top-k
  -> chunk ids / source refs
  -> semantic DB lookup
  -> context assembly
```

## Storage rule

Vector DB is not the source of truth.

The durable semantic DB stores source-linked memory records and enough metadata to rebuild vector indexes.
