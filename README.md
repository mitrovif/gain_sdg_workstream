# GAIN — SDG Indicators for the Forcibly Displaced

A living map of comparable **SDG indicators for refugees, IDPs and stateless people**, drawn from
**GAIN examples** — government-produced where possible, data year **2018 onward** (post-IRRS) — and
read against the **14 priority SDG indicators** in EGRISS Methodological Paper 3 (Oct 2024).

**▶ [Open the interactive map](sdg-map.html)** · or the landing page: `index.html`

## Contents
| File | What it is |
|------|------------|
| `index.html` | Landing page (share this) |
| `sdg-map.html` | Interactive choropleth — hover any country for its indicators |
| `data/gain_sdg_values_calculated.csv` | Values computed from microdata (host + method) |
| `data/gain_sdg_values_promoted.csv` | Values extracted from official reports (source + page) |
| `data/GAIN_SDG_methodology_record.md` | How each indicator was derived, per survey family |
| `data/census_sdg_tabulation_targets.csv` | NSO outreach targets for custom tabulations |
| `data/gain_next_round_outreach_draft.md` | Draft outreach messages for the next GAIN round |

## How to read the map
Each country is shaded by the strength of its evidence. Hover for cards, **grouped by population**
(refugees · IDPs · stateless). Each card shows the value, a **calc / rep / est** tag, the
**producer** (gov / UNHCR / WB), source, year, host comparison, a **◆** if it's an EGRISS priority
indicator, and a **✻** if the value comes from a GAIN example.

## Publish (GitHub Pages)
Push this folder to a repo, then **Settings → Pages → Deploy from branch → `main` / root**.

**Live link:** https://mitrovif.github.io/gain-sdg-workstream/

## Scope & caveats
Government-produced GAIN-example data, 2018+. Values are computed, report-extracted, or estimated
(labelled per card). Underlying micro-level data is **not** redistributed here — only derived
indicators. Built by the EGRISS Secretariat / GAIN.
