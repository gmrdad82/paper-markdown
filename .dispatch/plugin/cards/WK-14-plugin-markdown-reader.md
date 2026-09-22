# WK-14 — Plugin: Markdown reader

Wave: 1 · Run: single · Depends on: none · Complexity: **
Branch: split-wk-14 · Base: main

## What
The desk's markdown renderer becomes the `paper-markdown` plugin; the desk falls back to plain text without it.

## How
1. Plain-text fallback in the desk; `src/desk/markdown.rs` removed.
2. The plugin crate: parser, `render_flat`, `note_line`.
3. Tests per construct; a 1× capture matched against the baseline.

## Guards
- Do not touch: the host and the slots.
- Gate: the project's gate
- Accept: the test suite names every construct of the subset.
