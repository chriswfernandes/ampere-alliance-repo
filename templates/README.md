# Upload templates

Starter files for adding your own acquisition targets to the **Ampere Alliance**
pipeline. They match exactly what the app's **"Load your own targets" → Templates ▾**
menu downloads, so you can grab one here, fill it in, and import it.

| File | Format | Best for |
| --- | --- | --- |
| `ampere-targets-template.csv` | CSV | Spreadsheets (Excel, Google Sheets) |
| `ampere-targets-template.md` | Markdown table | Pasting from docs / quick edits |
| `ampere-targets-template.json` | JSON | Programmatic export / round-tripping |

## How to use

1. Download or copy one of the templates above.
2. Replace the example row with your own targets (keep the header row / keys).
3. In the live site, open **Load your own targets**, then either choose the file
   via **Import file** or paste the contents into **Paste data** and click **Load**.
   - **Append** adds to the current list; **Replace** swaps it out.
4. Optionally use **Export current ↓** to download a clean JSON of what's loaded.

To make targets load for everyone (persistently), paste your roster into the `seed`
array in `index.html` and commit it, rather than importing per-session.

## Field reference

| Field | Type | Notes |
| --- | --- | --- |
| `name` | text | Company name |
| `city` | text | Auto-geocoded from a built-in lookup if `lat`/`lng` omitted |
| `prov` | text | Province/territory code: `BC AB SK MB ON QC NB NS PE NL YT NT NU` (or `US`) |
| `seg` | text | Segment / line of business |
| `rev` | text | Revenue, free-form (e.g. `$10–20M`) |
| `lat`, `lng` | number | Optional; exact map placement. Leave blank to geocode from `city` |
| `proven` | 0–100 | Track record & stability |
| `aligned` | 0–100 | Values fit / openness to equity roll |
| `scalable` | 0–100 | Headroom + shared-services leverage |
| `ambitious` | 0–100 | Growth appetite of leadership |
| `succession` | 0–100 | Owner-readiness signal |
| `fit` | 0–100 | Strategic fit / cross-sell whitespace |
| `why` | text | One-line rationale |

**Note on JSON:** the six 0–100 scores are nested under an `s` object
(`"s": { "proven": 80, ... }`). In the CSV and Markdown formats they are flat columns
— the importer maps them into `s` for you.
