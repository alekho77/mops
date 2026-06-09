# ADR-0006: Mobile-only local UX v0.1

## Status: Accepted

## Context

MOPS MVP v0.1 is now defined around the object chain:

```text
ActiveSketchBuffer
  -> Sketch
  -> SemanticSketch
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> KnowledgeArea
```

The first shippable UX needs to be smaller than the broader multi-device MOPS architecture. It should prioritize fast capture, local privacy, and a complete loop from raw sketch to long-term knowledge without requiring accounts, registration, cloud sync, or a desktop application.

## Decision

MOPS UX v0.1 is a mobile-only local application.

General principles:

1. No account.
2. No registration.
3. Only the mobile app is part of v0.1.
4. Storage is local on the device.
5. The app always starts on the new sketch editor.
6. Any user input is autosaved.
7. All destructive actions require confirmation.

The accepted main screens are:

```text
Sketch Editor
  -> Inbox
  -> Outbox
  -> Knowledge Base
  -> Settings
```

Canonical Russian UI screen labels:

| Screen | UI RU |
| --- | --- |
| Sketch Editor | Редактор наброска |
| Inbox | Inbox |
| Outbox | Outbox |
| Knowledge Base | База знаний |
| Settings | Настройки |

Navigation:

| Screen | Available transitions |
| --- | --- |
| Sketch Editor | Inbox, Knowledge Base, Settings |
| Inbox | Sketch Editor, Outbox, Knowledge Base, Settings |
| Outbox | Inbox, Sketch Editor, Knowledge Base, Settings |
| Knowledge Base | Sketch Editor, Inbox, Outbox, Settings |
| Settings | back to previous screen |

## UX Chain

The accepted user-facing chain is:

```text
New sketch
  -> Inbox
  -> Outbox
  -> Draft
  -> KnowledgeItem
  -> KnowledgeArea
```

Canonical Russian user-facing chain:

```text
Новый набросок
  -> Inbox
  -> Outbox
  -> Черновик
  -> Знание
  -> Направление
```

The accepted object chain is:

```text
ActiveSketchBuffer
  -> Sketch
  -> SemanticSketch
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> KnowledgeArea
```

The core UX decision is:

```text
Sketch Editor = fast capture
Inbox = raw list
Outbox = semantic workbench
Knowledge Base = long-term memory
```

Canonical Russian UX decision:

```text
Редактор = быстрый захват
Inbox = сырой список
Outbox = семантический рабочий стол
База знаний = долговременная память
```

## Consequences

Positive:

- The first UX has no onboarding, account, or registration blocker.
- Local-only storage matches the privacy-first boundary.
- The startup screen optimizes for immediate capture.
- Autosave prevents accidental loss of unstructured thoughts.
- Confirmation rules reduce data-loss risk in early versions.

Trade-offs:

- Device sync is not part of UX v0.1.
- Desktop processing is not part of the first user-facing release.
- Heavy semantic processing must either run locally on mobile in a limited form or be deferred until a later architecture phase.
- The first mobile app must keep processing scope small enough for mobile OS and device constraints.

Obligations:

- Persist `ActiveSketchBuffer` separately from Inbox `Sketch` records.
- Do not clear `ActiveSketchBuffer` on app restart or navigation.
- Clear `ActiveSketchBuffer` only after confirmed submission to Inbox or confirmed manual clear.
- Require confirmation for deletion, clearing, sending to Outbox, merging, returning Outbox items to Inbox, saving to the Knowledge Base, and full local reset.
- Keep local storage as the only v0.1 persistence layer.
