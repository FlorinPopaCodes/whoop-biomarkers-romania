# Maintaining this repo

`README.md`, `BIOMARKERS.md`, `SHOPPING-LIST-SYNEVO.md`,
`SHOPPING-LIST-REGINA-MARIA.md` and `SHOPPING-LIST-MEDLIFE.md` are the product —
all five of it. There are no scripts, no build, no tests. Everything that keeps
these files correct is written down; nothing is enforced by a machine.

Each file answers exactly one question, and **every price exists in exactly one
place**:

| File | Answers | Prices |
|---|---|---|
| `README.md` | Which provider do I pick? | Four hand-copied numbers per provider |
| `BIOMARKERS.md` | What is this biomarker called, and who sells it? | None, bar one documented exception |
| `SHOPPING-LIST-*.md` | What do I order and what does it cost? | Source of truth for every price |

The biomarker set is 127: 75 Core plus 52 Extended. 100 are purchasable — 57
Core, 43 Extended — and 27 are Derived. Those numbers appear in every coverage
count in the repo.

## Read first

- **`CONTEXT.md`** — the glossary. Test, Panel, Block, Basket, Common Set,
  Subscriber Price, Referral, Recommendation. It also names the synonyms to
  avoid. Use its vocabulary; read it before writing anything.
- **`docs/agents/file-shapes.md`** — the exact shape of every product file.
  Read before editing one.
- **`docs/agents/refresh.md`** — the quarterly sweep end to end: where each
  provider's prices come from, how the subscriber columns and the `§` set are
  derived, how to recompute a Basket. Read before a refresh.
- **`docs/adr/`** — decisions not to silently reverse. `0001` puts prices in the
  shopping lists; `0002` models only the prevention route.
- **`docs/research/cnas-medic-de-familie.md`** — the state-funding analysis `§`
  came from, including the diagnostic route this repo deliberately omits.

## Invariants

Break one of these and the repo is wrong in a way nothing will flag.

1. **Every price lives in exactly one shopping list.** `BIOMARKERS.md` carries
   no prices. The sole exception is DHEA Sulfate, which belongs to no
   Specialized Panel and so has no shopping-list line; its three prices live in
   the note explaining why it's unplaced. See
   `docs/adr/0001-prices-live-in-the-shopping-lists.md`.
2. **After changing any price, recompute every Basket from scratch.** The Basket
   is an optimizer output, not data. Drop Synevo's lipid profile from 90 to 60
   and the right answer changes — buying the panel now beats buying its parts —
   but nothing in the README looks broken. This is the one way this repo goes
   quietly wrong.
3. **Prices are per Test, never per biomarker.** Each Test appears once, so the
   columns sum honestly. The old Solo Price — one hemogram's price repeated on
   every biomarker it yields, under a "never sum this column" warning — is
   retired. Don't reintroduce it.
4. **Derived biomarkers are 0 RON and never get a line of their own.** But a
   product *named* after one is still a Test: all three providers sell an "Indice
   HOMA", and where it bundles its input assays for less than they cost
   separately it belongs in the Basket like any other Panel. Synevo's does — 82
   against 86 — and is in. The derived value itself stays 0 either way.
5. **`—` and `?` are different states.** `—` means the provider genuinely sells
   nothing that yields the biomarker; `?` means undetermined. Never render `?` as
   `—` — unresolved biomarkers are excluded from the Common Set and named
   explicitly, and quietly demoting one shifts every total on evidence you don't
   have.
6. **Never put a price in heading text.** Anchors like `#lipids` are permalinks
   and must survive a refresh. Numbers go on the metadata line under the heading.
7. **Quote the head-to-head over the Common Set**, so the comparison is
   like-for-like. List each provider's exclusives separately with their prices;
   never fold an exclusive into the comparable total.
8. **Absences are counted, never named** — `13 not sold here`, not a list of
   thirteen biomarkers. `BIOMARKERS.md` already renders every absence as `—`.
9. **No per-item editorial notes in the product files.** Why a line is kept at
   full price, why an annex match was rejected, why a name is ambiguous — all
   agent-facing, all lives in `docs/agents/`. The product files state what is
   true, not how it was decided.
10. **Re-stamp a footer date only for what you actually re-verified.** The four
    price-carrying files share one date because they share prices.
    `BIOMARKERS.md`'s date means *names checked against the catalogue*. Each
    subscription section carries its own date meaning *that annex re-read at its
    source*, which moves independently — Regina Maria's already sits ahead of its
    price footer. The `§` date means *the funded set re-derived*. These are four
    different claims.

## The refresh sweep

Every ~3 months. Re-verify **every** price at all three providers, not just the
gaps: a table mixing fresh and stale cells under one date lies about half its
contents.

Work in this order — each step consumes the one before it.
`docs/agents/refresh.md` carries the how for every step.

1. Check whether Whoop's biomarker list moved.
2. Re-fetch all three catalogues; re-verify every price.
3. Re-check every test name against the catalogue → `BIOMARKERS.md`.
4. Re-derive the `§` set from the CNAS prevention lists. It moves independently
   of every price here, so nothing else would catch a change.
5. Re-check each subscriber annex at its source, then re-grade every `●`/`○`.
   The annex can change independently of that provider's public prices.
6. Confirm panel membership still holds — the Basket optimizer's only input
   besides prices.
7. Recompute every Basket from scratch.
8. Rewrite the shopping lists; recompute every Block subtotal and coverage count.
9. Copy README's four numbers per provider across.
10. Re-stamp the dates, per invariant 10.

**Onboarding a new provider is not a sweep.** Adding a provider's first column
and file leaves the existing providers' cells genuinely untouched, not stale, so
re-stamping them under the new provider's date is the same lie in reverse. A
footer may carry one date per provider until the next full sweep collapses it
back to one.

## Open items

Carried deliberately, not forgotten. Each is resolved during a sweep, not before.

- **Regina Maria's `Indice HOMA` is unverified.** At 90 it would beat buying
  `Glucoza serica` and `Insulina` separately (30 + 70), but Regina Maria
  publishes no per-test page, so whether it reports the two input assays or only
  the ratio is unknown. Worth 10 RON off RM's Core if it holds.
  `docs/agents/refresh.md` carries the detail.
- **DHEA Sulfate is unplaced.** Extended, sold at all three providers, but part
  of none of Whoop's five Specialized Panels — checked against Whoop's own panel
  marketing, not inferred. It stays in its own note rather than being folded into
  a topically-close Panel, which would make that Panel's row count disagree with
  its shopping-list coverage count. Revisit if Whoop ever documents where it
  belongs.
- **A CNAS draft in transparency (July 2026) proposes adding HDL cholesterol.**
  That would mark a new line at Synevo and MedLife. It would *not* retire Regina
  Maria's lipid swap note: triglycerides stay unfunded, so `Profil lipidic` would
  still lose — to buying Trigliceride alone for 30 against the panel's 85, a
  wider gap than today's.

## Style

Spartan. Five files, one question each — `README.md` a thin landing page,
`BIOMARKERS.md` the name map, one `SHOPPING-LIST-*.md` per provider carrying
every price including the subscriber column. Each file carries its own back-link,
one-line disclaimer, and one-line licence. No table of contents, no emoji
headers, no medical essay.

Before adding a section to any product file, check whether it belongs in
`docs/agents/` instead. Justifications especially.

## Agent skills

**Issue tracker.** Issues and PRDs live as GitHub issues in this repo (`gh`
CLI). See `docs/agents/issue-tracker.md`.

**Domain docs.** Single-context layout — `CONTEXT.md` and `docs/adr/` at the
repo root. See `docs/agents/domain.md`.
