# Maintaining this repo

`README.md`, `BIOMARKERS.md`, `SHOPPING-LIST-SYNEVO.md`,
`SHOPPING-LIST-REGINA-MARIA.md` and `SHOPPING-LIST-MEDLIFE.md` together are
the product. `BIOMARKERS.md` is the source of truth for every price; the
three `SHOPPING-LIST-*.md` files are the source of truth for what to
actually order; `README.md` is a thin landing page — intro, provider links,
the headline Core verdict, and pointers to the other files. There are no
scripts. Everything here is what you need to refresh it correctly.

`SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` and
`SUBSCRIPTION-MEDLIFE-RESPECT-INFINIT.md` are two more files, deliberately
outside "the product": each prices one provider's subscription tier for a
subscriber, which isn't a price every reader of this repo gets. See their
own sections below.

Read `CONTEXT.md` first — it defines Solo Price, Common Set, Basket, Block and
the rest of the vocabulary used below.

## Refresh cadence

Every ~3 months. Re-verify **every** price at all three providers, not just
the gaps: a table mixing fresh and stale cells under one date lies about half
its contents. Re-stamp the date at the bottom of `BIOMARKERS.md`, all three
`SHOPPING-LIST-*.md` files, and `README.md` only when the whole sweep is
done — all five files share prices, so they share a date.

**Exception: onboarding a new provider.** Adding a provider's first column/file
(as MedLife's Core column and shopping list were) isn't a refresh sweep — the
existing providers' cells are genuinely untouched, not stale, so re-stamping
them under the new provider's date would be the same lie in reverse. Until the
next full sweep unifies them, a footer may carry one date per provider instead
of one shared date. The next full sweep collapses it back to a single date.

After recomputing the Common Set totals in `BIOMARKERS.md`, copy the same two
numbers into `README.md`'s teaser under `## Biomarkers`. The teaser's
`Subscriber (estimated)` column also has to stay in sync — copy Regina
Maria's and MedLife's headline Core totals from
`SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` and
`SUBSCRIPTION-MEDLIFE-RESPECT-INFINIT.md` on the same sweep, right after
each subscription file's own pass (below) recomputes them. Nothing enforces
any of these copies; forgetting one is the fast way for the files to
quietly disagree.

The Basket recompute (see below) determines the `SHOPPING-LIST-*.md` contents
directly — there's no separate step, but a price change that flips a
panel-versus-parts decision has to be reflected in both the chosen Basket
*and* which lines are ticked-off-able in the shopping list.

**Both `SUBSCRIPTION-*.md` files need their own pass**, not just a copy from
their shopping list: re-check each provider's discount annex itself (it can
change independently of that provider's public prices, and nothing else in
this repo would catch that), then re-derive which lines are struck through.
See each file's own section below for how to find its annex.

## The two deliverables

**Comparison tables** (`BIOMARKERS.md`) — split into Core and Extended.

- *Core* is one flat table, alphabetical by Whoop biomarker, 7 columns: Whoop
  Biomarker · Synevo test · RON · Regina Maria test · RON · MedLife test · RON.
  Plain text, no links.
- *Extended* is the same shape, 7 columns, grouped into subsections by Whoop's
  five Specialized Panels (see Blocks below). A biomarker shared by more than one
  panel repeats under each, marked `†` — this is deliberate: it keeps every
  panel's row count matching its own coverage count in the shopping list, at
  the cost of the Extended table being longer than a flat list would be.
- Each half is followed by its own short Derived list (Name · Formula, 2
  columns, no price columns — see Pricing rules) instead of interleaving
  Derived rows into the price table.
- **DHEA Sulfate is unplaced.** It's Extended, priced at all three providers, but
  isn't part of any of Whoop's five Specialized Panels — checked directly
  against Whoop's panel marketing, not inferred. It currently sits in its own
  one-line "not yet placed" note rather than being folded into a topically-close
  panel; doing that would make that panel's row count disagree with its own
  shopping-list coverage count. Revisit if Whoop ever documents where it belongs.

Together they answer *which provider do I pick*. `README.md` keeps only the
Common Set totals as a teaser, linking to `BIOMARKERS.md` for the row-by-row
breakdown.

**Shopping list** (`SHOPPING-LIST-SYNEVO.md`, `SHOPPING-LIST-REGINA-MARIA.md`,
`SHOPPING-LIST-MEDLIFE.md`) — one file per provider, each grouped into Blocks
with checkboxes, per-item prices, subtotals and biomarker counts. Test names
are hyperlinked here and nowhere else — except MedLife's, which has no
per-test deep links to hyperlink to (see "Looking prices up" below). Together
they answer *what do I actually order*. Split per provider rather than kept
as one file because a reader shops at one provider at a time, not all of them
at once.

