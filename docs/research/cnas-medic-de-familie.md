# State-funded analyses via the medic de familie — research findings
The prevention route has since been folded into the product as the `§` marker — see `docs/adr/0002-only-the-prevention-route.md`. Everything else here, the diagnostic route above all, remains research only. Verified 2026-08-05; the prevention list re-verified against primary sources 2026-08-05, see below.
## The headline
There is no single "free via CNAS" answer. There are **two different routes** with different gates, different yields, and — critically — different exposure to the monthly budget ceiling. Conflating them is the main way a reader would be misled.

|     | Prevention route | Diagnostic route |
| --- | --- | --- |
| What it needs | Nothing but being asymptomatic and due | A diagnosis or presumptive diagnosis (ICD-10 code) on the referral |
| Whoop Core biomarkers it yields | **27 of 57** | **39 of 57** |
| Whoop Extended it yields | 0–2 (FT4 for women 40+; PSA for men 50+) | 8   |
| How often | **Once per calendar year** | Per clinical episode; no annual cap found |
| Referral validity | 60 calendar days | 30 days (acute), up to 90 (chronic) |
| Monthly ceiling | **Exempt — settled above the contract value** | **Fully exposed** |

The prevention route is the one that actually fits this repo's reader — someone with no symptoms who wants numbers. It yields less, but it is the only route that doesn't depend on a doctor agreeing you might be ill, and it is immune to the fund-exhaustion problem.

One eligibility trap: the asymptomatic-adult route is explicitly for people **not** on their family doctor's register with any chronic disease. If you are, you fall under a different, similar list instead — so being healthier is what qualifies you here.
## The mechanic
An insured person gets a **bilet de trimitere pentru investigații paraclinice** from a family doctor who is themselves under contract with a county insurance house. The form is serialised, issued in two copies, and carries an ICD-10 diagnosis code plus a category tick — `A/S` acute, `C` chronic, `M` case management, `G` pregnancy, `PREV` prevention, and the oncology/hepatitis codes.

Hard rules worth knowing:

- The lab **may not add or substitute** investigations on the referral. What the doctor wrote is what you get.
  
- The lab **may not charge** for anything inside the package.
  
- If the lab can't run the tests the day you show up, it must schedule you inside the referral's validity, or **hand the referral back** so you can go elsewhere. This is the only patient-side protection with teeth.
  
- Being hospitalised between issue and execution voids the referral.
  

Governing documents: **HG 521/2023** (Contract-cadru) and **Ordin MS/CNAS 1857/441/2023** (norms), whose **Anexa 17** holds the list. Referral form and filling rules are **Ordin MS/CNAS 2168/502/2023**. The older `Ordin 1068/627/2021` naming that circulates in blog copy is superseded.
## The list, and who may prescribe from it
Anexa 17 lit. A pct. 1 is **108 laboratory investigations**, each with a reimbursed tariff in RON. It is a text-layer PDF — no HTML, CSV or API — and the font is a shifted subset, so naive text extraction returns mojibake.

There is no "family doctor" column. The gate is a **footnote marker system**:

| Marker | Meaning | Count |
|---|---|---|
| `*1` | May also be recommended by family doctors | **~55–58 of 108** |
| `*8` | Family doctor only with `management de caz` on the referral | 4 |
| `*9` | Family doctor only for children 2–5 | 1 |
| `*10` | Family doctor only with the prevention field filled, for adults 18+ who are overweight/obese or carry a diabetes risk factor, and only if not done in the last 6 months | 2 |
| `*3 *4 *5 *6` | Run by the lab reflexively, no referral at all | 4 |
| no marker | **Specialist referral only** | ~40 |

The specialist-only bucket is what breaks the naive reading. **Every sex hormone, cortisol, PTH, ATPO, the immunoglobulins, reticulocytes, LDH, lipase, amylase and serum bicarbonate are outside a family doctor's reach.** So are albumin (case-management only) and phosphorus (children only). HbA1c is family-doctor-reachable but only via the diabetes-risk prevention marker, not on a plain referral.

Cross-checked two ways: an independent decode of the annex PDF and a separate documentary trace both land on the same conclusion; they differ by three rows on the exact `*1` count, which does not move any biomarker in or out.
## What the two routes actually yield
### Prevention route — 27 of 57 Core
For asymptomatic adults 18–39 the annual preventive list is: complete blood count, CRP, fasting glucose, total cholesterol, LDL cholesterol, creatinine with eGFR, AST, ALT, and VDRL/RPR. Adults 40+ add urinary albumin/creatinine ratio; women add TSH and FT4; men add PSA from 50, once every three years.

