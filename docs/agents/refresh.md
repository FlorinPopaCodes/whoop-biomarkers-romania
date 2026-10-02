# The refresh

How to re-check everything, step by step. The sections follow the numbered list
in `CLAUDE.md`. Work in that order, because each step uses the result of the one
before.

Each provider needs a different approach. **Don't assume what works for one
works for the others.**

## 1. Has Whoop's list changed?

There are 127 biomarkers: 75 Core (the Comprehensive Health Panel) and 52
Extended (the five Specialized Panels). Whoop's marketing says "122+" and its
text suggests 51 outside Core. The difference is Blood Fasting Glucose: the
marketing list leaves it out, but the FAQ counts it. Checked 2026-08-04: 100
purchasable (57 Core, 43 Extended) and 27 Derived.

Whoop's panel pages, checked 2026-10-02, still show 75 Core biomarkers and the
same five Specialized Panels. They only list a few key biomarkers in plain HTML,
so the full list of 127 names is still dated 2026-08-04.

Start with the [Advanced Labs
FAQ](https://support.whoop.com/s/article/Advanced-Labs-FAQ?language=en_US). It
lists the Comprehensive panel and shows a "Last Published Date", which makes
changes easy to spot. The Specialized Panels are at
`…/Advanced-Labs-Specialized-Panels?language=en_US`.

`https://www.whoop.com/us/en/advanced-labs/` has the full supported list, but
Cloudflare blocks scripted requests. Use a `web.archive.org` copy instead, where
the Next.js data contains the list as JSON.

**Whoop's own data has two problems.** First, its per-panel totals don't add up:
it says every Specialized Panel includes the Comprehensive 75, but the numbers
imply five different Core sets, none of them 75. Second, its CPT table has typos
(`Plasma Osmolaity`, `Alkaline Phosotase`) and one garbled row that is actually
DHEA Sulfate. Use the Title Case spellings from whoop.com.

## 2–3. Fetch the catalogues again; re-check every price and name

None of the three sites blocks bots, needs a login, or needs JavaScript to show
prices. None publishes a usable PDF price list; the ones on third-party sites
are old business rates.

### Regina Maria: one request gets everything

```
https://www.reginamaria.ro/laboratoare-inteligente/gama-de-analize
```

The server returns the whole catalogue of 1,305 tests in about 1 MB of HTML
(2026-10-02), with no login, no pagination and no JavaScript. Each row has
machine-readable attributes:

```html
<div class="add-analysis" data-drupal-investigation="155290"
     data-drupal-price="45.00 Lei"
     data-drupal-investigation-name="Acid uric urinar">
```

Read those three attributes and you have the full price list. **Use the
attributes, not the page's JSON-LD `ItemList`**, which stops after 20 items.

Prices are for București, the default when you pass no parameters. Compared with
Cluj-Napoca, only 6 of 843 shared tests differ, all placeholder prices on genetic
panels. Treat prices as national. What changes by city is which tests are
available (1,083 in București, 856 in Cluj).

Links: about 241 tests have clean dictionary URLs at
`/utile/dictionar-de-analize/<slug>`. For the others, use
`…/gama-de-analize?city=6951&location=6805&investigation_category=All&investigation=<id>`,
where `<id>` is `data-drupal-investigation`. Use these links only to check
things yourself. The shopping list doesn't link RM tests; see `file-shapes.md`.

### Synevo: one request per test

Get the product slugs from `https://www.synevo.ro/sitemap_index.xml` (2,375
`/shop/` products; despite the name it's a single flat list). Then fetch
`https://www.synevo.ro/shop/<slug>/` and read the price from the JSON-LD
`offers.price`. The JSON-LD `sku` is a stable `CH…` code, worth recording.

**Each Synevo product page has two names**: the `<title>`, and the `<h1>` /
JSON-LD `name`. For example, one product is "Cupru in plasma" in one and "Cupru
în sânge" in the other. **Use the `<title>` name.** It's the name shown in the
catalogue listing, and the shopping-list links already use it. Mixing the two
is how `BIOMARKERS.md` and the shopping list once disagreed on two rows.

The A–Z filter on the site uses Livewire and ignores URL parameters, so the
sitemap is the only way to get everything. There is one national price and no
region selector.

### MedLife: one request gets everything

```
https://www.medlife.ro/gama-analize
```

The server returns the whole catalogue of 2,096 tests in about 1.9 MB of HTML on
a plain GET (2026-10-02), with no parameters, login, pagination or JavaScript.
It doesn't look like it at first, since there's no JSON-LD and no
`data-drupal-*`, but each row does have machine-readable attributes under other
names:

```html
<li class="option" data-name="1,25 Dihidroxi Vitamina D3"
    data-price="278 lei" data-id="23024768">
```

They sit inside `<ul id="servicii-wrapper">`. **MedLife spells some tests with
`s` where Synevo and Regina Maria use `z`**: its serum cortisol is `Cortisol
seric`, not `Cortizol`. Searching with the other providers' spelling once led
this repo to say MedLife didn't sell cortisol at all. Search by word stem, not
the whole word.

Read `data-name`, `data-price` and `data-id`. `data-id` is a stable numeric ID
for each test (2,096 unique values for 2,096 rows as of 2026-10-02), worth
recording like Synevo's `CH…` code and Regina Maria's
`data-drupal-investigation`. MedLife has no per-test pages, so there's nothing
to link to.

The page has a city selector, a lab selector and a filter with 27 categories,
which default to "București", "Laborator MedLife Grivița" and "Toate
categoriile". **On 2026-08-05 we confirmed these make no difference when
scraping.** Sending `field_localitate_target_id` as a URL parameter, and
separately submitting the page's Drupal form
(`medlife_servicii_laborator_gama_analize`) with another city, both returned the
same number of rows and exactly the same prices as the default. The response
also showed the same default selections whatever was sent. The filtering happens
in the browser (the page uses React) over data that's already all there.
**Prices are national and the default request gets everything. There's no need
to loop over cities, labs or categories.**

## 4. Re-check the `§` list

`§` marks Tests you can get free on a prevention referral from a family doctor.
Unlike the subscription columns, this is **national**: the same at all three
providers. So work out the list once, by *biomarker*, and apply it to all three
files. Only the prevention route is covered;
`docs/adr/0002-only-the-prevention-route.md` explains why the larger diagnostic
route is left out.

**The list, as checked on 2026-10-02:** for everyone from 18, the 20-value blood
count, fasting glucose, total cholesterol, LDL, creatinine, AST and ALT; for
women from 40, TSH and free T4; for men from 50, PSA once every three years.
**TSH is for women only.** The rules say "TSH şi FT4 la femei", and `la femei`
applies to both, so men get no other Core biomarker at any age. Update this date
when you re-check the list, not when prices change.

### Finding the lists

**There are two documents, and you need the second one.** The first is the
tariff annex, Anexa 17 to Ordin MS/CNAS 1857/441/2023. It lists all 108 tests
the state pays for in any situation, with their prices. **Reading it alone gives
twelve more Core biomarkers than the prevention route allows**, because it
includes everything a *specialist* can order.

The prevention lists are elsewhere. **HG 521/2023, anexa 1, pct. 1.2.3 describes
the check-up but has no list of tests.** The list is only in the implementing
rules, Ordin MS/CNAS 1857/441/2023, at anexa 1 pct. 1.2.3 and anexa 2 pct. 1.2.3,
which say the same thing. Use CNAS's own consolidated version, currently:

```
https://cnas.ro/wp-content/uploads/2026/01/ALL-ORDIN-Nr.-1857_441_2023-Partea-I.pdf
```

The [consolidated rules on the Legislative
Portal](https://legislatie.just.ro/Public/DetaliiDocumentAfis/305898) were also
checked on 2026-10-02. Their latest consolidation is dated 2026-01-01, and the
prevention list still doesn't include HDL or triglycerides.

### Three things to watch for when reading them

- **The tariff PDF uses a shifted font.** `pdftotext` returns garbage. Each byte
  is shifted by −29, so add 29 to each character to decode it. Padding spaces
  come out as `=`, and digits come out as control bytes from `0x13` to `0x1C`.
  Decode it before drawing any conclusions.
- **In the tariff annex, footnote `*1` means a family doctor can order it.**
  There is no "family doctor" column; the markers in NOTA 1 are the only signal.
  A test without the marker can only be ordered by a specialist. This is why
  cortisol, all sex hormones, ATPO, PTH, the immunoglobulins and reticulocytes
  aren't on the list.
- **The same passage has a third condition that's easy to miss.** The annual
  blood tests are for "adulţii asimptomatici, **cu factori de risc
  modificabili**", and only "pentru investigaţiile paraclinice apreciate a fi
  necesare de către medicul de familie". The tests require a risk factor, not
  just the check-up.

### Two tests that look like they qualify but don't

- **CRP hs**: the prevention list includes ordinary C-reactive protein, which is
  a different test from the high-sensitivity one this repo uses for Whoop's
  hs-CRP.
- **The hemogram with reticulocytes**: the state only pays for the 20-value
  blood count.

### Showing it in the files

`§` marks a Test. So when a provider's Basket buys a Panel that mixes free and
paid biomarkers, **the Panel can't be marked**, and the shopping list has to name
the free biomarkers in prose instead. There are two such cases today: Regina
Maria's `Profil lipidic` (total cholesterol and LDL are free, HDL and
triglycerides aren't) and Synevo's `Indice HOMA` (fasting glucose is free,
insulin isn't). That leaves both with 25 `§` biomarkers, against MedLife's 27.
**This is expected and doesn't need fixing.**

**`§` and `↑` don't combine**, and the Synevo and Regina Maria lists say so. The
lab can't add anything to a referral, so you can't buy the upgrade on top of a
free blood count. A reader who wants reticulocytes has to buy the full hemogram
(75 at Synevo, 70 at RM) and give up the free one. MedLife isn't affected,
because its reticulocyte count is a separate Test. Keep this in prose; don't
try to show it with a marker.

**Two Blocks are deliberately not the cheapest option on this route.** At Regina
Maria, total cholesterol and LDL are free, which leaves only HDL (35) and
Trigliceride (30): 65 against `Profil lipidic`'s 85. At Synevo, fasting glucose
is free, which leaves only Insulina (65) against `Indice HOMA`'s 82. Both
Baskets keep the panel, and both shopping lists describe the swap in prose. That
way every total in the repo means *what you pay without a referral*. **Keep one
Basket per provider; don't make a second one for this route.**

Regina Maria also publishes the tests it does under state insurance at
`/laboratoare-inteligente/gama-de-analize-decontabile`, using the same
`data-drupal-*` markup as its main catalogue: 114 rows, all at `0 Lei`. It's
useful for checking names against the annex, but it's the *whole* insured set,
not the prevention list, so **never mark `§` from it directly**.

## 5. Re-check the subscriber columns

Check each subscriber column on its own; don't work it out from the standard
prices next to it. Re-read each provider's discount annex, because it can
change without any public price changing and nothing else here would catch it.
Then decide again which lines are `●`, which are `○`, and which stay at full
price.

Both shopping lists **leave out the subscription's monthly price** on purpose.
The column is a price list for someone who already has the subscription, not an
argument for buying one.

### Regina Maria: Comfort Premium

**How it works.** Comfort Premium covers tests in two ways, like Respect Infinit
below.

The **annual set** is 11 tests: Test Papanicolau clasic/PSA, Sumar de urină,
Glicemie, LDL colesterol, HDL colesterol, Trigliceride, Hemoleucogramă, VSH,
Transaminaze (TGO, TGP), Creatinină serică and Colesterol total. They're free
with **no Recommendation**, **once a year each**, and the year counts from first
use, not from signing. These are the `●` lines. It's exactly the same eleven as
Respect Infinit's annual set.

The **discount annex** covers about 300 lab tests at 100% off, but *only* "la
recomandarea medicului RM". Whether a doctor gives it is their call, and RM
publishes no criteria, so this repo doesn't treat it as guaranteed. Every line
covered by the annex is `○` (estimated), never shown as a firm price.

The column is worth showing because the discount is big, roughly half of RM's
Core list, not because every reader will get it.

**Six shopping-list lines get the annual set's `●`:** the blood count, Glucoza
serica (as "Glicemie"), both transaminases, Creatinina serica, and PSA (read as
a Pap smear for women and PSA for men, the same way as MedLife's combined line).
All six are in the annex too, so `●` makes them certain but doesn't change their
price. **The annual set didn't change any RON figure in the shopping list.** The
other six annual-set tests have no line to grade. Sumar de urină and VSH aren't
Whoop biomarkers. Total cholesterol, LDL, HDL and triglycerides come inside
`Profil lipidic`, which is only in the annex. That last case is a real choice for
the reader: buying the four separately costs 0 guaranteed, against the panel's
0 estimated. So the shopping list mentions it in one sentence, the same way it
mentions the `§` swap.

**`↑` works differently in the subscriber column.** The hemogram with
reticulocytes is in neither list, so a subscriber pays its full 70, but their
Core hemogram cost 0, not 60. So the upgrade costs **10 in the standard column
and 70 in the subscriber column**, and Performance Health's subscriber subtotal
includes 70. Don't copy the 10 across just to make the columns look alike.

**Finding the current annex.** The current terms PDF is linked from
`https://shop.reginamaria.ro/abonamentul-comfort-premium-adulti.html` as a
download (`Anexa includeri abonament.pdf`). **Check that page** each refresh,
not a dated PDF under `reginamaria.ro/sites/default/files/`, because Regina
Maria updates the PDF without changing its URL. Before re-checking anything,
compare the PDF's `ModDate` with your last refresh to see whether it changed. It
was `2024-05-27` at the 2026-08-07 check.

The page has no link ending in `.pdf`. The download is a Magento endpoint,
`https://shop.reginamaria.ro/catalog/product/downloadattachment/product_id/70/`,
which returns `application/pdf` on a plain GET. Searching the page for `.pdf`
finds nothing; search for `downloadattachment`.

**Matching annex names to shopping-list lines is manual, and must be exact.**
The annex uses generic names ("Testosteron total", "PCR (Proteina C reactivă)
test cantitativ"), while the shopping list uses RM's specific product names.
**Only count a product as covered if the annex names it specifically.** Three
unclear cases are currently all kept at full price:

- **Proteina C reactiva inalt sensibila (HSCRP)**: the annex says "PCR", not the
  high-sensitivity test specifically.
- **Free PSA**: the annex says "PSA", not free PSA specifically.
- **The hemogram upgrade with reticulocytes**: the annex says "Hemoleucogramă
  completă", not that product specifically.

Re-check these three by name against the current annex each refresh. Regina
Maria could clarify any of them either way.

### MedLife: Respect Infinit

The MedLife equivalent of Comfort Premium. The same rules apply: leave out the
monthly price, and mark anything that needs a doctor's recommendation as `○`.

**How it works.** Respect Infinit (539 RON/month, valid 12 months) covers tests
in two ways, like Comfort Premium.

The **annual set** is 11 tests: Papanicolau clasic/PSA, Sumar de urină,
Glicemie, LDL colesterol, HDL colesterol, Trigliceride, Hemoleucogramă, VSH,
Transaminaze (TGO, TGP), Creatinină serică and Colesterol total. They're free
with **no Recommendation**, **once a year each**, and the year counts from first
use, not from signing. These are the `●` lines.

The **discount annex** covers about 19 categories of tests (Biochimie,
Hematologie, Markeri endocrini, Markeri cardiovasculari, Imunologie, Coagulare,
Electroforeză, Biologie moleculară, Anatomie patologică, Markeri tumorali,
Markeri osoși, Markeri hepatici, Markeri infecțioși, Markeri alergii,
Microbiologie, Bacteriologie, Toxicologie, Parazitologie, Screening prenatal) at
100% off, but needs "cu recomandarea medicului MedLife". That's the same
uncertainty as Comfort Premium's annex. These are the `○` lines.

Unlike Comfort Premium, Respect Infinit's annex limits each test to **4 times a
year** ("1 analiză/trimestru"). Comfort Premium's annex has no such limit, so
**the two files word this differently on purpose**.

**Finding the current annex.**

```
https://www.medlife.ro/documente_publice/abonamente_individuale/2024/Abonament_individual_MedLife_Respect_Infinit.pdf
```

It's in Romanian and English, and MedLife updates it at the same URL despite the
`/2024/` in the path (`ModDate` was `2025-07-14` at the 2026-08-05 check).
Compare `ModDate` with your last refresh before re-checking anything, as with
Comfort Premium's PDF.

**Matching is manual and must be exact**, as above.

*Not in the annex, so kept at full price with no note:* Cortisol, Estradiol,
FSH, Insulin, DHEA Sulfate (which also has no line, since it has no Block), and
"Testosteron liber" (only plain "Testosteron" is listed). The `Markeri
endocrini` category is a specific list, not a catch-all, and cortisol isn't on
it. "17 OH Corticosteroizi" is a different test.

*Unclear, so kept at full price:*

- **Free T4** and **Free T3**: the annex says "T4" and "T3", not the free
  hormone tests specifically.
- **Ureea nitrogen (BUN)**: the annex says "Uree serica", not BUN specifically.
- **CRP hs**: the annex says "CRP cantitativ", not the high-sensitivity test
  specifically. Same problem as Comfort Premium's HSCRP line.

*Counted as covered even though the wording differs. Re-check all three by name
each refresh:*

- **Glucoza serica** is covered because the annex's Biochimie category lists it
  by that exact name. It's probably also the annual set's "Glicemie", which is
  the everyday Romanian word for the same fasting-glucose test. That would make
  it covered twice, but the decision doesn't depend on it.
- **Ag. specific prostatic (PSA)** is covered because the annex's Markeri
  tumorali category lists it by that exact name. It's probably also the annual
  set's combined "Test Papanicolau clasic / PSA" line, read as a Pap smear for
  women and PSA for men. Again the decision doesn't depend on it; it's only why
  the line is `●` rather than `○`.
- **Ac Anti-Tireoperoxidaza (ATPO)**: the annex's Imunologie category calls it
  "Ac Anti-Tireoperoxidaza (TPO)". It's the same antibody test.

**Free PSA isn't a problem at MedLife the way it is at Regina Maria.** MedLife's
Markeri tumorali category lists "Ag. specific prostatic (PSA)" and "Free PSA" as
two separate lines, so both are clearly covered. **Keep MedLife's Free PSA line
covered.**

## 6. Confirm what each panel contains

This is an input to the next step. Check these still hold; they change much
less often than prices.

| Provider | Panel test | RON | Biomarkers it gives |
|---|---|---:|---|
| Synevo | Hemograma cu formula leucocitara, Hb,Ht,indici si reticulocite (Hemograma) | 75 | 21: Basophil %, Basophils, Eosinophil %, Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes, Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell Distribution Width (RDW), Reticulocyte Count (RET), White Blood Cells (WBC) |
| Synevo | Hemograma cu formula leucocitara cu Hb, Ht si indici | 44 | 20: as above, without Reticulocyte Count (RET) |
| Synevo | Acizi grasi omega 3 si omega 6 | 502 | 4: Arachidonic Acid (AA), Docosahexaenoic Acid (DHA), Eicosapentaenoic Acid (EPA), Linoleic Acid (LA) |
| Synevo | Glucoza serica (glicemie) | 21 | 2: Blood Fasting Glucose, Glucose |
| Synevo | Indice HOMA | 82 | 3: Blood Fasting Glucose, Glucose, Insulin. The product page says it *conține Glucoză serică și Insulină* |
| Regina Maria | Hemoleucograma completa cu formula leucocitara, Hb, Ht, indici si reticulocite | 70 | 21: the same 21 as Synevo's hemogram with reticulocytes |
| Regina Maria | Hemoleucograma cu formula leucocitara,Hb,Ht, indici eritrocitari | 60 | 20: as above, without Reticulocyte Count (RET) |
| Regina Maria | Profil lipidic | 85 | 4: HDL Cholesterol, LDL Cholesterol, Total Cholesterol, Triglycerides |
| Regina Maria | Glucoza serica | 30 | 2: Blood Fasting Glucose, Glucose |
| Regina Maria | Profil LDL (LDL colesterol, sd-LDL colesterol, LDL oxidat, lipoproteina A) | 400 | 2: LDL Cholesterol, LDL Small |
| MedLife | Hemoleucograma completa | 51 | 20: the 21 above, without Reticulocyte Count (RET) |
| MedLife | Glucoza serica | 21 | 2: Blood Fasting Glucose, Glucose |
| MedLife | Acizi grasi omega 3 si omega 6 | 512 | 4: the same four as Synevo's. Same name, so we **assume** the same contents; MedLife has no per-test page to confirm it |

**The other two providers also sell an `Indice HOMA`, and neither is in a
Basket.** MedLife's (85) costs more than its parts (21 + 61 = 82). Regina
Maria's (90) would cost less than its parts (30 + 70 = 100), but RM has no
per-test page, so we don't know whether it reports both input values or only the
ratio. Confirm that before adding it to RM's Basket; it would save 10 RON. It's
listed as an open item in `CLAUDE.md`.

**Regina Maria's `Profil LDL` names `lipoproteina A` in its product name, but
the table above leaves it out on purpose.** Regina Maria has that test switched
off, so the panel only gives LDL Cholesterol and LDL Small. Check it each
refresh. If it's switched back on, a reader buying Core plus Heart Health pays
for Lp(a) twice, because Core's Basket already buys `Lipoproteina A` on its own
for 85. No Basket changes either way: for Core alone, 85 beats 400 (catalogue
price on 2026-10-02).

**We don't know what Regina Maria's broad fatty-acid panel reports.** The
current catalogue lists `Acizi grasi - saturati, mononesaturati, omega-3,
omega-6` at 830 RON, but doesn't name the individual acids. AA, DHA, EPA and LA
are `?` for Regina Maria in `BIOMARKERS.md`, not confirmed. Find the panel's
contents before counting or pricing them in Heart Health or Performance Health.

The full 21-biomarker blood count: Basophil %, Basophils, Eosinophil %,
Eosinophils, Hematocrit, Hemoglobin, Lymphocyte %, Lymphocytes, Mean Corpuscular
Hemoglobin (MCH), Mean Corpuscular Hemoglobin Concentration (MCHC), Mean
Corpuscular Volume (MCV), Mean Platelet Volume (MPV), Monocyte %, Monocytes,
Neutrophil %, Neutrophils, Platelets, Red Blood Cell Count (RBC), Red Cell
Distribution Width (RDW), Reticulocyte Count (RET), White Blood Cells (WBC).

## 7. Recompute every Basket

The Basket is the cheapest set of Tests that covers every Core biomarker a
provider sells. **It's calculated, not stored.** Invariant 2 in `CLAUDE.md`
explains why this is the easiest way for the repo to go wrong unnoticed.

For each provider, find the cheapest set of Tests that together cover every Core
biomarker it sells. The only real choice is whether to buy a panel or its parts,
so compare each panel's price with the total of the separate Tests it replaces,
using the table above.

Recompute all three from scratch after **any** price change. That includes a
change that comes from a subscription annex or the `§` list, not just from a
catalogue.

## 8–10. Update the files

The Basket decides what goes in each `SHOPPING-LIST-*.md`; there's no separate
step. But if a price change flips a panel-or-parts choice, update **both** the
Basket *and* which lines appear in the shopping list.

Then recompute every Block subtotal and coverage count, copy the four numbers
per provider into the README (see `file-shapes.md`), and update the dates as
invariant 10 describes.
