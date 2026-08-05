# 1. Prices live in the shopping lists

Date: 2026-08-05

## Status

Accepted.

## Context

Every price used to exist in more than one file. A Regina Maria test's price was
written in `BIOMARKERS.md` (as a Solo Price), in `SHOPPING-LIST-REGINA-MARIA.md`
(as a line item), and again in `SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` (as
the struck-through standard price). Three copies, no mechanism keeping them
equal, and a refresh cadence of once a quarter that had to update all three by
hand.

The copies had already started disagreeing in a way nothing flagged. `README.md`
quoted Synevo's Core basket as 1,797 RON while `SHOPPING-LIST-SYNEVO.md` said
1,860 RON — both correct, one over the Common Set and one over the provider's
full Core, with nothing on either page saying which. In the same README table the
`Subscriber (estimated)` column was silently on the full-Core basis while the
column beside it was on the Common Set basis.

Separately, the Solo Price rule made `BIOMARKERS.md`'s price column actively
awkward: sixteen biomarkers come out of one hemogram, so sixteen rows repeated
the same number, and a reader's natural instinct — summing the column — was
wrong by construction. The file carried a standing warning about a trap it had
itself created.

## Decision

Prices live in the three `SHOPPING-LIST-*.md` files and nowhere else.

- `BIOMARKERS.md` loses its three `RON` columns and becomes a name map: what each
  provider calls each Whoop biomarker, with `—` for absent and `?` for
  undetermined.
- Subscriber pricing merges into the provider's shopping list as a second price
  column. `SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` and
  `SUBSCRIPTION-MEDLIFE-RESPECT-INFINIT.md` are deleted.
- `README.md` shows both the Common Set and full-Core figures side by side, so
  the two can no longer disagree invisibly.

Each file now answers exactly one question: `README.md` which provider,
`BIOMARKERS.md` what it's called and who sells it, `SHOPPING-LIST-*.md` what it
costs.

One price genuinely has no shopping-list home — DHEA Sulfate belongs to no
Specialized Panel, so it has no Block. Its three prices stay in `BIOMARKERS.md`,
in the note that already explains why it's unplaced.

## Consequences

**Solo Price stops existing.** With no price column there is no cell for it, and
both traps it carried — "never sum this column" and "MCV shows the full hemogram
price" — stop being possible. The term is retired from `CONTEXT.md`.

**Per-biomarker price comparison is gone.** You can no longer read one row and
see that Regina Maria charges 130 RON for ApoB where Synevo charges 52. Answering
that now means opening two shopping lists. This is the real cost of the decision.
It was judged acceptable because the repo's actual questions are answered
elsewhere: the provider head-to-head is a basket comparison in `README.md`, and
the per-test price you'd pay is in the shopping list you're ordering from.

**Subscriber estimates now sit next to hard prices.** The risk is a reader taking
a referral-gated estimate for a guaranteed price. Mitigated by per-row glyphs —
`●` guaranteed, `○` estimated — defined in each file's legend, so the grade
travels with the number instead of being asserted once in a file header.

**A refresh updates one place per price.** The Basket recompute discipline is
unchanged; what changes is that its output no longer has to be copied into a
second and third file afterwards.
