# Codings of 13 trustworthy-AI frameworks, with a crosswalk to a seven-step practitioner's loop
  [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24424711.svg)](https://doi.org/10.5281/zenodo.24424711)

Version 1, November 2026. Author: Dragos Tomescu. First coding by Claude (Anthropic), blind second coding by Gemini (Google), every disagreement settled by the author.

The data behind the article *AI frameworks for mortals: a practitioner's guide*. It breaks 13 widely used frameworks for trustworthy AI into their 108 headline items, tags each item against 19 themes, and maps each item to the step of a seven-step practitioner's loop where work on it starts. It holds labels and codes only: no framework text.

## Source and licence

The frameworks belong to their publishers. This dataset reproduces only item labels (the headings each framework gives its principles, risks or practices), the section they come from and a link to the source. Read the framework itself for what each item says.

This dataset is released under CC-BY 4.0. If you use it, please cite: *Tomescu, D. (2026). Codings of 13 trustworthy-AI frameworks, with a crosswalk to a seven-step practitioner's loop. Version 1.* https://doi.org/10.5281/zenodo.24424711

## Files

| File | What it holds |
|---|---|
| `data/frameworks.csv` | The 13 frameworks (publisher, type, version, link), plus the two excluded and why |
| `data/items.csv` | One row per headline item, 108 rows: label, source section, primary theme, themes named and mentioned |
| `data/final_codes.csv` | The final codes in long format, 207 rows: one row per item and theme |
| `data/codebook.csv` | The 19 themes: definition, what each includes and excludes, decided borderline cases |
| `data/coding_rules.csv` | The ten coding rules |
| `data/codes_claude.csv`, `data/codes_gemini.csv` | Each coder's codes before adjudication, with notes |
| `data/agreement.csv` | Each coder's primary theme per item, and the final one |
| `data/adjudication.csv` | The 16 decisions that settled disagreements |
| `data/coverage.csv` | How many frameworks name each theme (chart 1 in the article) |
| `data/crosswalk_items.csv`, `data/crosswalk_steps.csv` | Which steps of the loop start work on each item (chart 3) |
| `figures/` | The article's three charts |
| `METHOD.md` | The method, its limits and the main results |
| `CITATION.cff` | Citation details for the "Cite this repository" button |
| `LICENSE` | CC-BY 4.0 |

## The frameworks

| ID | Framework | Publisher | Type |
|---|---|---|---|
| F01 | AI Risk Management Framework and Generative AI Profile | NIST (US) | Public |
| F02 | ISO/IEC 42001, Annex A | ISO and IEC | Public |
| F03 | AI Act, Articles 4, 9 to 15 and 50 | European Union | Public |
| F04 | Model AI Governance Framework for Agentic AI | IMDA (Singapore) | Public |
| F05 | AI Verify Testing Framework | IMDA and AI Verify Foundation (Singapore) | Public |
| F06 | Guidance for AI Adoption | National AI Centre (Australia) | Public |
| F07 | AI Playbook for the UK Government | Government Digital Service (UK) | Public |
| F08 | Responsible AI core dimensions | AWS | Company |
| F09 | Responsible AI principles | Microsoft | Company |
| F10 | Trustworthy AI framework | Deloitte | Company |
| F11 | Trusted AI framework | KPMG | Company |
| F12 | Guiding principles for responsible agentic AI | Salesforce | Company |
| F13 | VURA | Alteryx | Company |

## Columns that need explaining

- **Themes** use three-letter codes, defined in `codebook.csv`. For example `TRA` transparency, `TST` testing and monitoring, `RPR` reproducibility (called repeatability in the article), `ACT` agent action limits.
- **Strength.** `Named`: the item itself is about the theme (its label or main point). `Mentioned`: the theme appears only in the item's description. The article counts Named only.
- **Primary.** Each item has exactly one primary theme.
- **Steps** (crosswalk) refer to the loop in the article: 1 Pick one task and define good; 2 Break the task into steps; 3 Decide who does each step: code, model or person; 4 Build each step for its type; 5 Test and fix the most common failure; 6 Decide what a person checks; 7 Go live gradually, then maintain it. A step "starts work on" an item; it does not meet it.
- **first_addressed_at_stage** (crosswalk) is how deep a team has to go before the item is covered: stage 1 a first, thin round of the loop; stages 2 and 3 deeper rounds as the stakes rise.

## Known limits

- **Headline items only.** A theme a framework discusses below its headline items is not counted. Counts are lower than in reviews that code whole documents.
- **Model coders, not people.** Claude and Gemini agreed on the primary theme for 101 of the 108 items (Cohen's kappa 0.89 on theme presence). That is agreement between two models, not human reliability. The author settled every disagreement.
- **ISO from secondary sources.** ISO/IEC 42001 is paywalled; its item names come from secondary sources, and the standard itself was not consulted.
- **The crosswalk is one person's judgement.** Claude drafted the mapping and the author reviewed it. It was not coded twice.
- **A snapshot.** Frameworks change. Versions are in `frameworks.csv`.

## Reproduce a few numbers

```python
import pandas as pd

items = pd.read_csv("data/items.csv")
codes = pd.read_csv("data/final_codes.csv")
named = codes[codes.strength == "Named"].assign(fw=lambda d: d.item_id.str[:3])

print(len(items))                                        # 108
print(named.groupby("theme").fw.nunique().sort_values()) # TRA 13, ACC 12 ... TST 3, RPR 2, ACT 2
agree = pd.read_csv("data/agreement.csv")
print(agree.same_primary.sum())                          # 101
```