## Pricing rules

**Solo Price.** Every price cell is what that biomarker costs *alone* by the
cheapest route. Sold individually → its own price. Obtainable only inside a
panel → the whole panel's price. So MCV shows the full hemogram price.

**Never sum the price column.** Sixteen biomarkers come out of one hemogram; a
column sum counts it sixteen times. The totals under the table come from the
Basket optimization, not from the column.

**Derived biomarkers are 0 RON at all three providers**, and live in their own Name ·
Formula list (Core Derived, Extended Derived) rather than the price tables —
a 0/0 row carries no comparison information, so it doesn't belong there. Derived
means *no Romanian lab sells it and it must be computed from other biomarkers on
this list*. That is not Whoop's definition —
Whoop marks LDL Cholesterol, TIBC and ALP as "Calculated" but Romanian labs sell
all three as line items, so they are purchasable here. (Whoop's ALP row is
simply an error; it is an enzyme assay.)

Some providers *do* sell a derived value directly — Regina Maria lists "Indice
HOMA". Never put that price in the table and never put it in a Basket: the
Basket already contains its inputs, so buying it is pure waste. Derived stays 0.

**Three states, kept distinct.** A price means covered. `—` means the provider
genuinely sells nothing that yields it. `?` means undetermined. Never render an
undetermined cell as `—`: unresolved biomarkers are excluded from the Common Set
and named explicitly under the totals, and quietly demoting one to "unavailable"
shifts every provider's totals on evidence you don't have.

## Totals

Quote the head-to-head over the **Common Set** — the Core biomarkers all
providers cover — so the comparison is like-for-like. List each provider's
exclusives separately with their prices. Never fold an exclusive into the
comparable total.

## The Basket, and the trap

The Basket is the minimum-cost set of tests covering every Core biomarker a
provider can cover. **It is an optimizer output, not data.**

This is the one way this repo goes quietly wrong. Drop Synevo's lipid profile
from 90 to 60 RON and the right answer changes — buying the panel now beats
buying its four parts — but nothing in the README looks broken. So:

> **After changing any price, recompute every Basket from scratch.**

To recompute: for each provider, choose the set of tests of least total cost
whose union covers every covered Core biomarker. The only real decisions are
panel-versus-parts, so compare each panel's price against the sum of the
individual tests it replaces. Panel membership is below and changes far more
slowly than prices do.

## Blocks

**Core Blocks** are clinical themes: CBC, lipids, metabolic, liver, kidney,
iron, electrolytes, protein, thyroid, hormones, inflammation, vitamins.

**Extended Blocks are Whoop's five Specialized Panels** — Heart Health,
Performance Health, Metabolic Health, Women's Health, Men's Health. Keep these
names and this grouping; it mirrors Whoop's own product structure.

Extended Blocks overlap: Leptin appears in three, and Magnesium, B12, Folate,
Free T3/T4, Prolactin, Uric Acid, Zinc and the omega panel each appear in two or
more. Price each Block **standalone** — the cost of taking that Block on top of
Core — mark shared items `†`, and keep the one line saying you pay for them once.
Block subtotals deliberately do not add up.

## Looking prices up

The three providers need completely different handling. Do not treat them
symmetrically.

**Regina Maria — one request gets everything.**

```
https://www.reginamaria.ro/laboratoare-inteligente/gama-de-analize
```

Server-renders the entire 1,083-test catalogue in ~1 MB of HTML, no login, no
pagination, no JS. Each row carries machine-readable attributes:

```html
<div class="add-analysis" data-drupal-investigation="155290"
     data-drupal-price="45.00 Lei"
     data-drupal-investigation-name="Acid uric urinar">
```

Parse those three attributes and you have the whole price list. Ignore the
page's JSON-LD `ItemList` — it truncates at 20 items.

Prices are București (the default with no parameters). Checked against
Cluj-Napoca: of 843 shared tests only 6 differ, all placeholder values on genetic
panels. Treat prices as national; *availability* is what varies by city (1,083
tests in București vs 856 in Cluj).

Deep links: ~241 tests have clean dictionary URLs at
`/utile/dictionar-de-analize/<slug>`. For the rest use
`…/gama-de-analize?city=6951&location=6805&investigation_category=All&investigation=<id>`,
where `<id>` is `data-drupal-investigation`.

**Synevo — one request per test.**

