# Maintaining this repo

The product is five Markdown files: `README.md`, `BIOMARKERS.md`,
`SHOPPING-LIST-SYNEVO.md`, `SHOPPING-LIST-REGINA-MARIA.md` and
`SHOPPING-LIST-MEDLIFE.md`. There are no scripts, build or tests. Everything
that keeps these files correct is written down here and in `docs/`; nothing
checks it automatically.

Each file answers one question, and **each price is written in exactly one
place**:

| File | Answers | Prices |
|---|---|---|
| `README.md` | Which provider should I pick? | Four numbers per provider, copied by hand |
| `BIOMARKERS.md` | What is this biomarker called, and who sells it? | None, with one documented exception |
| `SHOPPING-LIST-*.md` | What do I order and what does it cost? | Every price, as the source of truth |

There are 127 biomarkers: 75 Core and 52 Extended. 100 can be bought (57 Core,
43 Extended) and 27 are Derived. Every coverage count in the repo uses these
numbers.

## Read first

- **`CONTEXT.md`**: the glossary (Test, Panel, Block, Basket, Common Set,
  Subscriber Price, Referral, Recommendation) and the synonyms to avoid. Read it
  before writing anything, and use its terms.
- **`docs/agents/file-shapes.md`**: the exact layout of each product file. Read
  it before editing one.
- **`docs/agents/refresh.md`**: the quarterly refresh step by step: where each
  provider's prices come from, the public sources behind `§`, how the subscriber
  columns are worked out, and how to recompute a Basket. Read it before a
  refresh.
- **`docs/adr/`**: decisions to keep unless someone deliberately reopens them.
  `0001` puts prices in the shopping lists; `0002` covers only the prevention
  route.

## Invariants

Nothing will flag it if you break one of these.

1. **Each price lives in one shopping list.** `BIOMARKERS.md` has no prices,
   except for DHEA Sulfate. It belongs to no Specialized Panel, so it has no
   shopping-list line, and its three prices sit in the note that explains why.
   See `docs/adr/0001-prices-live-in-the-shopping-lists.md`.
2. **After any price change, recompute every Basket from scratch.** The Basket
   is calculated, not stored. If a panel becomes cheaper than buying its parts,
   the right Basket changes, but nothing in the README looks wrong. This is the
   easiest way for the repo to become wrong without anyone noticing.
3. **Prices are per Test, never per biomarker.** Each Test appears once, so the
   price columns add up correctly. The old Solo Price, which repeated one
   hemogram's price on every biomarker it measures, is gone. Keep it out.
4. **Derived biomarkers cost 0 RON and never get their own line.** A product
   *named* after one is still a Test, though. All three providers sell an
   "Indice HOMA". Where it bundles its input tests for less than they cost
   separately, it goes in the Basket like any other Panel. Synevo's does (82
   against 86) and is in. The derived value itself still costs 0.
5. **`—` and `?` mean different things.** `—` means the provider sells nothing
   that measures the biomarker. `?` means we don't know. Never turn a `?` into a
   `—`: unresolved biomarkers are left out of the Common Set and named, and
   guessing would change totals without evidence.
6. **Keep prices out of headings.** Anchors like `#lipids` are permalinks and
   must not change on a refresh. Numbers go on the metadata line under the
   heading.
7. **Compare providers over the Common Set**, so the comparison is like for
   like. List each provider's Exclusives separately with their prices, and keep
   them out of the comparable total.
8. **Count absences; don't list them.** Write `13 not sold here`, not thirteen
   names. `BIOMARKERS.md` already shows every absence as `—`.
9. **Product files say what is true, not how it was decided.** Reasons a line
   is kept at full price, why an annex match was rejected, or why a name is
   ambiguous belong in `docs/agents/`.
10. **Only update a date for what you actually checked.** The four files with
    prices share one date because they share prices. `BIOMARKERS.md`'s date means
    *names checked against the catalogues*. Each subscription section has its
    own date, meaning *that annex was re-read at its source*, and it can move
    separately from the price footer. The `§` date means *the free list was
    re-checked*. These are four separate claims.

## The refresh

About every three months. Re-check **every** price at all three providers, not
only the gaps: a table that mixes new and old prices under one date is wrong
about half of them.

Work in this order, because each step uses the result of the one before.
`docs/agents/refresh.md` explains how to do each step.

1. Check whether Whoop's biomarker list has changed.
2. Fetch all three catalogues again and re-check every price.
3. Re-check every test name against the catalogue and update `BIOMARKERS.md`.
4. Re-check the `§` list against the CNAS prevention lists. It changes
   independently of prices, so nothing else would catch it.
5. Re-read each subscription annex at its source, then re-grade every `●` and
   `○`. An annex can change without any public price changing.
6. Confirm each panel still contains the same tests. Apart from prices, this is
   the only input to the Basket.
7. Recompute every Basket from scratch.
8. Rewrite the shopping lists, and recompute every Block subtotal and coverage
   count.
9. Copy the four numbers per provider into the README.
10. Update the dates, following invariant 10.

**Adding a new provider is not a refresh.** The existing providers' prices
weren't checked again, so they keep their old date. A footer may show one date
per provider until the next full refresh brings them back to one.

## Open items

These are known and left open on purpose. Resolve them during a refresh.

- **Regina Maria's `Indice HOMA` is unconfirmed.** At 90 RON it would be cheaper
  than buying `Glucoza serica` (30) and `Insulina` (70) separately, but Regina
  Maria has no per-test page, so we don't know whether it reports both input
  values or only the ratio. If it reports both, RM's Core drops by 10 RON.
  Details in `docs/agents/refresh.md`.
- **We don't know what Regina Maria's fatty-acid panel reports.** The catalogue
  lists `Acizi grasi - saturati, mononesaturati, omega-3, omega-6` but doesn't
  say whether AA, DHA, EPA or LA are reported separately. Keep those four
  Extended biomarkers as `?` until someone finds the panel's contents, and don't
  count or price them on the panel name alone.
- **DHEA Sulfate has no Specialized Panel.** It's Extended and all three
  providers sell it, but Whoop's own panel pages don't put it in any of the five.
  It stays in its own note. Adding it to a related Panel would make that Panel's
  row count disagree with its shopping-list count. Revisit if Whoop says where it
  belongs.
- **A CNAS draft from July 2026 proposes adding HDL cholesterol** to the
  prevention list. The current list, checked 2026-10-02, still leaves it out. If
  it's added, a new line gets `§` at Synevo and MedLife. Regina Maria's lipid
  note would stay: triglycerides would still not be free, so `Profil lipidic`
  (85) would lose to buying Trigliceride alone (30) by an even wider margin.

## Style

Short and plain. Five files, one question each: `README.md` is a short landing
page, `BIOMARKERS.md` is the name map, and each `SHOPPING-LIST-*.md` has every
price for its provider, including the subscriber column. Each file has its own
link back, a one-line disclaimer and a one-line licence. No table of contents,
no emoji in headings, no medical explanations.

Before adding a section to a product file, check whether it belongs in
`docs/agents/` instead. Explanations of why usually do.

## Agent skills

**Issue tracker.** Issues and PRDs are GitHub issues in this repo, managed with
the `gh` CLI. See `docs/agents/issue-tracker.md`.

**Domain docs.** One context: `CONTEXT.md` and `docs/adr/` at the repo root.
See `docs/agents/domain.md`.
