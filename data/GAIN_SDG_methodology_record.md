# GAIN — SDG Indicators for Forcibly Displaced Populations: Methodology Record

*How each SDG indicator was derived from the different survey microdata, the disaggregation
used, and the caveats. Companion to `gain_sdg_values_calculated.csv` (computed) and
`gain_sdg_values_promoted.csv` (report-extracted).*

Last updated: 2026-08-24.

---

## 1. Disaggregation — the population-group variable, per survey family

Every value is broken down by displacement status. The variable that carries that status
**differs by survey family**:

| Survey family | Datasets in hand | Group variable | Groups it distinguishes |
|---|---|---|---|
| **UNHCR FDS** (Forced Displacement Survey) | South Sudan 2023, Pakistan 2024, **Zambia 2025** | `Intro_07` / `Intro_07_1` | Refugees · Host community · (Former refugees · Returnees) |
| **UNHCR Profiling** | Cameroon 2024, DR Congo Grand Kasaï 2023 | `HOUSEHOLD_STATUS` (CMR) · `s00q0_HHtype_HHH` (COD) | IDP · Host · Returned IDP · Repatriated refugee |
| **WB–UNHCR JDC HFPS** | Ethiopia, Chad, Burkina, Djibouti, Sudan, Jordan | (survey-specific stratum) | Refugee / IDP / host |
| **CBPS** (Cox's Bazar Panel Survey — **Rohingya**) | Bangladesh 2019–2020 | `stratum` | **Camp (Rohingya)** · High-spillover host · Low-spillover host |
| **SRHCS** (Syrian Refugee HH Survey) | Jordan, Lebanon, Iraq/Kurdistan 2016 | Syrian-refugee sample frame | Syrian refugees |
| **National MICS / DHS** | Bangladesh MICS7, Afghanistan MICS6 | **none** | General population **only** — camps excluded from frame |

> **Critical caveat:** the **national MICS/DHS surveys carry no refugee/IDP flag** — their
> sampling frames exclude the camps. They are used as the **general-population baseline** for
> comparison, **not** as a source of refugee-disaggregated values.

---

## 2. How each SDG indicator was computed

For every indicator: the internationally-agreed definition (fixed), then the **survey-specific
variable mapping** (what changes per survey), and the survey weight.

### SDG 2.1.2 — Food insecurity
- **Definition:** WFP **Food Consumption Score** = Σ (7-day frequency of 8 food groups ×
  weight: cereals ×2, pulses ×3, vegetables ×1, fruit ×1, meat/fish ×4, dairy ×4, sugar ×0.5,
  oil ×0.5). Classified **Poor ≤21 / Borderline ≤35 / Acceptable >35**; *food-insecure = FCS ≤35*.
  Own-use farming is **excluded** (per 19th ICLS). Where a survey ships a pre-computed reduced
  Coping Strategy Index (**rCSI**) level, that is used directly (level 3 = food-insecure).
- **Variable mapping:**
  - South Sudan FDS → `Food_div1..8`; weight `wgh_samp_resc_pop`
  - Pakistan & Zambia FDS → `Food11aa, Food12a, Food13a, Food14a, Food15a, Food16a, Food17a, Food18a`; weight `wgh_strata_spec`
  - Cameroon Profiling → `rCSI_LEVEL` (pre-computed); by `HOUSEHOLD_STATUS`

### SDG 6.1.1 / 6.2.1 — Improved drinking water / sanitation
- **Definition:** WHO/UNICEF **JMP ladder** — % using an *improved* source/facility.
  Water improved = piped/faucet, borehole/tubewell, protected well/spring, rainwater, packaged/
  delivered. Sanitation improved = flush/pour-flush, VIP, pit latrine with slab, composting.
- **Variable mapping:**
  - FDS (SSD/PAK/ZMB) → `BD01` (water), `SAN01` (sanitation); classified by JMP code list
  - DRC Profiling → `s06q15_WaterSource`, `s06q25_Toilet` (codes 1–4 = improved sanitation)
  - National MICS → `WS1`, `WS11`

### SDG 8.5.2 — Unemployment rate
- **Definition (ILO):** unemployed ÷ (employed + unemployed). **Employed** = worked ≥1 hr for
  pay/profit or temporarily absent. **Unemployed** = not employed **and** actively sought work
  **and** available to start.
- **Variable mapping:**
  - Pakistan FDS → employed `EMP01–EMP05`; sought `EMP25a`; available `EMP29`
  - South Sudan FDS → **not computed** (its non-employed job-search question routes differently;
    `EMP20/EMP22` measure *underemployment* of the already-employed, so were not used)

### SDG 16.9.1 — Birth registration, under-5
- **Definition:** children under 5 whose birth is registered ÷ all under-5.
- **Variable mapping:** roster level, filtered to age <5.
  - SSD & PAK FDS → `HH_07b` (birth registered); age `ageYears` (SSD) / `agetouse` (PAK)
  - DRC Profiling → `s01q04_BirthCertificate` (member file, joined to household group via `ID_Household`)

### SDG 4.1.2 — School attendance, ages 6–17
- **Definition:** children 6–17 currently attending/enrolled ÷ all 6–17.
- **Variable mapping:** roster level.
  - SSD FDS → `HH_Educ02a` (currently attending)
  - DRC Profiling → `s03q03_InSchoolNow`
  - *Pakistan:* the auto-detected variable was pre-school (`HH_Educ00`) — **not used** (wrong band).

---

## 3. Computed values (as of 2026-08-24)

16 calculated values across **5 countries** (refugee/IDP value; host value in the CSV):

| Country | 2.1.2 Food | 6.1.1 Water | 6.2.1 Sanit. | 8.5.2 Unemp. | 16.9.1 Birth reg | 4.1.2 School |
|---|---|---|---|---|---|---|
| South Sudan (FDS) | 90% | 97% | 58% | — | 62% | 81% |
| Pakistan (FDS) | 47% | 79% | 74% | 11% | 42% | — |
| Zambia (FDS) | 76% | 98% | 75% | — | — | — |
| DR Congo (Profiling) | — | 30% | 8% | — | — | — |
| Cameroon (Profiling) | 51% | — | — | — | — | — |

**Recurring finding:** refugees in *managed settlements* often out-perform surrounding hosts on
services (Zambia water 98% vs 75%, sanitation 75% vs 29%; South Sudan sanitation 58% vs 18%;
birth registration 62% vs 27%), while **food insecurity is severe for both** (South Sudan 90%,
Zambia 76%). Full host comparisons and per-cell method notes are in
`gain_sdg_values_calculated.csv`.

---

## 4. Caveats recorded (per survey / indicator)

- **National MICS (Bangladesh MICS7, Afghanistan MICS6):** general population only; the frame
  excludes camps → used as **baseline**, never as a refugee value. (Bangladesh national water
  came out ~47% via `WS1` codes — a JMP-tier/arsenic mapping quirk; the national figure is
  really ~98% and needs the improved/limited tiering before use.)
- **South Sudan 8.5.2 (unemployment):** skipped — a wrong 0–1% resulted from using the
  underemployment variables; the correct non-employed job-search item must be located first.
- **DR Congo (Grand Kasaï Profiling):** **unweighted** (no design weight ships with the file);
  member-level birth-certificate and enrolment came out 0% for all groups (the yes/no code is not
  `1` as assumed) — **WASH only** was retained until the value labels are inspected.
- **Cameroon Profiling:** the distributed **CSV strips value labels** (WASH variables are bare
  codes 1–4) — only the self-documenting `rCSI_LEVEL` was computable from the CSV; the `.dta`/`.sav`
  would carry the labels for the rest.
- **CBPS (Rohingya):** rich panel, but **modules differ by round and by group** — e.g. the
  skip-meals item (`s04c`) was asked to the **host** sample only in Round 2, so a clean
  camp-vs-host cell requires selecting a variable asked to both; and the COVID-subsample weight is
  missing for the camp records.
- **Zambia FDS:** ships as **`.rds`** (R-native) not readable Stata `.dta` — read with `readRDS`.

---

## 5. Method summary (repeatable recipe)

> Read the value labels (codebook) → identify the population-group variable + survey weight →
> map the survey's variables to the indicator definition → apply the fixed formula (WFP FCS / JMP
> ladder / ILO / registration ratio) → weight → disaggregate by group → tag `calc`.

