# 2. Only the prevention route is covered

Date: 2026-08-05

## Status

Accepted.

## Context

Romania's public health insurance pays for lab tests on a *bilet de trimitere*
from a family doctor. Adding this to the repo looked like one feature. It's
actually two, and they differ in what the reader has to say about their own
health.

**The diagnostic route.** The tariff annex to the implementing rules lists 108
tests, each with a footnote saying who may order it. Footnote `*1` marks the
roughly 55 a family doctor can order; the rest need a specialist. Every sex
hormone, cortisol, PTH, ATPO, the immunoglobulins, reticulocytes, LDH and serum
bicarbonate need a specialist. What a family doctor can order covers **39 of the
57 Core biomarkers** and 8 Extended ones, worth 501–675 RON off a Core basket.
But the referral carries an ICD-10 code for a known or suspected diagnosis, and
the lab is only paid up to its monthly contract limit. That limit is why labs
tell patients "the funds have run out, come back on the 1st".

**The prevention route.** An insured adult who isn't registered with a chronic
disease and has a modifiable risk factor can get an annual preventive check-up
and, with it, a referral for a fixed list of tests that needs no diagnosis. It
covers **27 of the 57 Core biomarkers**: the blood count, fasting glucose,
total cholesterol, LDL, creatinine, AST and ALT, plus TSH and free T4 for women
from 40, and PSA for men from 50. That's worth 149–182 RON, or 205–250 RON for
women from 40. Men get no other Core biomarker at any age. Importantly, the lab
is paid for these **outside** its monthly limit, so it can't legally refuse
because the funds have run out.

The prevention route saves about a third as much as the diagnostic route. It
was tempting to document both and let the reader choose.

## Decision

Cover only the prevention route.

No product file marks, prices or describes the diagnostic route. The fuller
analysis of the diagnostic route is kept in private notes. The public sources
for the prevention route, and how to maintain it, are in
`docs/agents/refresh.md`.

Keep it minimal: a `§` marker on the Test lines it covers and one explanatory
section in each shopping list. No fourth price column, no new file, and no
change to any Basket or total. Every figure in the repo still means *what you
pay without a referral*.

## Consequences

**The repo doesn't tell anyone to get a diagnosis they don't have.** Getting the
diagnostic route's 12 extra Core biomarkers would mean a doctor writing a
diagnosis code for tests you want out of curiosity. Publishing that as a way to
save money would mean advising readers to misuse a public system at the expense
of people who actually need it. This is the real reason for the decision; the
rest supports it.

**One marker, not two.** Like the subscriber annex, `§` depends on a doctor: the
prevention list is a maximum, not something you can order from, and the
shopping lists say so. The difference is what the doctor is deciding on. Here
it's a published entitlement set in law and paid outside the lab's monthly
limit, not a private discount with unpublished rules. A second marker would give
every `§` line the same grade, so the condition is explained once in prose
instead.

**The repo understates what you can get.** A reader who really does have a
diagnosis can get more than `§` shows, and nothing here tells them so. We accept
that cost.

**Some Baskets are deliberately not the cheapest on this route.** With total
cholesterol and LDL free, Regina Maria's 85 RON lipid panel costs more than
buying HDL and triglycerides separately for 65. With fasting glucose free,
Synevo's 82 RON `Indice HOMA` costs more than buying insulin alone for 65.
Instead of keeping a separate Basket for this route, each shopping list keeps
the panel and describes the swap in one sentence. Recomputing Baskets for a
route the reader may not use would give every total two meanings. (When this was
decided, only Regina Maria was affected. Synevo joined once its `Indice HOMA`
turned out to be cheaper than its parts without a referral.)

**A refresh has a new way to go wrong.** The prevention lists are in the
implementing rules, not the tariff annex. The framework contract
(Contract-cadru) describes the check-up but doesn't list any tests. The lists can
change independently of any provider's prices, and nothing else in this repo
would catch it. A CNAS draft already proposes adding HDL cholesterol, which
would mark a new line at Synevo and MedLife. It wouldn't change Regina Maria's
swap: triglycerides would still not be free, so the panel would still cost more,
by a wider margin than today.
