# ADR-0012: Capture-first Send to Inbox with Undo

## Status: Accepted

## Supersedes

This ADR supersedes the v0.1 `Send to Inbox` confirmation rule from:

- ADR-0006: Mobile-only local UX v0.1.
- ADR-0010: Incremental private mobile pre-1.0 release train.

ADR-0006 remains accepted for local-only mobile UX principles. ADR-0010 remains accepted for the private mobile pre-1.0 release train.

## Context

MOPS v0.1 starts directly in Sketch Editor and uses `ActiveSketchBuffer` autosave to optimize for immediate capture.

The previous v0.1 documentation required confirmation before sending `ActiveSketchBuffer` to Inbox. That protects against accidental moves, but it adds friction to the main capture flow. For a capture-first product, saving a sketch should be faster than clearing, deleting, resetting, or batch-modifying data.

`Send to Inbox` is not destructive in the same way as clear/delete/reset. It converts the current buffer into a persisted `Sketch` and clears the editor for the next capture. The user should be able to reverse an accidental send immediately.

## Decision

In v0.1, `Send to Inbox` must be a one-tap explicit action without a confirmation dialog.

After send:

1. The app creates a `Sketch` from `ActiveSketchBuffer`.
2. The app clears `ActiveSketchBuffer`.
3. The app shows an Undo affordance.
4. Undo restores the sent text to `ActiveSketchBuffer` and removes the just-created `Sketch`, unless the sketch has already been edited or another conflicting action has made rollback unsafe.

Confirmation remains required for destructive, reset, and batch operations:

- manual clear of `ActiveSketchBuffer`;
- delete `Sketch`;
- clear Inbox;
- full local reset;
- later destructive or batch operations introduced by future milestones.

Settings must not expose a v0.1 option for confirming `Send to Inbox`. Confirmation settings may cover destructive actions only.

## Consequences

Positive:

- Capture remains fast and interruption-free.
- Accidental sends are recoverable without blocking every normal send.
- Confirmation policy is easier to explain: destructive and batch operations ask first; ordinary capture sends provide Undo.

Trade-offs:

- v0.1 needs a small rollback path for the most recent send.
- Undo behavior must be scoped clearly so it does not become a general history system.

Obligations:

- Update v0.1 UX and architecture docs to replace `Send to Inbox` confirmation with Undo.
- Keep confirmations for clear, delete, reset, and batch operations.
- Do not introduce semantic Outbox, embeddings, or sync as part of this UX correction.
