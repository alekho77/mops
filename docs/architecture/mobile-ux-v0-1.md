# Mobile UX v0.1

## Scope

Mobile UX v0.1 is the first private installable local phone build. It is limited to capture and Inbox management.

Required object chain:

```text
ActiveSketchBuffer
  -> Sketch
```

Excluded from v0.1:

- Outbox;
- voice capture;
- embeddings;
- cosine similarity;
- `SemanticSketch`;
- semantic links;
- graph persistence;
- Bundles;
- Drafts;
- KnowledgeItems;
- KnowledgeAreas;
- semantic search;
- 2D Semantic Map.

## General Principles

1. No account.
2. No registration.
3. Mobile app only.
4. Local device storage only.
5. The app always starts on the new Sketch Editor.
6. Any input is autosaved.
7. Send to Inbox is one-tap and recoverable with Undo.
8. Destructive, reset, and batch capture/Inbox actions require confirmation.
9. The private build must be buildable as Android APK or iOS/Xcode build for phone testing.

## Main Screens

```text
Sketch Editor
  -> Inbox
  -> Settings
```

Navigation:

| Screen | Transitions |
| --- | --- |
| Sketch Editor | Inbox, Settings |
| Inbox | Sketch Editor, Settings |
| Settings | back to previous screen |

## Sketch Editor

The Sketch Editor is the startup screen.

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
4. Sending to Inbox is a one-tap explicit action without confirmation.
5. Sending to Inbox creates a `Sketch` and clears the input field.
6. After sending to Inbox, the app shows Undo.
7. Undo restores the sent text to `ActiveSketchBuffer` and removes the just-created `Sketch`, unless rollback is unsafe because of a later conflicting edit/action.
8. Manual clear requires confirmation.

Minimum UI:

```text
Top:    New sketch
Center: Large text field
Bottom: [Clear] [Send to Inbox]
Nav:    [Inbox] [Settings]
```

Actions:

| Action | Behavior |
| --- | --- |
| Clear | confirmation -> clear `ActiveSketchBuffer` |
| Send to Inbox | create `Sketch` -> clear editor buffer -> show Undo |
| Inbox | open sketch list |
| Settings | open settings |

Sketch title format:

```text
Sketch #N on DD.MM.YY
```

`N` must come from a monotonic local counter, not from current Inbox count, to avoid duplicates after deletion.

## Inbox

Purpose:

- raw list of captured sketches.

Objects:

```text
Sketch
```

Card UI:

```text
Sketch #42 on 09.06.26
first 2-3 lines of text
status
```

Opening a sketch:

- tapping an item opens an editor for an existing `Sketch`;
- editing an Inbox sketch does not modify `ActiveSketchBuffer`.

Editor modes:

```text
New sketch       -> ActiveSketchBuffer
Existing sketch  -> SketchEditor
```

Required actions:

| Action | Behavior |
| --- | --- |
| Create from Sketch Editor | persist `Sketch` -> clear `ActiveSketchBuffer` -> show Undo |
| Open | show full sketch text |
| Edit | update existing `Sketch` |
| Delete | confirmation -> delete `Sketch` |
| Clear all | confirmation -> delete all Inbox sketches |

v0.1 does not support sending sketches to Outbox. That action appears in v0.3 after v0.2 Mobile Voice Capture.

## Settings

Minimum v0.1 settings:

```text
Local storage
Action confirmations
Data reset
About
```

Important settings:

| Setting | Meaning |
| --- | --- |
| Confirm clear | always enabled in v0.1 |
| Confirm deletion | enabled |
| Clear all data | confirmation -> full local reset |

Settings must not expose embedding model, voice input, semantic search, sync, or Knowledge Base controls in v0.1.

## Phone Acceptance Scenario

The v0.1 private build is acceptable when this scenario passes on a real Android or iOS device:

1. Install the app from an APK or Xcode/iOS build.
2. Open the app and land on Sketch Editor without login or registration.
3. Type text into the editor.
4. Restart the app and verify the text is restored from `ActiveSketchBuffer`.
5. Tap Send to Inbox without a confirmation dialog and verify a `Sketch` appears in Inbox.
6. Use Undo after send and verify the text returns to `ActiveSketchBuffer` and the just-created `Sketch` is removed.
7. Send again, edit the `Sketch`, restart, and verify the edit persists.
8. Delete the `Sketch` only after confirmation.
9. Clear all local data only after confirmation.

## Final v0.1 Chain

User-facing chain:

```text
New sketch
  -> Inbox
```

Object chain:

```text
ActiveSketchBuffer
  -> Sketch
```