Slugs enumerate from `https://www.synevo.ro/sitemap_index.xml` (2,354 `/shop/`
products; it is one flat urlset despite the name). Then fetch
`https://www.synevo.ro/shop/<slug>/` and read the JSON-LD `offers.price`. The
JSON-LD `sku` is a stable `CH…` code worth recording. The on-site A-Z filter is
Livewire and does not respond to query params, so the sitemap is the only bulk
route. One national price, no region selector.

**MedLife — one request gets everything.**

```
https://www.medlife.ro/gama-analize
```

Server-renders the entire 2,031-test catalogue in ~1.8 MB of HTML on a plain
GET with no parameters, no login, no pagination, no JS. Each row carries
machine-readable attributes, contrary to how it first looks — there is no
JSON-LD and no `data-drupal-*` naming, but the same shape exists under
different names:

```html
<li class="option" data-name="1,25 Dihidroxi Vitamina D3"
    data-price="278 lei" data-id="23024768">
```

inside `<ul id="servicii-wrapper">`. Parse `data-name`, `data-price` and
`data-id`. `data-id` is a stable per-test numeric ID (2,031 unique values for
2,031 rows, one-to-one) worth recording the way Synevo's `CH…` SKU and Regina
Maria's `data-drupal-investigation` are. There is no per-test deep link —
neither a Synevo-style slug nor a Regina Maria-style dictionary URL exists on
this page.

The page also shows a locality selector, a specific-lab selector, and a
27-category filter, all defaulting to "București" / "Laborator MedLife
Grivița" / "Toate categoriile" in the markup. Verified 2026-08-05 that these
are decorative for scraping purposes: submitting `field_localitate_target_id`
as a GET query param, and separately POSTing the exposed Drupal form
(`medlife_servicii_laborator_gama_analize`) with a different locality, both
left the row count and every sampled price byte-identical to the untouched
default, and the response still echoed the same hardcoded selections
regardless of what was submitted — the filtering that select implies happens
client-side (the page ships React) over data that's already all there.
**Prices are national and the default fetch is exhaustive; there is nothing
to iterate by city, lab, or category.**

None of the three sites has bot protection, a login wall, or a JS requirement
for prices. None publishes a usable PDF price list — third-party ones are
stale B2B rates.

## Subscription pricing (Regina Maria Comfort Premium)

`SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md` is not one of "the two
deliverables" above — it prices Regina Maria for someone holding a Comfort
Premium subscription specifically, which most readers don't have. It exists
because the discount is large enough (roughly half of Regina Maria's Core
list) to be worth showing, not because it's a universal price.

**The mechanic.** Comfort Premium gives two things: a free annual screening
panel (~11 tests, no referral, capped at 1×/year each), and a much larger
discount annex — about 300 lab tests at 100% off, but *only* "la
recomandarea medicului RM" (with a referral from an RM doctor). Enforcement
of that referral is a GP's individual call and isn't published anywhere, so
this repo doesn't model it as guaranteed. Every annex-covered line in the
subscription file is marked **(estimated)**, not asserted as a hard price —
this is the one file in the repo where "Covered" doesn't mean "buy it for
this price, guaranteed."

**Finding the current annex.** The live terms PDF is linked from
`https://shop.reginamaria.ro/abonamentul-comfort-premium-adulti.html` as a
dynamically-served download (`Anexa includeri abonament.pdf`) — that page,
not a dated static PDF under `reginamaria.ro/sites/default/files/`, is the
one to check each refresh, since Regina Maria revises the PDF in place
without changing its URL. Check the PDF's `ModDate` against your last refresh
to see whether it changed at all before re-deriving anything.

**Matching annex test names to shopping-list lines is manual and literal.**
The annex names tests generically (e.g. "Testosteron total", "PCR (Proteina
C reactivă) test cantitativ") while the shopping list sells RM's specific
SKUs (e.g. "Free PSA" vs plain "PSA"). Do not assume a generic annex name
covers a more specific SKU — three known ambiguous cases, currently all kept
at full price rather than assumed covered:
- "Proteina C reactiva inalt sensibila (HSCRP)" — the annex lists generic
  "PCR", not explicitly the high-sensitivity assay.
- "Free PSA" — the annex lists generic "PSA", not explicitly free PSA.
- The reticulocyte-inclusive hemogram upgrade — the annex says
  "Hemoleucogramă completă" generically, not specifically the
  reticulocyte-inclusive SKU.

Re-check these three by name against the current annex text on every
refresh; Regina Maria could make any of them explicit in either direction.

