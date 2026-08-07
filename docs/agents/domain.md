# Domain docs

Where this repo's domain documentation lives, for skills that go looking for it.

- **`CONTEXT.md`** at the repo root — the glossary. Read it before writing
  anything.
- **`docs/adr/`** at the repo root — the decisions. Read the ones touching what
  you're about to change.

Single-context layout: no `CONTEXT-MAP.md`, no per-context `CONTEXT.md`, and no
source tree to scope either to. This repo is five Markdown files and the notes
that keep them correct.

## Use the glossary's vocabulary

When your output names a domain concept — an issue title, a proposal, a heading,
a commit message — use the term as `CONTEXT.md` defines it, and not the synonyms
it lists under *Avoid*.

If the concept you need isn't in the glossary, that's a signal: either you're
inventing language the project doesn't use, or there's a real gap worth naming.

## Flag ADR conflicts

If your output contradicts an ADR, say so rather than silently overriding it:

> _Contradicts ADR-0002 (only the prevention route) — but worth reopening
> because…_
