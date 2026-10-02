# Whoop Biomarkers — Romanian Lab Mapping

Whoop's paid blood panel is only sold in the US, but any member anywhere can
upload their own lab results for free. This repo takes every biomarker Whoop
accepts on upload, finds the test that measures it at Synevo, Regina Maria and
MedLife, and says what it costs.

## Language

### What Whoop measures

**Biomarker**:
A single value Whoop displays. It's what the user *wants*. The full set is the
127 Whoop accepts on upload: 100 you can buy in Romania and 27 Derived.
_Avoid_: marker, analyte, test

**Core**:
The 75 Biomarkers in Whoop's Comprehensive Health Panel. Basket costs and the
provider comparison are both quoted over Core.
_Avoid_: base panel, standard panel, CHP

**Extended**:
The 52 Biomarkers outside Core, taken from Whoop's five Specialized Panels.
Optional.
_Avoid_: extras, add-ons, advanced

**Derived Biomarker**:
A Biomarker no Romanian lab sells, which you calculate from other Biomarkers:
Globulin, HOMA-IR, Plasma Osmolality. It costs 0 RON and is always Covered,
because all its inputs are on the list. What counts is whether you can buy it
here, not how Whoop labels it. Whoop marks LDL Cholesterol and TIBC as
calculated, but Romanian labs sell both as tests, so neither is Derived. (Whoop
also marks ALP as calculated, which is a mistake.)
_Avoid_: indirect biomarker, calculated marker

### What the user buys

**Test**:
One item in a Provider's catalogue that you can order and pay for on its own.
It's what the user *buys*. A Test covers one or more Biomarkers.
_Avoid_: analysis, analiză, product, SKU

**Panel**:
A Test that covers more than one Biomarker, such as a hemogram. It's a kind of
Test, not a separate thing. A bare *Panel* never means one of Whoop's five;
those are always written in full as Specialized Panels.
_Avoid_: profile, profil, package, pachet, bundle

**Specialized Panel**:
One of the five groups Whoop sells as Specialized Panels: Heart, Performance,
Metabolic, Women's and Men's Health. Together they make up Extended. We keep
Whoop's name because it matches how Whoop sells them.
_Avoid_: specialized block, extended block

**Provider**:
A Romanian lab network that sells Tests. There are three: Synevo, Regina Maria
and MedLife.

### Money and coverage

**Covered / Uncovered / Unresolved**:
The three states a Biomarker can have at a Provider. Covered: some Test measures
it, and the name map names that Test. Uncovered: the Provider sells nothing that
measures it, shown as an em dash. Unresolved: we couldn't find out, shown as
`?`. Unresolved Biomarkers are left out of the Common Set and listed by name, so
a guess never ends up looking like a fact.

**Subscriber Price**:
What a Test costs someone who holds that Provider's subscription. It comes in
two grades that must never be mixed up. *Guaranteed*: a fixed annual set, no
Recommendation needed, once a year. *Estimated*: a discount annex that needs a
Recommendation, which is up to the doctor. Always quoted with its grade. It sits
next to the standard price in the Provider's shopping list, never instead of it.
_Avoid_: discounted price, member price

**Prevention Route**:
The free annual screening an insured adult can get from their family doctor
without being ill. It needs no diagnosis and no symptoms, and it's only for
people who aren't registered with a chronic disease and who have a modifiable
risk factor. It gives a fixed list of Tests at 0 RON, depending on age and sex.
The list is a maximum, not an entitlement: the doctor decides which of it to
write.
_Avoid_: CNAS price, free tests, state discount

**Diagnostic Route**:
The same state funding, claimed on a Referral that carries an ICD-10 diagnosis
code. It reaches more Biomarkers than the Prevention Route, but the lab is only
paid up to its monthly contract limit.

**Referral**:
The *bilet de trimitere* a contracted family doctor writes, listing the Tests
the state will pay for. It's what the user is *entitled to*. The lab can't add
to it or swap anything on it, so you get exactly what the doctor wrote.
_Avoid_: prescription, trimitere, order, recommendation

**Recommendation**:
The *recomandarea medicului* a Provider's own doctor gives, which unlocks that
Provider's discount annex. It's up to the doctor and can't be enforced, which is
why every price that depends on one is graded estimated. It's never a Referral:
it doesn't entitle you to anything.
_Avoid_: referral, prescription

**Common Set**:
The Core Biomarkers that every Provider Covers. Provider totals are quoted over
it so the comparison is like for like.

**Exclusive**:
A Biomarker only one Provider Covers. It's priced and listed apart from the
Common Set total, never added into it.

**Basket**:
The cheapest set of Tests that covers every Core Biomarker a Provider can Cover.
It's calculated, not looked up, so recalculate it whenever a price changes: a
Panel that gets cheaper can beat buying its parts and change the answer without
anything looking wrong.
_Avoid_: cart, order, selection

**Block**:
A group of Tests in a shopping list, with its own subtotal and Biomarker count,
so a user can drop a whole group and see what they save and what they lose.
Core Blocks are clinical themes. Extended's Blocks are the five Specialized
Panels.
_Avoid_: category, group, section

**Shared Biomarker**:
A Biomarker that appears in more than one Specialized Panel, such as Leptin,
Magnesium, Free T3/T4 or the omega panel. Each Block prices it as if bought for
that Block alone, so Shared Biomarkers are marked `†`, and two Specialized Panels
together cost less than their subtotals add up to.
