# 1. Prices live in the shopping lists

Date: 2026-08-05

## Status

Accepted.

## Context

Each price used to be written in more than one file. A Regina Maria test's
price appeared in `BIOMARKERS.md` (as a Solo Price), in
`SHOPPING-LIST-REGINA-MARIA.md` (as a line item), and again in
`SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` (as the crossed-out standard
price). That was three copies, nothing to keep them in sync, and a quarterly
refresh that had to update all three by hand.

The copies had already drifted apart without anyone noticing. `README.md` gave
Synevo's Core basket as 1,797 RON while `SHOPPING-LIST-SYNEVO.md` said 1,860
RON. Both were right: one was over the Common Set and the other over the
provider's full Core, but neither page said which. In the same README table, the
`Subscriber (estimated)` column used full Core while the column next to it used
the Common Set.

The Solo Price rule also made `BIOMARKERS.md`'s price column awkward. Twenty
biomarkers come from one hemogram, so twenty rows showed the same number, and
adding up the column, which is the obvious thing to do, gave a wrong answer. The
file needed a permanent warning about a problem it had created itself.

## Decision

Prices live in the three `SHOPPING-LIST-*.md` files and nowhere else.

- `BIOMARKERS.md` drops its three `RON` columns and becomes a name map: what
  each provider calls each Whoop biomarker, with `—` for not sold and `?` for
  unknown.
- Subscriber prices move into each provider's shopping list as a second price
  column. `SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` and
  `SUBSCRIPTION-MEDLIFE-RESPECT-INFINIT.md` are deleted.
- `README.md` states what each figure is a total of, so its numbers can't
  quietly disagree. `docs/agents/file-shapes.md` describes the resulting column
  layout.

Each file now answers one question: `README.md` which provider, `BIOMARKERS.md`
what it's called and who sells it, `SHOPPING-LIST-*.md` what it costs.

One price has no shopping-list home. DHEA Sulfate belongs to no Specialized
Panel, so it has no Block. Its three prices stay in `BIOMARKERS.md`, in the note
that already explains why it isn't placed.

## Consequences

**Solo Price is gone.** Without a price column there's nowhere to put it, and
both of its problems ("don't add up this column" and "MCV shows the full
hemogram price") disappear. The term is removed from `CONTEXT.md`.

**You can no longer compare one biomarker's price across providers in one
place.** One row used to show that Regina Maria charges 130 RON for ApoB and
Synevo charges 52. Now you have to open two shopping lists. This is the real
cost of the decision. We accepted it because the repo's main questions are
answered elsewhere: the provider comparison is a basket comparison in
`README.md`, and the price you'll pay for a test is in the shopping list you're
ordering from.

**Subscriber estimates now sit next to firm prices.** A reader might take a
Recommendation-gated estimate for a guaranteed price. To prevent that, each row
has a marker, `●` for guaranteed and `○` for estimated, explained in each file's
legend. The grade stays next to the number instead of being stated once at the
top of a file.

**A refresh updates each price in one place.** The Basket still has to be
recomputed as before; the difference is that the result no longer has to be
copied into a second and third file.
