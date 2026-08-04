# Maintaining this repo

`README.md` is the product and the only source of truth for prices. There are no
scripts. Everything here is what you need to refresh it correctly.

Read `CONTEXT.md` first — it defines Solo Price, Common Set, Basket, Block and
the rest of the vocabulary used below.

## Refresh cadence

Every ~3 months. Re-verify **every** price at both providers, not just the gaps:
a table mixing fresh and stale cells under one date lies about half its contents.
Re-stamp the date at the bottom of `README.md` only when the whole sweep is done.

## The two deliverables

**Comparison table** — one table, alphabetical by Whoop biomarker, 5 columns:
Whoop Biomarker · Synevo test · RON · Regina Maria test · RON. Plain text, no
links. It answers *which provider do I pick*.

**Shopping list** — per provider, grouped into Blocks with checkboxes, per-item
prices, subtotals and biomarker counts. Test names are hyperlinked here and
nowhere else. It answers *what do I actually order*.

## Pricing rules

**Solo Price.** Every price cell is what that biomarker costs *alone* by the
cheapest route. Sold individually → its own price. Obtainable only inside a
panel → the whole panel's price. So MCV shows the full hemogram price.

**Never sum the price column.** Sixteen biomarkers come out of one hemogram; a
column sum counts it sixteen times. The totals under the table come from the
Basket optimization, not from the column.

**Derived biomarkers are 0 RON at both providers**, and carry their formula in
the Whoop Biomarker cell. Derived means *no Romanian lab sells it and it must be
computed from other biomarkers on this list*. That is not Whoop's definition —
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
shifts *both* providers' totals on evidence you don't have.

## Totals

Quote the head-to-head over the **Common Set** — the Core biomarkers both
providers cover — so the comparison is like-for-like. List each provider's
exclusives separately with their prices. Never fold an exclusive into the
comparable total.

## The Basket, and the trap

The Basket is the minimum-cost set of tests covering every Core biomarker a
provider can cover. **It is an optimizer output, not data.**

This is the one way this repo goes quietly wrong. Drop Synevo's lipid profile
from 90 to 60 RON and the right answer changes — buying the panel now beats
buying its four parts — but nothing in the README looks broken. So:

> **After changing any price, recompute both Baskets from scratch.**

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

The two providers need completely different handling. Do not treat them
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

Neither site has bot protection, a login wall, or a JS requirement for prices.
Neither publishes a usable PDF price list — third-party ones are stale B2B rates.

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

Spartan. Two deliverables, a two-line provider block, a one-line disclaimer, a
one-line licence. No table of contents, no emoji headers, no medical essay. If
you are adding a third section, check first whether it belongs in this file
instead — agent-facing detail lives here, not in the README.