This file intentionally never mentions the subscription's own monthly
cost — it's a per-test price list for someone who already has the
subscription, not a cost/benefit case for buying one.

## Subscription pricing (MedLife Respect Infinit)

`SUBSCRIPTION-MEDLIFE-RESPECT-INFINIT.md` is the MedLife counterpart to the
Comfort Premium file above — same "not one of the two deliverables" framing,
same never-mention-the-monthly-cost rule, same **(estimated)** caveat for
anything gated on a doctor's say-so.

**The mechanic.** Respect Infinit (539 RON/month, 12-month validity) gives
two separate things, unlike Comfort Premium's single discount annex. An
**annual set** of 11 tests — Papanicolau clasic/PSA, Sumar de urină,
Glicemie, LDL colesterol, HDL colesterol, Trigliceride, Hemoleucogramă, VSH,
Transaminaze (TGO, TGP), Creatinină serică, Colesterol total — is included
with **no referral**, capped **1×/year each**; the limit applies from first
use, not from contract signing. A much larger **discount annex** — about 19
test categories (Biochimie, Hematologie, Markeri endocrini, Markeri
cardiovasculari, Imunologie, Coagulare, Electroforeză, Biologie moleculară,
Anatomie patologică, Markeri tumorali, Markeri osoși, Markeri hepatici,
Markeri infecțioși, Markeri alergii, Microbiologie, Bacteriologie,
Toxicologie, Parazitologie, Screening prenatal) — is 100% off but needs "cu
recomandarea medicului MedLife" (a MedLife doctor's referral), the same
enforcement uncertainty as Comfort Premium's annex. Unlike Comfort Premium,
Respect Infinit's annex caps each test at **4×/year** ("1 analiză/trimestru,"
one per quarter) — Comfort Premium's annex has no equivalent per-test annual
cap, so don't copy that caveat wording across the two files.

**Finding the current annex.** The terms PDF is at
`https://www.medlife.ro/documente_publice/abonamente_individuale/2024/Abonament_individual_MedLife_Respect_Infinit.pdf`
— bilingual RO/EN, revised in place at a stable URL despite the `/2024/` path
segment (`ModDate` was `2025-07-14` as of the 2026-08-05 check). Check
`ModDate` against your last refresh before re-deriving anything, same
discipline as Comfort Premium's PDF.

**Matching annex test names to shopping-list lines is manual and literal**,
same discipline as Comfort Premium's — see that section above. Known gaps
confirmed absent from the annex, kept at full price without further comment:
Cortisol (MedLife doesn't sell it at all), Estradiol, FSH, Insulin, DHEA
Sulfate (also unplaced — see "The two deliverables" above — so it never gets
a line in the subscription file either), and "Testosteron liber" (free
testosterone; only plain "Testosteron" is listed). Known ambiguous cases,
currently kept at full price rather than assumed covered:
- "Free T4" and "Free T3" — the annex lists generic "T4"/"T3", not
  explicitly the free-hormone assays.
- "Ureea nitrogen (BUN)" — the annex lists generic "Uree serica" (serum
  urea), not explicitly BUN.
- "CRP hs" — the annex lists generic "CRP cantitativ", not explicitly the
  high-sensitivity assay — the same trap as Comfort Premium's HSCRP line.

Two resolved matches, kept covered despite non-identical wording — re-check
both by name against the current annex text on every refresh:
- "Glucoza serica" is covered on the strength of the annex's Biochimie
  category alone, which lists it verbatim. It's also plausibly the annual
  set's "Glicemie" line — the colloquial Romanian term for the same
  fasting-glucose draw, not a different assay — which would make it doubly
  covered, but that reading isn't load-bearing for the "covered" verdict.
- "Ag. specific prostatic (PSA)" is covered on the strength of the annex's
  Markeri tumorali category alone, which lists it verbatim. It's also
  plausibly the annual set's combined "Test Papanicolau clasic / PSA" line,
  read as Pap smear for women / PSA for men — again not load-bearing for the
  "covered" verdict, just the reason it gets the simpler no-referral caveat
  in the subscription file.
- "Ac Anti-Tireoperoxidaza (ATPO)" — the annex's Imunologie category spells
  it "Ac Anti-Tireoperoxidaza (TPO)", same antibody test, not a scope
  difference.

**"Free PSA" is not the same trap it is for Comfort Premium.** Regina
Maria's annex only lists generic "PSA", which is why Free PSA is one of
*its* three known ambiguous cases (see above). MedLife's Markeri tumorali
category lists "Ag. specific prostatic (PSA)" and "Free PSA" as two
separate, literal lines — both are unambiguous, covered matches here. Don't
import Comfort Premium's caution onto this file's Free PSA line.

## Panel membership

Optimizer input. Verify these still hold when refreshing, but expect them to
change far less often than prices.

| Provider | Panel test | RON | Biomarkers it yields |
|---|---|---:|---|
| Synevo | Hemograma cu formula leucocitara, Hb,Ht,indici si reticulocite (Hemograma) | 75 | 21: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), Reticulocyte Count (RET), White Blood Cells (WBC) |
| Synevo | Hemograma cu formula leucocitara cu Hb, Ht si indici | 44 | 20: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), White Blood Cells (WBC) |
| Synevo | Acizi grasi omega 3 si omega 6 | 502 | 4: Arachidonic Acid (AA), Docosahexaenoic Acid (DHA), Eicosapentaenoic Acid (EPA), Linoleic Acid (LA) |
| Synevo | Glucoza serica (glicemie) | 21 | 2: Blood Fasting Glucose, Glucose |
| Regina Maria | Hemoleucograma completa cu formula leucocitara, Hb, Ht, indici si reticulocite | 70 | 21: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), Reticulocyte Count (RET), White Blood Cells (WBC) |
| Regina Maria | Hemoleucograma cu formula leucocitara,Hb,Ht, indici eritrocitari | 60 | 20: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), White Blood Cells (WBC) |
| Regina Maria | Profil lipidic | 85 | 4: HDL Cholesterol, LDL Cholesterol, Total Cholesterol, Triglycerides |
| Regina Maria | Glucoza serica | 30 | 2: Blood Fasting Glucose, Glucose |
| Regina Maria | Profil LDL (LDL colesterol, sd-LDL colesterol, LDL oxidat, lipoproteina A) | 400 | 2: LDL Cholesterol, LDL Small |
| MedLife | Hemoleucograma completa | 51 | 20: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), White Blood Cells (WBC) |
| MedLife | Acizi grasi omega 3 si omega 6 | 512 | 4: Arachidonic Acid (AA), Docosahexaenoic Acid (DHA), Eicosapentaenoic Acid (EPA), Linoleic Acid (LA) — same name and yield assumed identical to Synevo's identically-named panel; MedLife has no per-test deep link to verify the composition directly |

