# glg-training-site — Pages redirect branch

This branch (`pages-redirect`) is the **published GitHub Pages source**. It contains
**redirect stubs only** — no training content, no answer keys.

Every module URL that previously served a full training module now serves a stub that
redirects to the canonical Azure host:

    https://red-dune-0e206040f.7.azurestaticapps.net/<module>.html

## Why

`main` served 41 training modules with answer keys embedded in client-side HTML.
Staff-facing links point at this host, so the Azure cutover reduced zero real-world
exposure until traffic actually moved. Ruled P1 by Harrell 2026-08-19 (msg 45735).

## Important

- **`main` is unchanged.** It remains the canonical module tree. Do not merge this branch into it.
- Pages source was repointed `main` -> `pages-redirect`. Reverting = repoint Pages back.
- Pre-cutover content is tagged `pre-redirect-2026-08-19` on `main`.
- **Git history on this PUBLIC repo still contains every answer key.** Taking Pages down does
  not remove them. That is a separate open decision (D3) for Harrell: make the repo private,
  or accept it.
