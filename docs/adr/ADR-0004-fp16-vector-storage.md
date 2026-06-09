# ADR-0004: Use FP16 for vector DB storage

## Status: Accepted

## Context

MOPS keeps a full local semantic DB on each personal device. The vector DB stores ready embeddings and search metadata, while raw source files remain outside the semantic/vector DB by default.

FP32 vectors are common for model computation, but they use twice as much persisted storage as FP16.

## Decision

MOPS will use FP16 as the default persisted vector format in the vector DB.

Embedding pipelines may compute vectors in FP32 internally. Before persistence, vectors must be converted to FP16 unless a future ADR changes the storage format.

Every vector record must store:

```text
model_id
model_version
dimensions
storage_dtype
distance_metric
normalization
created_at
```

MVP sizing target:

```text
100 semantic chunks/day
384-dimensional embeddings
FP16 stored vectors
```

Five-year raw vector estimate:

```text
100 chunks/day x 365 x 5 = 182,500 chunks
182,500 x 384 x 2 bytes = 140,160,000 bytes approx 140 MB
```

## Consequences

Positive:

- Raw vector storage is reduced by 50 percent compared with FP32.
- The semantic DB budget is more suitable for local-first mobile and desktop use.
- FP32 can still be used inside model/runtime computation where needed.

Negative:

- Persistence requires an explicit conversion step from FP32-producing models.
- Vector serialization must preserve dtype metadata.
- Tests must cover FP16 round-trip, normalization, and distance behavior.

Obligations:

- Store `storage_dtype` for every vector record.
- Do not mix vectors with incompatible model, dimension, metric, normalization, or dtype assumptions in one searchable space.
- Keep vector indexes rebuildable from FP16 persisted records.