**The list lives only in the norms, and TSH is women-only.** HG 521/2023 anexa 1 pct. 1.2.3 defines the consultation but carries no test list; that exists solely in Ordin MS/CNAS 1857/441/2023, in anexa 1 pct. 1.2.3 and anexa 2 pct. 1.2.3, which agree. The relevant lines, from CNAS's January 2026 consolidation, pct. 1.2.3.b ("pentru adultul asimptomatic cu vârsta de 40 de ani şi peste"):

> — TSH şi FT4 la femei
> — pentru bărbaţi, PSA, începând cu vârsta de 50 de ani, o dată la 3 ani.

`la femei` scopes the coordinated pair, and the wording is identical in the original Monitorul Oficial 484 bis/31.V.2023 publication and the August 2024 consolidation. The drafters write a bare `• TSH` where they mean no sex restriction — the pediatric dismetabolic lists in the same document do exactly that. The 18–39 list carries no TSH at all, so **TSH is never claimable by a man on this route, at any age.** Sources: `https://cnas.ro/wp-content/uploads/2026/01/ALL-ORDIN-Nr.-1857_441_2023-Partea-I.pdf` and `https://cnas.ro/wp-content/uploads/2024/08/ordin-1857.pdf`.

**A second gate, not previously modelled.** The same passage conditions the annual bloods on risk, not merely on the consultation: "adulţii asimptomatici, **cu factori de risc modificabili**, beneficiază anual de investigaţii paraclinice […] doar pentru investigaţiile paraclinice apreciate a fi necesare de către medicul de familie". A reader with no modifiable risk factor may get the consultation and none of the tests.

Mapped onto Whoop: the blood count alone is 20 Core biomarkers, plus fasting glucose and glucose, total cholesterol, LDL, creatinine, AST and ALT — **27**. Women 40+ add TSH (28 Core) and FT4 (1 Extended). Men gain no further Core biomarker at any age — what they add is the urinary albumin/creatinine ratio at 40 and PSA (Extended) at 50, neither of which moves the Core count.

Two things a reader would trip on. **HDL cholesterol is not on the preventive list** — total and LDL are, HDL isn't, which also strands several Derived ratios that need it. CNAS published a draft in July 2026 proposing to add it; not in force. And the plain **CRP on the list is not the high-sensitivity assay** this repo maps to Whoop's hs-CRP.

The doctor also gets to decide which of the list is "necessary", so even 27 is a ceiling rather than an entitlement. The consultation itself is capped too: two preventive consultations a year at 18–39, one to three at 40+.

There is a **third route worth knowing**, and it is the only one that doesn't require a preventive consultation at all. The referral carries a `PREV` code naming the population — `4` asymptomatic 18–39, `5` asymptomatic 40+ — and two of those codes are opportunistic: `8` for adults who are overweight/obese or carry a diabetes risk factor, and `9` for chronic-kidney-disease risk. Both can be issued **with the occasion of any other consultation**, on a **six-month** cadence rather than annual. `PREV 8` is how HbA1c and the oral glucose tolerance test become reachable without a specialist — which matters, because they are otherwise closed to a family doctor.
### Diagnostic route — 39 of 57 Core, 8 Extended
With a diagnosis code, the `*1` set opens up. Beyond the prevention list this adds HDL, triglycerides, ALP, sodium, potassium, calcium, iron, ferritin, urea, total bilirubin, total protein and TSH — plus 8 Extended (uric acid, creatine kinase, folate, FT4, magnesium, B12, GGT, total PSA).

Eight Core biomarkers are on the annex but out of a family doctor's reach: albumin, bicarbonate, cortisol, estradiol, FSH, LH, testosterone, HbA1c.

Ten Core biomarkers are not on the annex at all, at any referrer: ApoB, chloride, free testosterone, hs-CRP, homocysteine, insulin, Lp(a), SHBG, TIBC and vitamin D.
## What it saves
Taking the diagnostic route's 39 free and pricing only the remainder:

| Provider | Core basket today | Remainder after CNAS | Saved |
| --- | ---: | ---: | ---: |
| Synevo | 1,860 RON | 1,359 RON | 501 RON |
| Regina Maria | 2,290 RON | 1,615 RON | 675 RON |
| MedLife | 1,858 RON | 1,329 RON | 529 RON |

