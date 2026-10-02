# How each product file is laid out

Read this before editing `README.md`, `BIOMARKERS.md` or any
`SHOPPING-LIST-*.md`. The invariants in `CLAUDE.md` say what must stay true;
this file says what the files look like.

## `README.md`: which provider should I pick?

A short landing page: an introduction, links to the catalogues, the Core
comparison, and links to the other files.

**Its table is copied by hand, and nothing checks it.** After recomputing a
Basket, copy across:

- *Core*: the provider's full Core total, taken exactly from the `## Core` line
  of its shopping list, plus its coverage as `n/57`.
- *Subscriber (est.)*: the `subscriber ~N RON` figure from the same `## Core`
  line, for Regina Maria and MedLife.

**The table has one price column because all three providers currently sell all
57 Core biomarkers.** That makes the Common Set the same as Core, so the totals
are already like for like. While MedLife was missing Cortisol, the table had a
separate *Like-for-like* column. If a provider stops selling a Core biomarker,
bring that column back. The two columns then measure different things, so don't
adjust one to match the other.

## `BIOMARKERS.md`: what is it called, and who sells it?

A name map in two halves, Core and Extended. Plain text, no links, no prices.

**Core** is one table, sorted alphabetically by Whoop biomarker, with four
columns: Whoop Biomarker · Synevo · Regina Maria · MedLife. If a biomarker only
comes as part of a panel, the cell names the panel.

**Extended** has the same columns, split into one subsection per Specialized
Panel. A biomarker in more than one Panel appears under each, marked `†`. This
keeps each Panel's row count equal to its count in the shopping list, even
though the table is longer than a flat list would be.

Each half ends with its own short **Derived list** (Name · Formula). Derived rows
aren't mixed into the name map.

Here, Derived means *no Romanian lab sells it, so you calculate it from other
biomarkers on the list*. That isn't Whoop's definition. Whoop marks LDL
Cholesterol, TIBC and ALP as "Calculated", but Romanian labs sell all three as
tests, so they count as purchasable here. (Whoop's ALP label is a mistake: ALP
is an enzyme test.)

**DHEA Sulfate** has its own short "not yet placed" note, and it's the only
place `BIOMARKERS.md` shows prices. With no Block, it has no shopping-list line,
so without the note its three prices would appear nowhere. See the open items in
`CLAUDE.md`.

## `SHOPPING-LIST-*.md`: what do I order, and what does it cost?

One file per provider, because a reader orders from one provider at a time. All
three follow the same outline. **Keep them the same shape, even where a section
has little to say for one provider.**

```
# <Provider> — Shopping List
  link back + how to find a Test in this provider's catalogue

## Core
   metadata line
### Blood count … ### Vitamins        (twelve Blocks, in this order)

## Extended
   metadata line
### Heart Health … ### Men's Health   (Whoop's five Specialized Panels)

## <Subscription name>                (Regina Maria and MedLife only)
## Free on a prevention referral      (all three)
   shared legend
   link to BIOMARKERS.md
---
disclaimer · date checked · licence
```

The subscription section uses the subscription's own name as its heading
(`## Comfort Premium`, `## Respect Infinit`), not a generic word.

### Metadata lines

Numbers never go in headings. They go on the line directly under the heading:

- Fully covered: `370 RON · 6 biomarkers · 6 derived`
- Partly covered: `+595 RON · 3 of 16 biomarkers · 13 not sold here`
- With a subscriber column, `subscriber ~N RON` comes second:
  `2,290 RON · subscriber ~985 RON · 57 biomarkers · 18 derived`

Rules:

- Write `n of m` only when `n` and `m` differ.
- Leave out the `derived` part when a Block has no derived biomarkers.
- `~` marks an estimate, so use it only when both are true: the figure isn't
  zero, and at least one line in the Block is `○`. A subtotal of 0, or one made
  up only of full-price and `●` lines, gets no `~`.
- `+` on an Extended subtotal means *what this Specialized Panel costs on top of
  Core*.

### Item tables

Columns are `| Test | RON |`, plus `| Subscriber |` at Regina Maria and MedLife.
Sort each Block by price, highest first.

**Markers after the Test name**, separated by a space:

| | |
|---|---|
| `‡` | One Test, several biomarkers |
| `†` | Shared between Specialized Panels |
| `↑` | Upgrade; the price shown is the *difference* |
| `§` | Free on a prevention referral from a family doctor |

**Markers in the Subscriber cell**, after the number (`0 ●`, `0 ○`):

| | |
|---|---|
| `●` | Guaranteed free, no Recommendation needed |
| `○` | Estimated, needs a Recommendation |

`●` and `○` go only in the price cell; `‡ † ↑ §` go only after the Test name.

`‡` is decided **per Block, not per Test**. It marks a Test that gives more than
one biomarker *to the Block it's in*. Regina Maria's `Profil LDL` measures four
things but only adds LDL Small to Heart Health, because LDL Cholesterol already
comes from `Profil lipidic` in Core. So it has no `‡`.

`↑` only appears at Synevo and Regina Maria, where the hemogram with
reticulocytes is an upgrade of the Core hemogram. At MedLife, reticulocytes are a
separate Test. **Work out the difference separately for each column**, against
what the Core version costs in that column. If a subscriber gets the Core
hemogram free, their upgrade costs the full price of the upgraded Test, not the
difference shown in the standard column.

`§` doesn't get a guaranteed or estimated grade like the subscriber markers do.
It does depend on the doctor, but a second marker would give every `§` line the
same grade, so the condition is explained once in prose instead. See
`docs/adr/0002-only-the-prevention-route.md`.

### The shared legend

One row for each marker the file uses, **worded the same in all three files**.
If you change a row's wording in one file, change it in all three. Which rows
appear depends on the file: Synevo has no `●` or `○`, and MedLife has no `↑`.

The `○` row is the one intended difference. MedLife's ends with `Up to 4 times a
year.` because its annex limits each test to four uses a year. Comfort Premium's
annex has no such limit, so Regina Maria's row ends a sentence earlier. Keep
them different.

Besides the markers, every legend also explains `+`, `Core` and `Derived` in the
same table. Keep those rows.

### Links on Test names

**Only Synevo's Test names are links.** Regina Maria's were removed on purpose:
the `?investigation=<id>` URLs don't open a test page, they're about 130
characters long, and only about 241 of RM's 1,083 tests have clean
`/utile/dictionar-de-analize/` URLs. Linking some tests and not others is worse
than linking none. **Leave them unlinked**, even when you find that
`data-drupal-investigation` exists. MedLife has no per-test pages to link to.

## Blocks

**Core Blocks** are clinical themes, in this order, with these exact headings:

`Blood count` · `Lipids` · `Metabolic` · `Liver` · `Kidney` · `Iron` ·
`Electrolytes` · `Protein` · `Thyroid` · `Hormones` · `Inflammation` ·
`Vitamins`

**Extended Blocks are Whoop's five Specialized Panels**: Heart Health,
Performance Health, Metabolic Health, Women's Health and Men's Health. Keep these
names and this grouping, because they match how Whoop sells them.

Extended Blocks overlap. Leptin is in three; Magnesium, B12, Folate, Free T3/T4,
Prolactin, Uric Acid, Zinc and the omega panel are each in two or more. Price
each Block **on its own**, as the cost of adding just that Block to Core. Mark
shared items `†`, and keep the line that says you only pay for them once.
**Block subtotals are not meant to add up.**
