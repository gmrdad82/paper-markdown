# pito-plugins

The public registry for the PITO desks' plugins and themes: `index.json`,
the developer documentation, the two templates, and the first-party
plugins and themes with the CI that builds them. Every other estate repo
is private; this one is read by strangers, and its laws follow from that.

# Hard rules

- **Public, so nothing internal, ever.** Concretely: no path under a home
  directory, no hostname but github.com, no personal tooling, no tunnel,
  no client, no person, no agent name, no internal item number. The gate
  in `.github/workflows/validate.yml` lists only generic patterns; a
  denylist of private names would itself leak them.
- **The index is the only listing.** A plugin or theme exists for a desk
  when `index.json` names it. Entries arrive by pull request, are
  validated by CI, and are merged by the owner.
- **Hashes are read, never typed.** An entry's `sha256` is computed from
  the bytes of the released asset. First-party artifacts are built by CI
  on a `v*` tag and attached to the GitHub release with their
  `SHA256SUMS`; a release whose built hash differs from the index's is
  refused.
- **`wit/` is a copy.** The `pito:host` world and the per-desk worlds are
  owned upstream and mirrored here byte for byte; edit them upstream,
  never here.
- **A third party's plugin lives in a public repository**, and its asset
  URL sits under that repository's own releases.
- **Product names are the codenames** (Work, Pigeon, Studio) until the
  naming lands; write them so a rename is a search-and-replace.

# The local gate

From the repo root, in this order, before any handover:

```
node bin/validate-index.mjs
actionlint
(cd templates/plugin && cargo fmt --check && cargo check --target wasm32-wasip2)
git diff --check
```

plus the two grep lines of `validate.yml`, run locally, both silent. CI
runs the same on every pull request and every push to `main`; a `v*` tag
builds and attaches the first-party artifacts.

# Style

- A short header comment per file saying what it owns.
- Plain prose, no marketing. Security first.
