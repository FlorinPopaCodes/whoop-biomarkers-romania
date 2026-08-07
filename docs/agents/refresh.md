# The refresh sweep

The quarterly re-verification, step by step. Sections here match the numbered
checklist in `CLAUDE.md`; work in that order, because each step consumes the one
before it.

The three providers need completely different handling. **Do not treat them
symmetrically.**

## 1. Has Whoop's list moved?

The set is 127: 75 Core (Comprehensive Health Panel) plus 52 Extended (the five
Specialized Panels). Whoop's marketing says "122+" and its prose implies 51
beyond Core; the gap is that the marketing list omits Blood Fasting Glucose,
which the FAQ counts. Verified 2026-08-04 — 100 purchasable (57 Core, 43
Extended), 27 Derived.

Check the [Advanced Labs
FAQ](https://support.whoop.com/s/article/Advanced-Labs-FAQ?language=en_US) — it
lists the Comprehensive panel and prints a "Last Published Date", which makes
drift cheap to detect. Specialized panels are at
`…/Advanced-Labs-Specialized-Panels?language=en_US`.

`https://www.whoop.com/us/en/advanced-labs/` carries the full supported list but
is Cloudflare-blocked to scripted fetches; use a `web.archive.org` capture, where
the Next.js payload holds the list as JSON.

**Two traps in Whoop's own data.** Their per-panel totals don't reconcile — they
say each Specialized Panel contains the Comprehensive 75, but the arithmetic
implies five different cores, none of them 75. And their CPT table contains typos
(`Plasma Osmolaity`, `Alkaline Phosotase`) and one garbled row that is actually
DHEA Sulfate. Normalize to whoop.com's Title Case spellings.

## 2–3. Re-fetch the catalogues; re-check every price and name

None of the three sites has bot protection, a login wall, or a JS requirement for
prices. None publishes a usable PDF price list — third-party ones are stale B2B
rates.

### Regina Maria — one request gets everything

```
https://www.reginamaria.ro/laboratoare-inteligente/gama-de-analize
```

Server-renders the entire 1,083-test catalogue in ~1 MB of HTML: no login, no
pagination, no JS. Each row carries machine-readable attributes:

```html
<div class="add-analysis" data-drupal-investigation="155290"
     data-drupal-price="45.00 Lei"
     data-drupal-investigation-name="Acid uric urinar">
```

Parse those three attributes and you have the whole price list. **Ignore the
page's JSON-LD `ItemList`** — it truncates at 20 items.

Prices are București (the default with no parameters). Checked against
Cluj-Napoca: of 843 shared tests only 6 differ, all placeholder values on genetic
panels. Treat prices as national; *availability* is what varies by city (1,083
tests in București against 856 in Cluj).

Deep links: ~241 tests have clean dictionary URLs at
`/utile/dictionar-de-analize/<slug>`. For the rest use
`…/gama-de-analize?city=6951&location=6805&investigation_category=All&investigation=<id>`,
where `<id>` is `data-drupal-investigation`. These are for your verification
only — the shopping list doesn't link RM tests, see `file-shapes.md`.

### Synevo — one request per test

Slugs enumerate from `https://www.synevo.ro/sitemap_index.xml` (2,354 `/shop/`
products; it is one flat urlset despite the name). Then fetch
`https://www.synevo.ro/shop/<slug>/` and read the JSON-LD `offers.price`. The
JSON-LD `sku` is a stable `CH…` code worth recording.

**Each Synevo product page carries two different names** — the `<title>` and the
`<h1>`/JSON-LD `name`, e.g. "Cupru in plasma" against "Cupru în sânge" for one
product. **Take the `<title>` form**: it is the catalogue-listing name, and it is
what the shopping-list links already use. Using both is how `BIOMARKERS.md` and
the shopping list came to disagree on two rows.

The on-site A-Z filter is Livewire and does not respond to query params, so the
sitemap is the only bulk route. One national price, no region selector.

### MedLife — one request gets everything

```
https://www.medlife.ro/gama-analize
```

Server-renders the entire 2,031-test catalogue in ~1.8 MB of HTML on a plain GET:
no parameters, no login, no pagination, no JS. Each row carries machine-readable
attributes, contrary to how it first looks — there is no JSON-LD and no
`data-drupal-*` naming, but the same shape exists under different names:

```html
<li class="option" data-name="1,25 Dihidroxi Vitamina D3"
    data-price="278 lei" data-id="23024768">
```

inside `<ul id="servicii-wrapper">`. **MedLife spells some tests with `s` where
Synevo and Regina Maria use `z`** — its serum cortisol is `Cortisol seric`, not
`Cortizol`. Searching with the other two providers' spelling is how this repo
came to claim for a while that MedLife sold no cortisol at all. Match on a stem,
never a whole word.

Parse `data-name`, `data-price` and `data-id`. `data-id` is a stable per-test numeric ID (2,031 unique values for
2,031 rows, one-to-one) worth recording the way Synevo's `CH…` SKU and Regina
Maria's `data-drupal-investigation` are. There is no per-test deep link — neither
a Synevo-style slug nor a Regina Maria-style dictionary URL exists.

The page shows a locality selector, a specific-lab selector, and a 27-category
filter, all defaulting to "București" / "Laborator MedLife Grivița" / "Toate
categoriile" in the markup. **Verified 2026-08-05 that these are decorative for
scraping purposes**: submitting `field_localitate_target_id` as a GET query
param, and separately POSTing the exposed Drupal form
(`medlife_servicii_laborator_gama_analize`) with a different locality, both left
the row count and every sampled price byte-identical to the untouched default,
and the response still echoed the same hardcoded selections regardless of what
was submitted — the filtering that select implies happens client-side (the page
ships React) over data that's already all there. **Prices are national and the
default fetch is exhaustive; there is nothing to iterate by city, lab, or
category.**

## 4. Re-derive the `§` set

`§` marks Tests obtainable free on a prevention referral from a family doctor.
Unlike the subscriber columns this is **national** — the same entitlement at all
three providers — so the set is derived once, at the level of *biomarkers*, and
applied three times. Only the prevention route is modelled;
`docs/adr/0002-only-the-prevention-route.md` says why the larger diagnostic route
is deliberately absent.

**The set as derived 2026-08-05:** the 20-parameter blood count, fasting glucose,
total cholesterol, LDL, creatinine, AST and ALT for everyone from 18; TSH and
free T4 for women from 40; PSA for men from 50, once every three years. **TSH is
women-only** — the norms read "TSH şi FT4 la femei", where `la femei` scopes both,
so a man gains no further Core biomarker at any age. Re-stamp this date when the
set is re-derived, not when prices move.

### Finding the lists

**They are in two different documents, and only one is the one you want.** The
tariff annex — Anexa 17 to Ordin MS/CNAS 1857/441/2023 — is the 108
investigations the state funds *at all*, with reimbursed tariffs. **Reading it
alone will overstate `§` by twelve Core biomarkers**, because it includes
everything a *specialist* can order too.

The prevention lists are elsewhere. **HG 521/2023, anexa 1, pct. 1.2.3 defines
the consultation but carries no test list** — that exists solely in the norms,
Ordin MS/CNAS 1857/441/2023, at anexa 1 pct. 1.2.3 and anexa 2 pct. 1.2.3, which
agree. Use CNAS's own consolidation, currently

```
https://cnas.ro/wp-content/uploads/2026/01/ALL-ORDIN-Nr.-1857_441_2023-Partea-I.pdf
```

legislatie.just.ro's consolidated forms are subscriber-gated.

### Three traps when reading them

- **The tariff PDF uses a shifted font subset.** `pdftotext` returns mojibake.
  Every byte is offset by −29: add 29 to each character to decode. Layout padding
  spaces come back as `=`, and digits arrive as control bytes in `0x13`–`0x1C`.
  Decode before concluding anything about its contents.
- **In the tariff annex, footnote `*1` is the family-doctor gate.** There is no
  "family doctor" column — the marker system in NOTA 1 is the whole mechanism.
  Unmarked means specialist-only. This is what keeps cortisol, all sex hormones,
  ATPO, PTH, the immunoglobulins and reticulocytes off the list.
- **A third gate sits in the same passage and is easy to miss.** The annual
  bloods go to "adulţii asimptomatici, **cu factori de risc modificabili**", and
  only "pentru investigaţiile paraclinice apreciate a fi necesare de către medicul
  de familie". Risk factors condition the tests, not just the consultation.

### Two lines that look like they qualify and don't

- **CRP hs** — the prevention list carries plain C-reactive protein, a different
  assay from the high-sensitivity one this repo maps to Whoop's hs-CRP.
- **The reticulocyte-inclusive hemogram** — the state covers the 20-parameter
  count only.

### Expressing it

`§` marks a Test, so a provider whose Basket buys a Panel mixing funded and
unfunded biomarkers **cannot mark it**; there the explainer must name the funded
members in prose instead. There are currently two such cases — Regina Maria's
`Profil lipidic`, where Total Cholesterol and LDL are funded and HDL and
Triglycerides are not, and Synevo's `Indice HOMA`, where fasting glucose is
funded and insulin is not. Both therefore carry 25 `§` biomarkers against
MedLife's 27. **That is the expected shape, not a gap to close.**

**`§` and `↑` don't compose**, which the Synevo and Regina Maria explainers say
out loud. A referral is a closed set the lab may not add to, so the upgrade
cannot be bought on top of a funded blood count: a reader wanting reticulocytes
buys the full hemogram (75 Synevo, 70 RM) and forfeits the free one. MedLife is
immune — its reticulocyte count is a standalone Test. Keep this in prose; don't
try to express it in a marker.

**Two Blocks are knowingly non-optimal on this route.** At Regina Maria, total
cholesterol and LDL come free, leaving only HDL (35) and Trigliceride (30) — 65
against `Profil lipidic`'s 85. At Synevo, fasting glucose comes free, leaving
only Insulina (65) against `Indice HOMA`'s 82. Both Baskets keep the panel and
both explainers name the swap in prose, so that every total in the repo keeps
meaning *what you pay walking in cold*. **Don't "fix" either by forking the
Basket.**

Regina Maria also publishes its CAS-settled catalogue at
`/laboratoare-inteligente/gama-de-analize-decontabile`, using the same
`data-drupal-*` markup as its main catalogue — 114 rows, all at `0 Lei`. Useful
for cross-checking names against the annex, but it is the *whole* covered set,
not the prevention subset, so **never mark `§` straight from it**.

## 5. Re-derive the subscriber columns

Each subscriber column needs its own pass, not a re-derivation from the standard
prices beside it. Re-check each provider's discount annex itself — it can change
independently of that provider's public prices, and nothing else in this repo
would catch that — then re-derive which lines are `●`, which are `○`, and which
stay at full price.

Both shopping lists intentionally **never mention the subscription's own monthly
cost**. The column is a per-test price list for someone who already holds the
subscription, not a cost/benefit case for buying one.

### Regina Maria — Comfort Premium

**The mechanic.** Comfort Premium gives two separate things, the same shape as
Respect Infinit below.

An **annual set** of 11 tests — Test Papanicolau clasic/PSA, Sumar de urină,
Glicemie, LDL colesterol, HDL colesterol, Trigliceride, Hemoleucogramă, VSH,
Transaminaze (TGO, TGP), Creatinină serică, Colesterol total — is included with
**no Recommendation**, capped **1×/year each**; the limit applies from first use,
not from contract signing. These are the `●` lines. It is the same eleven as
Respect Infinit's annual set, line for line.

A much larger **discount annex** — about 300 lab tests at 100% off — is *only*
"la recomandarea medicului RM". Enforcement is a GP's individual call and isn't
published anywhere, so this repo doesn't model it as guaranteed. Every
annex-covered line is `○` (estimated), never asserted as a hard price.

The column exists because the discount is large enough — roughly half of RM's
Core list — to be worth showing, not because it's a price every reader gets.

**Six shopping-list lines take the annual set's `●`:** the blood count, Glucoza
serica (as "Glicemie"), both transaminases, Creatinina serica, and PSA (read as
Pap smear for women / PSA for men, the same reading as MedLife's combined line).
All six are annex-covered too, so `●` is an upgrade in certainty, not in price —
**the annual set moved no RON figure in this file.** The remaining six annual-set
entries have no line to grade: Sumar de urină and VSH are not Whoop biomarkers,
and total cholesterol, LDL, HDL and triglycerides arrive inside `Profil lipidic`,
which is annex-covered only. That last one is a genuine reader choice — buying the
four separately is 0 and guaranteed against the panel's 0 and estimated — so the
shopping list names it in one sentence, the same way the `§` swap is named.

**`↑` does not compose with the subscriber column the way it does with the
standard one.** The reticulocyte hemogram is on neither list, so a subscriber buys
it at its full 70 — but their Core hemogram cost 0, not 60. The upgrade's
difference is therefore **10 standard and 70 subscriber**, and Performance
Health's subscriber subtotal carries 70. Don't copy the 10 across on the grounds
that the two columns should look alike.

**Finding the current annex.** The live terms PDF is linked from
`https://shop.reginamaria.ro/abonamentul-comfort-premium-adulti.html` as a
dynamically-served download (`Anexa includeri abonament.pdf`). **That page**, not
a dated static PDF under `reginamaria.ro/sites/default/files/`, is the one to
check each refresh, since Regina Maria revises the PDF in place without changing
its URL. Check the PDF's `ModDate` against your last refresh to see whether it
changed at all before re-deriving anything; it was `2024-05-27` as of the
2026-08-07 check.

The page carries no literal `.pdf` href — the download is a Magento endpoint,
`https://shop.reginamaria.ro/catalog/product/downloadattachment/product_id/70/`,
which serves `application/pdf` on a plain GET. Grepping the page for `.pdf` finds
nothing; grep for `downloadattachment`.

**Matching annex names to shopping-list lines is manual and literal.** The annex
names tests generically ("Testosteron total", "PCR (Proteina C reactivă) test
cantitativ") while the shopping list sells RM's specific SKUs. **Do not assume a
generic annex name covers a more specific SKU.** Three known ambiguous cases,
currently all kept at full price rather than assumed covered:

- **Proteina C reactiva inalt sensibila (HSCRP)** — the annex lists generic
  "PCR", not explicitly the high-sensitivity assay.
- **Free PSA** — the annex lists generic "PSA", not explicitly free PSA.
- **The reticulocyte-inclusive hemogram upgrade** — the annex says
  "Hemoleucogramă completă" generically, not specifically that SKU.

Re-check these three by name against the current annex text every refresh; Regina
Maria could make any of them explicit in either direction.

### MedLife — Respect Infinit

The counterpart to Comfort Premium, with the same never-mention-the-monthly-cost
rule and the same `○` caveat for anything gated on a doctor's say-so.

**The mechanic.** Respect Infinit (539 RON/month, 12-month validity) gives two
separate things, unlike Comfort Premium's single annex.

An **annual set** of 11 tests — Papanicolau clasic/PSA, Sumar de urină, Glicemie,
LDL colesterol, HDL colesterol, Trigliceride, Hemoleucogramă, VSH, Transaminaze
(TGO, TGP), Creatinină serică, Colesterol total — is included with **no
Recommendation**, capped **1×/year each**; the limit applies from first use, not
from contract signing. These are the `●` lines.

A much larger **discount annex** — about 19 test categories (Biochimie,
Hematologie, Markeri endocrini, Markeri cardiovasculari, Imunologie, Coagulare,
Electroforeză, Biologie moleculară, Anatomie patologică, Markeri tumorali,
Markeri osoși, Markeri hepatici, Markeri infecțioși, Markeri alergii,
Microbiologie, Bacteriologie, Toxicologie, Parazitologie, Screening prenatal) —
is 100% off but needs "cu recomandarea medicului MedLife", the same enforcement
uncertainty as Comfort Premium's annex. These are the `○` lines.

Unlike Comfort Premium, Respect Infinit's annex caps each test at **4×/year** ("1
analiză/trimestru"). Comfort Premium's annex has no equivalent per-test annual
cap, so **don't copy that caveat wording across the two files**.

**Finding the current annex.**

```
https://www.medlife.ro/documente_publice/abonamente_individuale/2024/Abonament_individual_MedLife_Respect_Infinit.pdf
```

Bilingual RO/EN, revised in place at a stable URL despite the `/2024/` path
segment (`ModDate` was `2025-07-14` as of the 2026-08-05 check). Check `ModDate`
against your last refresh before re-deriving anything, same discipline as Comfort
Premium's PDF.

**Matching is manual and literal**, same discipline as above.

*Confirmed absent from the annex, kept at full price without further comment:*
Cortisol, Estradiol, FSH, Insulin, DHEA Sulfate (also unplaced, so it has no line
to grade), and "Testosteron liber" — only plain "Testosteron" is listed. The
`Markeri endocrini` category is an enumerated list, not a catch-all, and cortisol
is not on it; "17 OH Corticosteroizi" is a different assay.

*Ambiguous, kept at full price rather than assumed covered:*

- **Free T4** and **Free T3** — the annex lists generic "T4"/"T3", not explicitly
  the free-hormone assays.
- **Ureea nitrogen (BUN)** — the annex lists generic "Uree serica", not
  explicitly BUN.
- **CRP hs** — the annex lists generic "CRP cantitativ", not explicitly the
  high-sensitivity assay. The same trap as Comfort Premium's HSCRP line.

*Resolved matches, kept covered despite non-identical wording — re-check all
three by name every refresh:*

- **Glucoza serica** is covered on the strength of the annex's Biochimie category
  alone, which lists it verbatim. It's also plausibly the annual set's "Glicemie"
  line — the colloquial Romanian term for the same fasting-glucose draw, not a
  different assay — which would make it doubly covered, but that reading isn't
  load-bearing for the verdict.
- **Ag. specific prostatic (PSA)** is covered on the strength of the annex's
  Markeri tumorali category alone, which lists it verbatim. It's also plausibly
  the annual set's combined "Test Papanicolau clasic / PSA" line, read as Pap
  smear for women / PSA for men — again not load-bearing, just the reason it's
  graded `●` rather than `○`.
- **Ac Anti-Tireoperoxidaza (ATPO)** — the annex's Imunologie category spells it
  "Ac Anti-Tireoperoxidaza (TPO)". Same antibody test, not a scope difference.

**"Free PSA" is not the same trap it is for Comfort Premium.** MedLife's Markeri
tumorali category lists "Ag. specific prostatic (PSA)" and "Free PSA" as two
separate, literal lines — both unambiguous, covered matches here. **Don't import
Comfort Premium's caution onto this file's Free PSA line.**

## 6. Confirm panel membership

Optimizer input for the next step. Verify these still hold; expect them to change
far less often than prices do.

| Provider | Panel test | RON | Biomarkers it yields |
|---|---|---:|---|
| Synevo | Hemograma cu formula leucocitara, Hb,Ht,indici si reticulocite (Hemograma) | 75 | 21: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), Reticulocyte Count (RET), White Blood Cells (WBC) |
| Synevo | Hemograma cu formula leucocitara cu Hb, Ht si indici | 44 | 20: as above, minus Reticulocyte Count (RET) |
| Synevo | Acizi grasi omega 3 si omega 6 | 502 | 4: Arachidonic Acid (AA), Docosahexaenoic Acid (DHA), Eicosapentaenoic Acid (EPA), Linoleic Acid (LA) |
| Synevo | Glucoza serica (glicemie) | 21 | 2: Blood Fasting Glucose, Glucose |
| Synevo | Indice HOMA | 82 | 3: Blood Fasting Glucose, Glucose, Insulin — the product page says outright it *conține Glucoză serică și Insulină* |
| Regina Maria | Hemoleucograma completa cu formula leucocitara, Hb, Ht, indici si reticulocite | 70 | 21: the same 21 as Synevo's reticulocyte hemogram |
| Regina Maria | Hemoleucograma cu formula leucocitara,Hb,Ht, indici eritrocitari | 60 | 20: as above, minus Reticulocyte Count (RET) |
| Regina Maria | Profil lipidic | 85 | 4: HDL Cholesterol, LDL Cholesterol, Total Cholesterol, Triglycerides |
| Regina Maria | Glucoza serica | 30 | 2: Blood Fasting Glucose, Glucose |
| Regina Maria | Profil LDL (LDL colesterol, sd-LDL colesterol, LDL oxidat, lipoproteina A) | 400 | 2: LDL Cholesterol, LDL Small |
| MedLife | Hemoleucograma completa | 51 | 20: the 21 above, minus Reticulocyte Count (RET) |
| MedLife | Glucoza serica | 21 | 2: Blood Fasting Glucose, Glucose |
| MedLife | Acizi grasi omega 3 si omega 6 | 512 | 4: the same four as Synevo's — same name, yield **assumed** identical; MedLife has no per-test deep link to verify the composition directly |

**The other two providers sell an `Indice HOMA` too, and neither is in a Basket.**
MedLife's (85) loses to its own parts (21 + 61 = 82). Regina Maria's (90) would
win against its parts (30 + 70 = 100), but RM publishes no per-test page, so
whether it reports the two input assays rather than the ratio alone is
unverified. Confirm that before putting it in RM's Basket; it is worth 10 RON.

The 21-biomarker blood count, in full: Basophil %, Basophils, Eosinophil %,
Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular
Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean
Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes,
Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell
Distribution Width (RDW), Reticulocyte Count (RET), White Blood Cells (WBC).

## 7. Recompute every Basket

The Basket is the minimum-cost set of Tests covering every Core biomarker a
provider can cover. **It is an optimizer output, not data** — see invariant 2 in
`CLAUDE.md` for why this is the one way the repo goes quietly wrong.

For each provider, choose the set of Tests of least total cost whose union covers
every covered Core biomarker. The only real decisions are panel-versus-parts, so
compare each panel's price against the sum of the individual Tests it replaces,
using the membership table above.

Recompute from scratch, all three, after **any** price change — including a price
change that arrives from a subscriber annex or the `§` set rather than from a
catalogue.

## 8–10. Write it back

The Basket determines the `SHOPPING-LIST-*.md` contents directly — there's no
separate step. But a price change that flips a panel-versus-parts decision has to
be reflected in **both** the chosen Basket *and* which lines appear in the
shopping list.

Then recompute every Block subtotal and coverage count, copy README's four
numbers per provider (see `file-shapes.md`), and re-stamp the dates per
invariant 10.
