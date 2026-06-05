# MOPS

**MOPS** — персональный local-first ИИ-слой над цифровой жизнью пользователя.

Это индивидуальный продукт для одного человека и его личных устройств. Не командная платформа, не корпоративная база знаний и не облачный сервис как главный источник истины.

Главная идея: создать личного «мопса» — персонального ИИ-агента, который постепенно накапливает, структурирует и переиспользует смысловую память пользователя.

## Цель проекта

MOPS помогает пользователю:

- быстро фиксировать мысли, заметки и голосовые фрагменты;
- искать по личному контексту семантически, а не только по ключевым словам;
- связывать заметки, документы, проекты, ссылки и идеи;
- восстанавливать контекст прошлых решений;
- автоматически выделять проекты, темы, задачи и смысловые кластеры;
- работать с собственной памятью локально и приватно.

## Базовый принцип

MOPS не хранит полные документы внутри векторной базы.

В semantic DB хранятся только:

- короткие смысловые фрагменты;
- summary;
- embeddings;
- metadata;
- entities;
- semantic tags;
- связи между фрагментами;
- ссылки на исходные документы;
- device/path references;
- fingerprints и hashes для восстановления источников.

Исходные файлы остаются там, где они уже лежат: на телефоне, ноутбуке, десктопе, внешнем диске или в облачной папке пользователя.

## Архитектура

```text
Device A
  Local SQLite semantic DB
  Local vector index
  Local files
  Local source resolver

Device B
  Full replicated semantic DB
  Own local vector index
  Own local files
  Own source resolver

Cloud Drive
  Encrypted changelog
  Encrypted snapshots
  Device manifests
  Backups
```

## Local-first модель

Каждое устройство хранит полную копию semantic DB.

Облако используется не как главный backend, а как зашифрованный транспорт синхронизации и резервных копий.

Подходящие варианты:

- Google Drive;
- OneDrive;
- Dropbox;
- iCloud Drive;
- WebDAV/S3-compatible storage в будущем.

## Что синхронизируется

- semantic chunks;
- summaries;
- embeddings;
- document metadata;
- source fingerprints;
- paths and device references;
- entities;
- relations;
- model metadata;
- encrypted snapshots;
- append-only changelog.

## Что не синхронизируется как source of truth

- live `db.sqlite`;
- SQLite WAL files;
- ANN/HNSW/Faiss indexes;
- temporary caches;
- raw documents by default;
- photos, videos, audio and large binary files by default.

Vector index считается производным локальным кэшем. Его можно пересобрать на каждом устройстве из semantic DB.

## Source Resolver

Отдельный слой отвечает за восстановление ссылок на исходные документы.

Он отслеживает:

- `document_id`;
- `device_id`;
- `path`;
- `content_hash`;
- `partial_hash`;
- `file_size`;
- `modified_at`;
- `last_seen_at`;
- `availability`.

Если файл переехал, был переименован или временно недоступен, semantic memory не ломается. Ломается только доступ к первоисточнику, который можно восстановить позднее.

## MVP

Первый реалистичный MVP:

1. Mobile-first приложение.
2. Local SQLite semantic DB.
3. Локальный vector search.
4. Ручные заметки, голосовые заметки, ссылки и выбранные документы.
5. Короткие semantic chunks вместо полного хранения документов.
6. Зашифрованная синхронизация через личную cloud folder.
7. Source Resolver для проверки и восстановления ссылок.
8. Простая semantic cleanup-механика.

## Почему mobile-first

Телефон — естественный центр MOPS:

- всегда рядом;
- содержит голос, камеру, заметки, ссылки, файлы и уведомления;
- подходит для быстрой фиксации мыслей;
- становится постоянным интерфейсом к личной памяти.

Десктоп и домашний сервер могут использоваться для тяжёлой индексации, batch processing и работы с большими архивами.

## Хранение данных

Ориентир для MVP:

```text
100 semantic chunks/day
384-dimensional embeddings
FP32 or INT8 vectors
short text + summary + metadata
```

Примерная оценка на 5 лет:

```text
100 chunks/day × 365 × 5 = 182,500 chunks

384 FP32 embeddings ≈ 280 MB raw vectors
384 INT8 embeddings ≈ 70 MB raw vectors

Full semantic DB with metadata ≈ hundreds of MB to ~1–2 GB
```

Это делает возможным многолетнюю персональную semantic memory даже в рамках небольшого личного облачного хранилища.

## Важные ограничения

MOPS не должен превращаться в бездумный индексатор всего подряд.

Главные риски:

- слишком много низкокачественных chunks;
- дубликаты;
- устаревшие связи;
- несовместимые embedding models;
- сломанные пути к файлам;
- переиндексация после смены модели;
- ограничения мобильных ОС на фоновые задачи;
- рост changelog без compact/snapshot механики.

Поэтому MOPS должен быть не пылесосом данных, а фильтром личного смысла.

## Принципы дизайна

- Individual-first.
- Local-first.
- Mobile-first.
- Offline-first.
- Privacy-first.
- Cloud as transport, not master.
- Semantic DB as source of truth.
- Vector index as rebuildable cache.
- Raw files outside vector DB.
- Short meaning fragments instead of full document copies.
- Explicit model versioning.
- Encrypted sync from day one.

## Нецели

MOPS не предназначен для:

- командной работы;
- корпоративной базы знаний;
- совместного редактирования;
- realtime collaboration;
- общих рабочих пространств;
- сложных ACL;
- хранения всех пользовательских файлов внутри своей базы;
- обязательного облачного backend как master-сервера.

## Статус

Проект находится на стадии проектирования архитектуры и MVP.

Первый фокус: компактная, приватная, синхронизируемая semantic memory для одного пользователя и его личных устройств.
