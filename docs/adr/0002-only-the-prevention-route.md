# 2. Only the prevention route is modelled

Date: 2026-08-05

## Status

Accepted.

## Context

Romania's public insurer funds laboratory analyses on a *bilet de trimitere*
from a family doctor. Adding this to the repo looked like one feature. It is
two, and they differ in what the reader has to claim about their own health.

**The diagnostic route.** The tariff annex to the implementing norms lists 108
investigations, each footnoted for who may prescribe it. Footnote `*1` marks the
~55 a family doctor can order; the unmarked remainder needs a specialist. Every
sex hormone, cortisol, PTH, ATPO, the immunoglobulins, reticulocytes, LDH and
serum bicarbonate are specialist-only. What a family doctor can reach maps onto
**39 of 57 Core biomarkers** and 8 Extended — worth 501–675 RON off a Core
basket. But the referral carries an ICD-10 code for a known or presumed
diagnosis, and it is settled only up to the lab's monthly contract value, which
is the whole reason for the "funds are exhausted, come back on the 1st" problem.

**The prevention route.** An insured adult not registered with a chronic disease,
carrying a modifiable risk factor, can claim an annual preventive consultation
and, with it, a referral for a fixed list requiring no diagnosis at all. It maps
onto **27 of 57 Core** — the blood count, fasting glucose, total cholesterol,
LDL, creatinine, AST, ALT — plus TSH and free T4 for women 40+, and PSA for men
from 50. Worth 170–182 RON, or 226–250 RON for women from 40; a man gains no
further Core biomarker at any age. Crucially it is settled **above** the monthly
ceiling, so exhausted funds are not a lawful refusal.

The saving is a third of the diagnostic route's. The temptation was to document
both and let the reader choose.

## Decision

Model the prevention route only.

The diagnostic route is not marked, not priced, and not described in any product
file. It is recorded in `docs/research/cnas-medic-de-familie.md` so the analysis
isn't lost.

Expression is deliberately minimal: a `§` marker on affected Test lines plus one
explainer section per shopping list. No fourth price column, no new file, and no
change to any Basket or total — every figure in the repo continues to mean *what
you pay walking in cold*.

## Consequences

**The repo doesn't tell anyone to get a diagnosis they don't have.** Reaching
the diagnostic route's extra 12 Core biomarkers means a doctor writing a
diagnosis code for tests wanted cosmetically. Publishing that as a cost-saving
tip would be advising readers to misuse a public system on behalf of people who
do need it. This is the actual reason for the decision; the rest is
corroboration.

**One marker, not two.** `§` is gated on a doctor, like the subscriber annex —
the prevention list is a ceiling, not an order form, and the explainer says so.
What differs is what the doctor is declining: a published entitlement fixed in
law and settled above the lab's monthly ceiling, rather than an unpublished
commercial discount whose enforcement nobody documents. A second glyph would
grade all 27 lines identically, so the condition is carried once in prose
instead.

**The repo understates what is obtainable.** A reader who genuinely has a
diagnosis can get more than `§` suggests, and nothing here tells them so. That
is the cost, and it is accepted.

**One Basket is knowingly left non-optimal.** With total cholesterol and LDL
free, Regina Maria's 85 RON lipid panel loses to buying HDL and triglycerides
separately for 65. Rather than fork the Basket per route, the shopping list
keeps the panel and the explainer names the swap in one sentence. Recomputing
Baskets against a route the reader may not take would give every total two
meanings.

**A refresh has a new failure mode.** The prevention lists live in the norms, not
the tariff annex — the Contract-cadru itself defines the consultation but carries
no test list — and they can move independently of any provider's prices, a change
nothing else in this repo would catch. There is already a CNAS draft in
transparency proposing to add HDL cholesterol, which would mark a new line at
Synevo and MedLife. It would not retire Regina Maria's swap: triglycerides stay
unfunded, so the panel would still lose, by a wider margin than today.
