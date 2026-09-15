# pito-plugins

One registry for the three PITO desks: Work, Pigeon and Studio. Plugins and
themes are listed in `index.json`, installed from GitHub releases over
HTTPS, and verified by SHA-256 before a desk loads them. A desk fetches the
index only from its Plugins screen; nothing is fetched at boot.

## Listing a plugin

Fork this repository, add an entry to `index.json` (its shape is
`schema/index.schema.json`), and open a pull request. CI validates the
entry; a maintainer merges. A listed plugin lives in a public repository,
and its `sha256` is the hash of the released asset's bytes.

## Building one

Start from `templates/plugin/` (Rust, `wasm32-wasip2`) or
`templates/theme/` (palette tokens, no code), and read `docs/`. The plugin
interface is the `pito:host` WIT world under `wit/`, plus one world per
desk.