## Checking whether Whoop's list moved

The biomarker set is 127: 75 Core (Comprehensive Health Panel) plus 52 Extended
(the five Specialized Panels). Whoop's marketing says "122+" and its prose implies
51 beyond Core; the gap is that the marketing list omits Blood Fasting Glucose,
which the FAQ counts. Verified 2026-08-04 — 100 purchasable, 27 Derived.

Check `https://support.whoop.com/s/article/Advanced-Labs-FAQ?language=en_US` —
it lists the Comprehensive panel and prints a "Last Published Date", which makes
drift cheap to detect. Specialized panels are at
`…/Advanced-Labs-Specialized-Panels?language=en_US`.

`https://www.whoop.com/us/en/advanced-labs/` carries the full supported list but
is Cloudflare-blocked to scripted fetches; use a `web.archive.org` capture, where
the Next.js payload holds the list as JSON.

Two known traps in Whoop's own data. Their per-panel totals don't reconcile —
they say each Specialized Panel contains the Comprehensive 75, but the arithmetic
implies five different cores, none of them 75. And their CPT table contains
typos (`Plasma Osmolaity`, `Alkaline Phosotase`) and one garbled row that is
actually DHEA Sulfate. Normalize to whoop.com's Title Case spellings.

## Style

Spartan. Two deliverables split across five files (`BIOMARKERS.md` for the
comparison tables, `SHOPPING-LIST-SYNEVO.md`, `SHOPPING-LIST-REGINA-MARIA.md`
and `SHOPPING-LIST-MEDLIFE.md` for the shopping list, `README.md` as a thin
landing page linking to all three), plus `SUBSCRIPTION-REGINA-MARIA-COMFORT-PREMIUM.md`
and `SUBSCRIPTION-MEDLIFE-RESPECT-INFINIT.md` as two clearly-separate files
for subscriber-only pricing. Each file carries its own two-line provider
block or back-link, one-line disclaimer, and one-line licence. No
table of contents, no emoji headers, no medical essay. If you are adding a
new section to any of these files, check first whether it belongs in this
file instead — agent-facing detail lives here, not in any product file.

## Agent skills

### Issue tracker

Issues and PRDs live as GitHub issues in this repo (`gh` CLI). See `docs/agents/issue-tracker.md`.

### Domain docs

Single-context layout — `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
