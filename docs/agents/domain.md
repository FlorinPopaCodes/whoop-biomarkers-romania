# Domain docs

Where this repo keeps its domain documentation, for skills that look for it.

- **`CONTEXT.md`** at the repo root is the glossary. Read it before writing
  anything.
- **`docs/adr/`** at the repo root holds the decisions. Read any that touch what
  you're about to change.

This repo has one context: no `CONTEXT-MAP.md`, no per-context `CONTEXT.md`, and
no source tree to split them by. It's five Markdown files plus the notes that
keep them correct.

## Use the glossary's terms

When you name a domain concept anywhere (an issue title, a proposal, a heading,
a commit message), use the term as `CONTEXT.md` defines it, not one of the
synonyms it lists under *Avoid*.

If the concept you need isn't in the glossary, either you're inventing a term
the project doesn't use, or there's a real gap that needs a name.

## Point out ADR conflicts

If what you're proposing contradicts an ADR, say so openly:

> _Contradicts ADR-0002 (only the prevention route), but worth reopening
> because…_
