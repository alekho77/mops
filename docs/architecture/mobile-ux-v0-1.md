# Mobile UX v0.1

## General Principles

1. No account.
2. No registration.
3. Mobile app only.
4. Local device storage only.
5. The app always starts on the new sketch editor.
6. Any input is autosaved.
7. All destructive actions require confirmation.

## Main Screens

```text
Sketch Editor
  -> Inbox
  -> Outbox
  -> Knowledge Base
  -> Settings
```

Canonical Russian screen labels:

| Screen | UI RU |
| --- | --- |
| Sketch Editor | Редактор наброска |
| Inbox | Inbox |
| Outbox | Outbox |
| Knowledge Base | База знаний |
| Settings | Настройки |

Navigation:

| Screen | Transitions |
| --- | --- |
| Sketch Editor | Inbox, Knowledge Base, Settings |
| Inbox | Sketch Editor, Outbox, Knowledge Base, Settings |
| Outbox | Inbox, Sketch Editor, Knowledge Base, Settings |
| Knowledge Base | Sketch Editor, Inbox, Outbox, Settings |
| Settings | back to previous screen |

## Sketch Editor

The Sketch Editor is the main startup screen.

Purpose:

- fast capture of unstructured thoughts.

State object:

```text
ActiveSketchBuffer
```

`ActiveSketchBuffer` is not a `Sketch` in Inbox until the user explicitly sends it there.

Behavior:

1. Text is saved in the background after every change.
2. App restart opens the same text.
3. Navigation to other screens does not clear the text.
4. Sending to Inbox clears the input field.
5. Manual clear requires confirmation.

UI:

```text
Top:    Новый набросок
Center: Большое текстовое поле
Bottom: [Очистить] [Голос] [В Inbox]
Nav:    [Inbox] [База знаний] [Настройки]
```

Actions:

| Action | Behavior |
| --- | --- |
| Очистить | confirmation -> clear `ActiveSketchBuffer` |
| Голос | record speech -> insert text at current cursor position |
| В Inbox | confirmation -> create `Sketch` -> clear editor buffer |
| Inbox | open sketch list |
| База знаний | open Vector DB view |
| Настройки | open settings |

Sketch title format:

```text
Набросок #N на DD.MM.YY
```

`N` must come from a monotonic local counter, not from current Inbox count, to avoid duplicates after deletion.

## Inbox

Purpose:

- raw list of sketches.

Objects:

```text
Sketch
```

Card UI:

```text
Набросок #42 на 09.06.26
первые 2-3 строки текста
```

Bottom actions:

```text
[Очистить все] [Весь Inbox -> Outbox]
```

Opening a sketch:

- tapping an item opens an editor for an existing `Sketch`;
- editing an Inbox sketch does not modify `ActiveSketchBuffer`.

Editor modes:

```text
New sketch       -> ActiveSketchBuffer
Existing sketch  -> SketchEditor
```

Swipe actions:

| Gesture | Action |
| --- | --- |
| Swipe left | delete / edit |
| Swipe right | send to Outbox |

Deletion requires confirmation.

Sending one sketch to Outbox:

1. Compute embedding.
2. Create `SemanticSketch`.
3. Find nearest Bundles.
4. Show assignment options:

```text
Добавить в связку:
- Связка A
- Связка B
- Создать новую связку
```

5. After confirmation, remove the `Sketch` from Inbox.

Sending the whole Inbox to Outbox:

1. Compute embeddings for all sketches.
2. Show found Bundles.
3. User confirms the full operation.
4. All processed sketches move to Outbox.

## Outbox

Purpose:

- semantic workbench.

Objects:

```text
SemanticSketch
Bundle
Draft
KnowledgeItemCandidate
```

Main view:

```text
point       = SemanticSketch
area        = Bundle
line        = Semantic Link
large point = merged document
```

Supported interactions:

```text
pan
zoom
tap
multi-select
```

Tap on `SemanticSketch`:

```text
[Редактировать]
[Вернуть в Inbox]
[Изменить связку]
[Удалить]
```

Rules:

1. Editing recalculates the embedding.
2. Returning to Inbox deletes the `SemanticSketch`.
3. Changing Bundle requires confirmation.
4. Deletion requires confirmation.

Selecting multiple sketches:

```text
[Объединить в черновик]
```

Result:

```text
Draft
```

The Draft can be opened, edited, and confirmed.

Tap inside a `Bundle` area, but not on a point:

```text
Открыть черновик связки
```

Actions:

```text
[Редактировать]
[Подтвердить merge]
[Отмена]
```

After confirmation:

```text
SemanticSketch[] + Bundle -> KnowledgeItemCandidate
```

The map replaces the Bundle with one merged object.

Important rule:

- a merged document cannot be returned to Inbox;
- it can only be edited, deleted, sent to the Knowledge Base, or have its links changed.

Bottom controls:

```text
[Очистить Outbox]
[Вернуть все в Inbox]
[Смержить все связки]
[В базу знаний]
```

Rules:

| Action | Behavior |
| --- | --- |
| Очистить Outbox | confirmation -> delete all unsaved Outbox objects |
| Вернуть все в Inbox | confirmation -> return only `SemanticSketch`, not documents |
| Смержить все связки | confirmation -> create Drafts for all Bundles |
| В базу знаний | confirmation -> save documents as `KnowledgeItem` |

## Knowledge Base

Purpose:

- long-term knowledge storage.

Objects:

```text
KnowledgeItem
KnowledgeArea
```

Main view:

```text
point = KnowledgeItem
area  = KnowledgeArea
line  = semantic link
```

Supported interactions:

```text
scroll
zoom
tap
semantic search
```

Controls:

```text
[Смысловой поиск]
```

Search flow:

1. User enters a query.
2. Query embedding is computed.
3. The map focuses on nearest KnowledgeItems.
4. Matching `KnowledgeItem` records and related `KnowledgeArea` records are highlighted.

Tap on `KnowledgeItem`:

```text
[Открыть]
[Редактировать]
[Изменить связи]
[Удалить]
```

Deletion requires confirmation.

Tap on `KnowledgeArea`:

```text
[Открыть список знаний]
[Переименовать]
[Изменить состав]
[Удалить направление]
```

Deleting a KnowledgeArea must not automatically delete KnowledgeItems.

## Settings

Minimum v0.1 settings:

```text
Локальное хранилище
Модель embedding
Голосовой ввод
Подтверждения действий
Очистка данных
О приложении
```

Important settings:

| Setting | Meaning |
| --- | --- |
| Подтверждать очистку | always enabled in v0.1 |
| Подтверждать отправку в Outbox | enabled |
| Подтверждать merge | enabled |
| Подтверждать удаление | enabled |
| Очистить все данные | full local reset |

## Final Chains

User-facing chain:

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

Object chain:

```text
ActiveSketchBuffer
  -> Sketch
  -> SemanticSketch
  -> Bundle
  -> Draft
  -> KnowledgeItem
  -> KnowledgeArea
```
