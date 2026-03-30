# Translation Engine — Vedabase Prabhupada Translations

System for translating Srila Prabhupada's books from English to Spanish (Spain) and Portuguese (Brazil), using a 6-step pipeline that combines DeepL's natural fluency with Claude's doctrinal precision.

## Source Material

`vedabase_gold_dia.md` — Complete Vedabase export (all of Prabhupada's books in English). Individual books are extracted from this file by finding `============` separator boundaries around the book name.

## Translation Prompts

| File | Language | Rules |
|------|----------|-------|
| `PROMPT_TRADUCCION_COMPLETO.md` | Spanish (Spain) | 94 rules: glossary, grammar, style, Sanskrit handling, false friends, connector rotation, etc. |
| `PROMPT_TRADUCAO_PORTUGUES.md` | Portuguese (Brazil) | Adapted for PT-BR: subjuntivo futuro, infinitivo pessoal, colocação pronominal, crase, contrações, ter/haver, gerúndio rules |

These prompts are the single source of truth for each language. They cover:
- Prabhupada's voice and rhetorical patterns
- Sanskrit terms (never translate, gender/article assignments, plurals, diacritics normalization)
- Honorifics, "Lord" disambiguation, strong expressions
- Complete theological and narrative glossary
- Grammar rules (passive voice, subjunctive, word order, narrative tenses, pronouns)
- False friends to avoid
- Connector rotation (no repeating the same connector more than twice per purport)
- Formatting and punctuation rules specific to each language

## The 6-Step Pipeline

Each book goes through these steps:

### Step 1 — DeepL Draft
Translate the English text using DeepL (ES-ES or PT-BR). This produces a naturally flowing draft with idiomatic target-language syntax and active voice. The goal here is **naturalness**.

### Step 2 — Claude Vedabase
Re-translate the same English text using Claude with the full translation prompt loaded (all 94 rules). This produces a **doctrinally precise** translation with correct glossary terms, Sanskrit handling, connector rotation, proper subjunctive, and all formatting rules applied.

### Step 3 — 3-Way Matrix
Compare both translations against the original English, scoring each on:

| Criterion | Weight |
|-----------|--------|
| Doctrinal Accuracy | 40% |
| Voice Fidelity (Prabhupada's style) | 30% |
| Natural Flow (target language) | 20% |
| Format (diacritics, punctuation, markdown) | 10% |

Pick the winner per criterion for each paragraph/section.

### Step 4 — Hybrid Final
Merge the best of both: DeepL's natural phrasing + Claude's doctrinal corrections. Target quality: **9.5+/10**. This is where human-like fluency meets philosophical precision.

### Step 5 — Production .md
Write only the final translated text to a clean `.md` file. No scores, no English, no matrix — just the translation. Same paragraph structure as English (1 EN paragraph = 1 ES/PT paragraph).

### Step 6 — HTML Table
Generate a side-by-side HTML table (English | Translation) aligned paragraph by paragraph:
- English column: gray text, 15px (reference)
- Translation column: black text, 16px (for review)
- Chapter/mantra headings highlighted in brown with border
- Sanskrit verses in italic
- Sticky header row

This HTML is copy-pasted into **Google Docs** for collaborative review with other devotees.

## Structural Rule

**Critical**: Each English paragraph must correspond to exactly one translated paragraph. Never merge or split paragraphs. Blank lines must match. This ensures perfect alignment in the side-by-side HTML table.

## Output Files

For each translated book, the pipeline produces:

```
traducciones_vedabase/espanol/[book-name]/
  ├── [book]_en.md          # English source
  ├── [book].md             # Final Spanish translation
  └── [book]_tabla.html     # Side-by-side HTML table

traducciones_vedabase/portugues/[book-name]/
  ├── [book]_en.md          # English source
  ├── [book].md             # Final Portuguese translation
  └── [book]_tabla.html     # Side-by-side HTML table
```

A `.txt` copy is also saved to `~/Desktop/` for sharing via WhatsApp.

## How to Run a Translation

### Using Claude Code skills

**Full book** (recommended for complete books):
```
/traducir-pipeline [book name as it appears in vedabase_gold_dia.md]
```
Example: `/traducir-pipeline Sri Isopanisad`

This extracts the book, splits it into sections, launches parallel translation agents, and produces all output files automatically.

**Single passage**:
```
/traducir [English text or file:path]
```

### Manual workflow

1. Extract the book section from `vedabase_gold_dia.md`
2. Get a DeepL translation (deepl.com, ES-ES or PT-BR target)
3. Open a Claude conversation, load the appropriate `PROMPT_TRADUCCION_*.md`, and translate the same text
4. Compare both drafts paragraph by paragraph (matrix scoring)
5. Produce hybrid final merging the best of each
6. Generate aligned HTML table for Google Docs review

## Quality Tracking

Completed translations are tracked in `traducciones_vedabase/tasks.md` with quality scores (target: 9.5/10).

## Repository Structure

```
translation_engine/              # This repo — prompts and source
  ├── PROMPT_TRADUCCION_COMPLETO.md   # Spanish prompt (94 rules)
  ├── PROMPT_TRADUCAO_PORTUGUES.md    # Portuguese prompt (adapted)
  ├── vedabase_gold_dia copia.md      # Full Vedabase source
  └── README.md

traducciones_vedabase/           # Output repo — translations
  ├── PROMPT_TRADUCCION_COMPLETO.md   # Spanish prompt (copy)
  ├── espanol/                        # Spanish translations by book
  ├── portugues/                      # Portuguese translations by book
  ├── tasks.md                        # Progress tracker
  └── vedabase_gold_dia.md            # Full Vedabase source
```
