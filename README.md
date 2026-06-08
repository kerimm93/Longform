# longform

A single-file HTML app for working through long-form learning sources — books, articles, and videos — in a structured way.

Longform acts as a personal learning studio: sources are broken into sections, processed through a defined workflow (scan → clean → import → summarize → flashcards → read → listen → notes), and exported to Anki via AnkiConnect or as CSV. Everything runs locally in the browser with no backend, no framework, and no build step.

---

## What it does

**Sources & Sections**
Add books, articles, or videos as sources. Define sections via three methods: equal split (by pages, minutes, or percent), TOC import (HITL: photo → ChatGPT prompt → paste JSON back), or manual entry. Each section has eight trackable processing steps you can tick off as you go.

**Processing Steps (per section)**
Gescannt · PDF bereinigt · NLM importiert · Audio-Summary · Flashcards · Gelesen · Angehört · Notizen verarbeitet

**Sessions & HITL Workflow**
Log NotebookLM sessions per source or section. Generate a cleanup prompt, paste the raw output into Claude or ChatGPT, and save the cleaned result back. The session modal lets you select a prompt, preview it, copy it, and paste the output — without switching tabs.

**Prompts**
11 built-in system prompts covering all three learning phases (Exploration → Elaboration → Consolidation), plus PDF-to-clean-text for TTS preparation. Add your own prompts. Copy any prompt directly from an expanded source card with source and section context pre-filled.

**Flashcards & Spaced Repetition**
Create MC and Cloze cards manually, or import a batch via AI-generated JSON. SM-2 review queue with quality ratings 0–5. Flashcard review runs in-tab with multiple-choice UI.

**AnkiConnect Export**
Push cards directly to Anki via AnkiConnect (`localhost:8765`). Deck hierarchy: `Longform::SourceTitle::SectionTitle` (root deck configurable). Uses the correct Memrise note types:

- MC cards → `Memrise (Lτ) Preset [Translation+Listenting | MultipleChoice+Typing] v5.1`
- Cloze cards → `Memrise (Lτ) Cloze Template v5.1`

**CSV Export**
Export Anki-compatible CSV files (UTF-8 BOM, semicolon-separated) for MC and Cloze cards, filtered by source or type.

**ZIP Backup**
Download a structured ZIP containing `manifest.json`, one JSON file per source (with its sections), flat files for sessions/cards/prompts/config, and Anki CSVs.

**GitHub Gist Sync**
Optional cloud sync via GitHub Gist. Push and pull with conflict detection. Token stored in `localStorage` only — never in exported data.

**Projects**
Group sources under NotebookLM notebook contexts. A project maps to one notebook — could be a single book, a book with chapters as separate sources, or a topic cluster.

---

## Setup

No installation required. Open `index.html` in any modern browser.

For AnkiConnect: install the [AnkiConnect add-on](https://ankiweb.net/shared/info/2055492159) in Anki, then add `http://localhost` (or your app's origin) to the allowed origins in AnkiConnect's config:

```json
"webCorsOriginList": ["http://localhost", "null"]
```

For GitHub Gist sync: create a [Personal Access Token](https://github.com/settings/tokens) with `gist` scope and a Gist to use as storage. Enter both in Settings.

---

## Data

All data is stored in `localStorage` under the key `longform_v1`. No data is sent anywhere unless you explicitly use Gist sync or AnkiConnect. Credentials (Gist token) are stored in `localStorage` separately and are never included in JSON exports or ZIP backups.

---

## Architecture

| Aspect | Decision |
|---|---|
| Stack | Single-file HTML, vanilla JavaScript, no framework |
| Persistence | `localStorage` (primary) + GitHub Gist (optional sync) |
| Build | None — open the file directly |
| External scripts | JSZip (CDN, for ZIP export) + Google Fonts |
| State | `var S` object with `save()` / `load()` helpers |
| Styling | CSS variables, dark theme, responsive |

---

## Status

**Current version:** v0.1 (post-Sprint 01)

Sprint 01 delivered: correct AnkiConnect note type mapping, ID-based prompt copying (no more inline JS encoding issues), session modal with prompt selector, batch AI flashcard import, and context-aware prompt panel in source cards.

Planned next: `sectionId` / `sessionId` data model cleanup, configurable AnkiConnect URL, prompt snapshots for sessions.

---

## License

Personal use. No license for redistribution.