The **formulas are fixed and international**; only the **variable names change per survey**
(~5–15 min of variable-location per survey). Mapping once per *family* (FDS, Profiling, HFPS,
MICS/DHS, CBPS, SRHCS) covers all countries within it.

---

## 6. Population group carried on every value (map tooltip)

Each value on the map is tagged with the **specific displaced population** it describes — never
generic "displaced." The tooltip chip is colour-coded: **Refugees** (incl. Syrian, Rohingya,
Palestine, resettled) = UNHCR blue; **IDPs** (incl. victims/displaced) = amber; **Venezuelans**
= teal; anything else (e.g. a national LFS that only *includes* non-nationals) = grey, so a
non-clean population is visible at a glance.

**Refugees ≠ migrants.** Census/foreign-born stock is *not* relabelled as refugees. Group by source:
- Refugees: South Sudan, Pakistan, Zambia, Kenya (FDS/RHHS); Egypt, Uganda, Italy, UK (reports)
- IDPs: DR Congo, Cameroon (Profiling); Somalia, Nigeria, Honduras, Colombia, Burkina (reports)
- Syrian refugees: Jordan, Iraq (SRHCS, est.); Rohingya refugees: Bangladesh (CBPS);
  Venezuelans: Ecuador, Peru

## 7. SDG 10.7.4 (share of population who are refugees) — not harvestable from reports

