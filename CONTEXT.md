# Whoop Biomarkers — Romanian Lab Mapping

Whoop's paid blood panel is US-only, but uploading lab results is free on any
membership worldwide. This repo maps every biomarker Whoop will accept on upload
to the lab test a Romanian user can actually buy at Synevo, Regina Maria, or
MedLife, and says what it costs.

## Language

### What Whoop measures

**Biomarker**:
A single value Whoop will display. The unit of *want*. The full set is the 127
Whoop accepts on upload — 100 purchasable in Romania, 27 Derived.
_Avoid_: marker, analyte, test

**Core**:
The 75 Biomarkers of Whoop's Comprehensive Health Panel. The like-for-like set —
Basket cost and the provider head-to-head are both quoted over Core.
_Avoid_: base panel, standard panel, CHP

**Extended**:
The 52 Biomarkers beyond Core, drawn from Whoop's five Specialized Panels. Opt-in.
_Avoid_: extras, add-ons, advanced

**Derived Biomarker**:
A Biomarker no Romanian lab sells, obtained by computing it from other
Biomarkers — Globulin, HOMA-IR, Plasma Osmolality. Costs 0 RON and is always
Covered, because every input is itself a Biomarker on the list. Defined by
what can be *bought here*, not by Whoop's methodology column: Whoop marks LDL
Cholesterol and TIBC as calculated, but both are sold as line items in Romania,
so neither is Derived. (Whoop also marks ALP calculated, which is simply wrong.)
_Avoid_: indirect biomarker, calculated marker

### What the user buys

**Test**:
A single purchasable line item in a Provider's catalogue, priced and orderable
on its own. The unit of *purchase*. A Test covers one or more Biomarkers.
_Avoid_: analysis, analiză, product, SKU

**Panel**:
A Test covering more than one Biomarker, such as a hemogram. A property of a
Test, not a separate kind of thing. Never means one of Whoop's panels — those
are Core and Extended.
_Avoid_: profile, profil, package, pachet, bundle

**Provider**:
A Romanian lab network selling Tests. Exactly three: Synevo, Regina Maria,
and MedLife.

### Money and coverage

**Solo Price**:
What it costs to obtain one Biomarker *alone* by the cheapest route at a
Provider — its own Test's price, or the whole Panel's price where that is the
only route. The number in every price cell of the Comparison Table. Solo Prices
do not sum to anything meaningful and the column is never totalled.
_Avoid_: unit price, price per biomarker

**Covered / Uncovered / Unresolved**:
The three states a Biomarker can hold at a Provider. Covered: some Test yields
it, and it has a Solo Price. Uncovered: the Provider sells nothing that yields
it, rendered as an em-dash. Unresolved: we could not determine it, rendered as
`?`. Unresolved is excluded from the Common Set and named explicitly, so a soft
number is never laundered into a hard one.

**Common Set**:
The Core Biomarkers Covered by all Providers. Provider totals are quoted over
it so the head-to-head is like-for-like.

**Exclusive**:
A Biomarker Covered by one Provider and not the others. Priced and listed apart
from the Common Set total, never folded into it.

**Basket**:
The minimum-cost set of Tests covering every Core Biomarker a Provider can
Cover. An optimizer output, not data — recompute it whenever a price moves,
because a cheaper Panel can beat buying its parts and silently change the answer.
_Avoid_: cart, order, selection

**Block**:
A group of the shopping list's Tests carrying its own subtotal and Biomarker
count, so a user can drop a whole theme and see what they saved and what they
lost. Core Blocks are clinical themes; Specialized Blocks are Whoop's five —
Heart, Performance, Metabolic, Women's, Men's Health.
_Avoid_: category, group, section

**Shared Biomarker**:
A Biomarker appearing in more than one Specialized Block — Leptin, Magnesium,
Free T3/T4, the omega panel. Each Block is priced standalone, so Shared
Biomarkers are marked `†` and two Blocks together cost less than their subtotals
suggest.
