# Synthetic FHIR R4 patient data: 30 patients, 6 months of history

**Free synthetic HL7 FHIR R4 test data** for developing and testing healthcare apps, FHIR servers, analytics and AI
agents: 30 made-up patients across Europe, aged 3 to 85, with six months of vital signs, lab results, conditions,
medications, allergies, symptoms, documents and Provenance. Coded with LOINC, ICD-10, ATC and UCUM.

No real person is behind any of it. It is the same data every [Anpheros sandbox](https://developers.anpheros.com/guides/fhir-sandbox?utm_source=github&utm_medium=readme&utm_content=fhir-synthetic-patients)
starts with, exported through the public API (`Patient/$everything`).

| | |
|---|---|
| Patients | 30 (ages 3–85, names from across Europe) |
| Observations | 3,437 (2,992 vital signs, 401 laboratory, 44 symptoms) |
| Conditions | 43 (ICD-10: hypertension, type 2 diabetes, atrial fibrillation, asthma, migraine, anxiety…) |
| MedicationStatements | 51 (ATC) |
| AllergyIntolerance | 6 |
| DocumentReference | 57 (lab report metadata) |
| Provenance | 3,624 (who wrote each resource, from which system) |
| FHIR version | R4 (4.0.1) |

## Files

| Path | Format |
|---|---|
| [`bundles/`](bundles) | one FHIR `Bundle` (type `collection`) per patient, e.g. [`04-andrei-popescu.json`](bundles/04-andrei-popescu.json) |
| [`ndjson/`](ndjson) | FHIR Bulk Data style NDJSON, one file per resource type |
| [`manifest.json`](manifest.json) | patients, resource counts, history window |

## Quick start in 2 minutes

Load it into any FHIR R4 server, or use it with pandas:

```python
import json, pandas as pd
rows = [json.loads(l) for l in open("ndjson/Observation.ndjson")]
df = pd.json_normalize(rows)
print(df[["code.coding", "valueQuantity.value", "effectiveDateTime"]].head())
```

Or skip the files: [get a free sandbox key](https://platform.anpheros.com/dashboard/?utm_source=github&utm_medium=readme&utm_content=fhir-synthetic-patients)
and query the same 30 patients through a live FHIR API:

```bash
curl -H "Authorization: Bearer $ANPHEROS_KEY" \
  "https://platform.anpheros.com/fhir/R4/Observation?category=laboratory&_count=5"
```

In the sandbox you can also write, reset the data, call `$summary` (IPS) and `$validate`, and connect an AI agent
through the [MCP server](https://github.com/anpheros/mcp-server).

## How it is made

A deterministic generator gives each persona realistic baselines, day-to-day variation, periodic lab panels with
reference ranges, and a few planted patterns (trends and recurring symptoms), so trend and pattern detection can be tested
against known answers. Ids and dates are fixed per release; the history ends
on the date in `manifest.json`.

## Guides

[Synthetic patient data](https://developers.anpheros.com/guides/synthetic-patient-data?utm_source=github&utm_medium=readme&utm_content=fhir-synthetic-patients) ·
[FHIR sandbox](https://developers.anpheros.com/guides/fhir-sandbox?utm_source=github&utm_medium=readme&utm_content=fhir-synthetic-patients) ·
[Free FHIR database](https://developers.anpheros.com/guides/free-fhir-database?utm_source=github&utm_medium=readme&utm_content=fhir-synthetic-patients) ·
[Getting started](https://developers.anpheros.com/guides/getting-started?utm_source=github&utm_medium=readme&utm_content=fhir-synthetic-patients)

## License

Apache-2.0 (see [LICENSE](LICENSE)).
The code systems used are the property of their owners: LOINC® (Regenstrief Institute, see [loinc.org/license](https://loinc.org/license/)),
ICD-10 (World Health Organization), ATC (WHO Collaborating Centre for Drug Statistics Methodology) and UCUM.
The data is for software testing only, not for clinical use.
