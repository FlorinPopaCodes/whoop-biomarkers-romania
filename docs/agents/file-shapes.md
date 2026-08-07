# The shape of every product file

Read this before editing `README.md`, `BIOMARKERS.md` or any
`SHOPPING-LIST-*.md`. The invariants in `CLAUDE.md` say what must stay true;
this says what the files look like.

## `README.md` — which provider do I pick?

A thin landing page: intro, catalogue links, the headline Core verdict, pointers
to the rest.

**Its table is four hand-copied numbers per provider and nothing enforces any of
them.** After a Basket recompute, copy across:

- *Like-for-like* — the Common Set total: Core minus any biomarker some provider
  doesn't sell. Currently that is Cortisol alone, so the set is 56.
- *Everything it sells* — that provider's full Core total, which is the `## Core`
  line of its shopping list verbatim, plus its coverage as `n/57`.
- *Subscriber (est.)* — the `subscriber ~N RON` figure from that same `## Core`
  line, for Regina Maria and MedLife.

The two money columns are on **different bases** and always were; the README now
says so out loud. Don't quietly re-base one to match the other.

## `BIOMARKERS.md` — what is this called, and who sells it?

A name map, split into Core and Extended. Plain text, no links, no prices.

**Core** is one flat table, alphabetical by Whoop biomarker, four columns: Whoop
Biomarker · Synevo test · Regina Maria test · MedLife test. Where a biomarker is
only obtainable inside a panel, the cell names the panel.

**Extended** is the same shape, grouped into subsections by Whoop's five
Specialized Panels. A biomarker shared by more than one repeats under each,
marked `†`. This is deliberate: it keeps every Specialized Panel's row count
matching its own coverage count in the shopping list, at the cost of the Extended
table running longer than a flat list would.

Each half is followed by its own short **Derived list** (Name · Formula) rather
than interleaving Derived rows into the map.

Derived means *no Romanian lab sells it and it must be computed from other
biomarkers on this list*. That is not Whoop's definition — Whoop marks LDL
Cholesterol, TIBC and ALP as "Calculated", but Romanian labs sell all three as
line items, so they are purchasable here. (Whoop's ALP row is simply an error; it
is an enzyme assay.)

**DHEA Sulfate** sits in its own one-line "not yet placed" note, and is the one
exception to this file carrying no prices — with no Block it has no shopping-list
line, so its three prices would otherwise vanish from the repo. See the open
items in `CLAUDE.md`.

## `SHOPPING-LIST-*.md` — what do I order, and what does it cost?

One file per provider, because a reader shops at one provider at a time. All
three share one skeleton. **Keep them identical in shape even where a provider
makes a section trivial.**

```
# <Provider> — Shopping List
  back-link + how to find a Test in this provider's catalogue

## Core
   metadata line
### Blood count … ### Vitamins        (twelve Blocks, this order)

## Extended
   metadata line
### Heart Health … ### Men's Health   (Whoop's five Specialized Panels)

## <Subscription tier>                 (Regina Maria, MedLife only)
## Free on a prevention referral       (all three)
   shared legend
   back-link to BIOMARKERS.md
---
disclaimer · verified date · licence
```

The subscription section is headed by the tier's own name — `## Comfort Premium`,
`## Respect Infinit` — not a generic word.

### Metadata lines

Numbers never go in heading text. They go on a line directly under the heading:

- Fully covered: `370 RON · 6 biomarkers · 6 derived`
- Partially: `+595 RON · 3 of 16 biomarkers · 13 not sold here`
- With a subscriber column, `subscriber ~N RON` goes second:
  `2,290 RON · subscriber ~985 RON · 57 biomarkers · 18 derived`

Rules:

- `n of m` only when `n ≠ m`.
- Omit the `derived` segment when a Block has none.
- `~` marks the figure an estimate, so it needs *both* conditions: a non-zero
  figure, and at least one `○` line in the Block. A subtotal of 0, or a non-zero
  one made entirely of full-price and `●` lines, takes no `~`.
- `+` on an Extended subtotal means *what this Specialized Panel costs on top of
  Core*.

### Item tables

`| Test | RON |`, plus `| Subscriber |` at Regina Maria and MedLife. Descending
price within a Block.

**Markers on the Test name**, space-separated after it:

| | |
|---|---|
| `‡` | One Test, several biomarkers |
| `†` | Shared across Specialized Panels |
| `↑` | Upgrade; the price shown is the *difference* |
| `§` | Free on a prevention referral from a family doctor |

**Markers in the Subscriber cell**, after the number — `0 ●`, `0 ○`:

| | |
|---|---|
| `●` | Guaranteed free, no Recommendation |
| `○` | Estimated, Recommendation-gated |

Don't put `●`/`○` on the Test name; don't put `‡ † ↑ §` in the price cells.

`‡` is judged **per Block, not per Test**: it marks a Test contributing more than
one biomarker *to the Block it sits in*. Regina Maria's `Profil LDL` bundles four
assays but contributes only LDL Small to Heart Health — LDL Cholesterol arrives
via `Profil lipidic` in Core — so it is correctly unmarked.

`↑` appears only at Synevo and Regina Maria, whose reticulocyte-inclusive
hemograms are upgrades. MedLife's reticulocyte count is a standalone Test.

`§` takes no guaranteed/estimated grade the way the subscriber glyphs do — not
because it is ungated, but because a second glyph would grade all 27 lines
identically. See `docs/adr/0002-only-the-prevention-route.md`.

### The shared legend

One row per marker the file actually uses, **worded identically across all three
files**. Change a row's wording in one, change it in all three. Which rows appear
follows from the file: Synevo carries no `●`/`○`, MedLife no `↑`.

Beyond the six markers, every legend also explains three non-marker rows —
`+`, `Core` and `Derived` — in the same table. Keep them.

### Test name links

**Hyperlinked at Synevo only.** Regina Maria's links were dropped deliberately:
`?investigation=<id>` URLs don't reach a test page, they run ~130 characters per
row, and only ~241 of RM's 1,083 tests have clean `/utile/dictionar-de-analize/`
URLs — linking some and not others is worse than linking none. **Don't re-add
them** on rediscovering that `data-drupal-investigation` exists. MedLife has no
per-test links to add.

## Blocks

**Core Blocks** are clinical themes, in this fixed order, with these exact
headings:

`Blood count` · `Lipids` · `Metabolic` · `Liver` · `Kidney` · `Iron` ·
`Electrolytes` · `Protein` · `Thyroid` · `Hormones` · `Inflammation` ·
`Vitamins`

**Extended Blocks are Whoop's five Specialized Panels** — Heart Health,
Performance Health, Metabolic Health, Women's Health, Men's Health. Keep these
names and this grouping; it mirrors Whoop's own product structure.

Extended Blocks overlap. Leptin appears in three; Magnesium, B12, Folate, Free
T3/T4, Prolactin, Uric Acid, Zinc and the omega panel each appear in two or more.
Price each Block **standalone** — the cost of taking that Block on top of Core —
mark shared items `†`, and keep the one line saying you pay for them once.
**Block subtotals deliberately do not add up.**
