# WK-14 — Plugin: Markdown reader

Branch: split-wk-14 (in both repos) · Worktree: ~/Projects/.worktrees/paper-markdown/split-wk-14 and ~/Projects/.worktrees/pito-work/split-wk-14

## What
The book (section 6): the Markdown reader is the first first-party plugin; "today's `desk/markdown.rs` and the reader compositions become the plugin; the desk keeps plain text".

## How
1. **The fallback in pito-work.** In `src/desk/page.rs` (`:258`, `:277`, `:485`, `:601`) and `src/desk/bodyeditor.rs:137`, replace the markdown calls by the slot call WK-13 introduced, with the role of each place (`page_body`, `reference_excerpt`, `reference_excerpt`, `thought_body`, `page_body`); the plain-text fallback keeps line breaks, wraps, and uses `theme::Type::Body` (`src/desk/theme.rs`).
2. **The plugin crate.** `pito-plugins/plugins/markdown-reader/` from the template: `Cargo.toml` (crate type `cdylib`, target `wasm32-wasip2`, the `pito:host` and `pito:work` bindings as the template imports them), `plugin.toml` (id `markdown-reader`, name "Markdown reader", version `0.1.0`, host `work`, capabilities: none beyond the slot — it reads only what the host hands it, so `core:read` is not requested), and …
3. **The parser.** Port the subset from pito-ui's `work/src/markdown.rs` (read it at `~/Projects/pito-ui/work/src/markdown.rs`, or under `pito-work/src/ui/` after WK-03): headings, rules, blockquotes, bullets, fenced code blocks, tables, inline emphasis, links, strikethrough, code. The plugin emits SH-13's tree: `column` of blocks;
4. **`render_flat` semantics.** For the `reference_excerpt` role: headings bold at body size, never larger (the pre-split rule, `src/desk/markdown.rs:8-10`).
5. **`note_line` semantics.** One line in, one `text` with spans out; a fenced-code state carries across lines (the plugin is called with the line and a small per-file state token the host keeps, or with the whole file and a line index — whichever the host API offers;
6. **Tests in the plugin.** Unit tests for every construct of the subset (one per construct, input markdown → expected tree, as data); a golden test that the pito-ui renderer's own test inputs (read from `work/src/markdown.rs`'s tests at writing time) produce the same block structure.
7. **Registry entry.** Add the plugin to `pito-plugins/index.json` with its release asset URL and SHA-256 as SH-16's CI computes them (the CI builds the artifact and writes the hash; never a hand-typed hash), and a page in the developer docs listing it as the reference for the `page_body` slot.

## Guards
- Do not touch: - The host and the slots (`pito-work/src/plugins/*`): WK-13's; only the role argument, if missing, and only as step 1 says.
- Gate: the project's gate
- Accept: `src/desk/markdown.rs` and the desk's own markdown renderer are gone; `grep -rn 'markdown::render' src` returns nothing.
- Accept: The plugin's test suite covers every construct of the subset by name (headings, rules, blockquotes, bullets, fenced code, tables, emphasis, links, strikethrough, code): ten test names in the runbook.
- Accept: US-2's 1x capture matches the baseline capture of main `3bfaf06` in block structure and type sizes (the runbook names the two files and the measured heading and body sizes).
- Accept: The reader's gutter and band are pixel-identical with and without the plugin (US-4, measured on the two captures).