Scanned all downloaded reports for a clean *"X% of the population are refugees/IDPs"* figure.
**Not available as a percentage:** refugee-focused reports state population *characteristics*, not
the population *share*; census releases report only foreign-born/migrant **stock** — which must
**not** be relabelled as refugees. A clean 10.7.4 would have to come from UNHCR Refugee Data
Finder (refugee count ÷ total population), not from these report PDFs.

## 8. Integrity note — a figure pulled after review

**Nigeria 4.1.2 removed:** the NBS IDP figure (63.4%) is *share who **never** attended school* —
the inverse of attendance — so it was dropped rather than shown under "School attendance."
Replaced coverage with a clean **Uganda 4.1.2 = 71%** (primary net enrolment, refugee sample,
vs 78% national — UBOS UNHS).

## 9. Full-report sweep (all 83 PDFs) — adds, corrections, new indicator

Scanned every downloaded PDF for indicator + displacement-population + percentage in-context.
Census releases (Armenia, Belize, Cambodia, Kazakhstan, Kosovo, Thailand, Turkmenistan, S. Africa,
Zambia, Djibouti, Indonesia, Mexico, Morocco RGPH, Côte d'Ivoire, Mali, Malawi, Spain, Kyrgyzstan,
Uganda census) returned **no refugee/IDP-disaggregated indicator** — they carry migrant/foreign-born
stock only, which is **not** relabelled as refugees.

- **New indicator SDG 7.1.1 (access to electricity)** added to the schema (user-flagged).
  Values: Morocco 99.8%, Nigeria (IDP) 61%.
- **Added:** Egypt 2.1.2 = 58% (refugees food-insecure); Morocco 6.1.1/6.2.1/7.1.1 (labelled
  *"forced migrants (incl. refugees)"* — HCP survey population is mixed, **not** refugees-only).
- **Corrections (earlier mis-extractions):** Burkina 1.2.1 56%→**93%** (multidim. poverty incidence,
  IDP vs 75% host) and 2.1.2 35.6%→**76%** (food insecurity) per INSD ESEP-PDI p33/p37;
  Palestine 8.5.2 29.2%→**42.2%** (29.2% was West Bank only; 42.2% is the State-of-Palestine total).
- **Left out (mislabeled):** France/Norway "64%" (activity/employment rate, not unemployment);
  Somalia "70% not attending" and Nigeria "never attended" (inverse-signed vs attendance).

## 10. Temporal scope rule (2026-08-25) — 2018+ only, prefer 2021+, government-produced

Per EGRISS timeline: **IRRS adopted 2018, GAIN implementation monitoring from 2021.** The map
excludes **pre-2018 data** (won't be in GAIN to most extent) and prefers **government-produced**
official statistics in the 2021+ window.

**Removed for being pre-2018:**
- **Lebanon** — MICS 2011 (Palestinian refugees), all 6 computed indicators — dropped entirely.
- **Iraq** — SRHCS 2015 (Syrian, estimated) — dropped entirely.
- **Jordan 2.1.2** — SRHCS 2015 (estimated) — dropped; Jordan keeps only 8.5.2 (DOS LFS 2024).
- **Somalia 6.1.1 / 6.2.1** — SHFS-W1 2016 — dropped; Somalia keeps its 2022 JDC Displacement
  Phone Survey values (1.2.1, 6.2.1, 11.1.1).

Nigeria 2018 IDP survey (school 70%) retained (2018 = IRRS adoption year). All other map values
are 2021+ except a few 2018-2020 (Bangladesh CBPS 2020, Nigeria 2018).

## 11. Alignment to EGRISS priority SDG indicators (Paper 3, Oct 2024)

Indicators now framed against **EGRISS Methodological Paper 3 — "Capturing Priority SDG Indicators
in Refugee, IDP and Statelessness Contexts"**: **14 priority indicators** (12 IAEG-prioritised + 2
statelessness): 1.2.1, 1.4.2, 2.2.1, 3.1.2, 4.1.1, 5.1.1, 6.1.1, 7.1.1, 8.3.1, 8.5.2, 10.3.1,
11.1.1, 16.1.4, 16.9.1.

- **On the map & priority (6):** 1.2.1, 6.1.1, 7.1.1, 8.5.2, 11.1.1, 16.9.1 → marked with ◆.
- **On the map but NOT the priority set (3):** 2.1.2 (priority uses 2.2.1 stunting), 4.1.2
  (priority uses 4.1.1 proficiency), 6.2.1 (only 6.1.1 water is priority). Kept as supplementary.
- **Priority indicators still MISSING (8):** 1.4.2 land tenure, 2.2.1 stunting, 3.1.2 skilled
  birth attendance, 4.1.1 learning proficiency, 5.1.1 gender frameworks, 8.3.1 informal
  employment, 10.3.1 discrimination, 16.1.4 feeling safe — the target set for future computation.

Indicator names updated to official/EGRISS titles; official UN SDG icons embedded (data-URI) as
the card badges.