Roughly 27–30% off, not half — because the free set is exactly the cheap commodity chemistry, while what survives is vitamin D, homocysteine, Lp(a), ApoB and the hormones. The provider ranking does not flip: like-for-like (excluding cortisol, which MedLife doesn't sell) Synevo stays ahead of MedLife, but the margin narrows from 61 RON to about 33.

On the prevention route the saving is smaller again, since it is essentially the blood count plus six cheap chemistries.
## The ceiling, and the exemption that matters
Each lab's contract value is split by quarter and by month, and the county house settles only up to that monthly value. That is the whole mechanism behind "funds are exhausted, come back on the 1st" — there is no written rule telling patients to wait; it is a consequence of the cap.

The exemption is the important part. Referrals marked `PREV 1`**–**`PREV 7`, `Monitor 2`–`Monitor 8`, `SO`, `AO`, `G` or `HS` are settled **at realised level, above the contract value** — funded from the National Cancer Plan line of the health budget rather than the lab's monthly allocation. So on the prevention route, "the plafon is exhausted" is not a lawful refusal, and Synevo's own rules confirm it operationally: a referral coded `Prevenție` (or `AO`, `SO`, `HS`, `G`) is walk-in, no booking, no monthly window. On an ordinary `A/S` or `C` referral, "come back on the 1st" effectively is the answer.

**You never pay a difference.** A contracted lab may not charge the insured for anything inside the package, including the draw itself, even though the reimbursed tariff is far below its list price — a complete blood count settles at 14.62 RON against a 44–75 RON shelf price. A statutory power to charge a personal contribution for outpatient paraclinics exists but has never been switched on for lab analyses, so a lab asking you to top up today has no basis.

Two caveats. The redistribution of unspent sums and the ±10% monthly overrun appear to be **suspended** while an austerity ordinance applies, and whether that is still active was not confirmed. And CNAS's July 2026 draft proposes mid-month ceiling monitoring as an "interim measure until ceilings are eliminated" — a signal the mechanic may change, but nothing adopted.
## Provider reality
All three hold CNAS lab contracts, but contracts are **per county house**, and what they publish differs sharply.

**Synevo** is the only one that publishes, per county and refreshed monthly, the exact date booking opens and when collections start — the first-of-month rush made explicit. It documents a waiting list when funds run out, valid only for that month. Booking is mandatory and the original referral must be presented, except for referrals coded `AO`, `SO`, `HS`, `G` or `Prevenție`, which can walk in. No CAS collections at weekends. Named footprint: 15 counties including București and Ilfov.

**Regina Maria** publishes the most useful artifact: a CAS-settled catalogue at `/laboratoare-inteligente/gama-de-analize-decontabile`, using the same `data-drupal-*` markup as its main catalogue. Independently fetched and confirmed — **114 investigations, every one priced** `0 Lei`, invariant to city parameters. That is the covered subset expressed in the provider's own SKU names, which is what makes it directly joinable to this repo. It also states plainly that if funds are exhausted you may wait for next month or pay privately, and that there is no per-patient annual cap — the limit is the clinic's, not yours. Its own FAQ claims national CAS coverage while its locations directory lists ~24 cities; treat the FAQ line as marketing.

**MedLife** publishes the least: no dedicated CAS page, no list of which collection points accept CAS referrals in any city, and CAS bookings by telephone only. Its published covered-list PDF is from 2021 and is missing items that are on the current annex. It carries one real MedLife-specific constraint — if a referral line needs a urine or stool sample and you arrive without it, that line is lost and cannot be done on a later visit under the same referral.
## Where this collides with the repo's existing model
- **The Basket optimizer gets a new input.** Panel-versus-parts flips when the parts are free: Regina Maria's 85 RON lipid panel is pointless when total cholesterol, HDL, LDL and triglycerides are individually covered. Every Basket would need recomputing, which is exactly the trap `CLAUDE.md` warns about.
  
- **The hemogram downgrade.** The annex covers the 20-parameter blood count, not the reticulocyte-inclusive version, and reticulocytes are specialist-only. So the free route yields the cheaper hemogram and Reticulocyte Count stays paid.
  
- **TIBC has a back door.** TIBC itself is not on the annex, but transferrin is, and is family-doctor-reachable. Whether that counts as covering TIBC is a modelling decision, not a fact.
  
- **"Covered" would mean a third thing.** The repo currently has Covered / Uncovered / Unresolved, plus the subscriber column's guaranteed-versus- estimated split. State funding is neither: it is conditional on a referral someone else decides to write, on which route it is written under, and on money that runs out mid-month.
  
## Open questions
- **Can you hand over a referral and pay for extras in the same blood draw?** Unresolved, and it is the question that decides whether this is one trip or two. Nothing forbids it — the referral is a closed set the lab may not add to, so any extra is by construction a separate private order — but no regulator and none of the three providers says it can be done at one appointment. This needs a phone call to a specific branch, not more desk research. The prevention referral is the most promising vehicle, since it is walk-in at Synevo rather than tied to a monthly booking window.
  
- Whether the austerity suspension of the redistribution mechanism is still active.
  
- Whether MedLife's actual covered set matches the current annex or its stale 2021 PDF.
  
- Whether a family doctor will in practice write a `PREV 8` referral for someone who simply wants the numbers — the marker requires being overweight/obese or carrying a diabetes risk factor, which is a judgement, not a checkbox.
