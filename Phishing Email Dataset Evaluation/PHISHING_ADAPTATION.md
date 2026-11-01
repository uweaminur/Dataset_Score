# Phishing Email Dataset Evaluation

This folder adapts the deterministic scoring framework of
[Rahman et al. (2026)](https://doi.org/10.3390/app16189205), built for intrusion detection datasets,
to **phishing email datasets**.
The framework keeps its six categories and its 0–5 scale.
Only the columns inside each category, and a few scoring rules, change.

## Files

| File | Purpose |
|---|---|
| `evaluate_phishing_datasets.py` | Scoring and figures. The original threat code with minimal changes, each marked `# PHISHING`. |
| `Full_Dataset1.csv` | Input for the script: 20 selected phishing datasets, two-row header. |
| `phishing_dataset_evaluation_20.xlsx` | Editable version of the input, with a **Selection** sheet (full names and why each dataset was chosen) and a **Changes and mapping** sheet. |
| `phishing_dataset_evaluation_framework_v2.xlsx` | All 58 phishing email datasets in the same format (supplementary table). |

## How to run

```bash
pip install pandas numpy matplotlib
python evaluate_phishing_datasets.py
```

The script reads `Full_Dataset1.csv` and writes `dataset_scores_table.csv`.
It then draws a heatmap, a multi-line chart, a radar chart and an overall bar chart.

To score all 58 datasets instead of the 20, export the first sheet of
`phishing_dataset_evaluation_framework_v2.xlsx` as `Full_Dataset1.csv`.

## The six categories

| Category | Columns | Score (0–5) |
|---|---|---|
| Attack Coverage | Phishing, Spear_Phishing, BEC, Fraud_Scam, LLM_Phishing | 5 × classes present / 5 |
| Provenance Source | Source, Legitimate_Email | Source score, +1 if legitimate email is present (max 5) |
| Feature Diversity | Header_Feature, URL_Feature, Content_Feature | 5 × features present / 3 |
| Documentation & Metadata | Release_Year, Meta_Data, Data_Format | 5 × items present / 3 |
| Evaluation Readiness | Predefine_Split, Balanced, Labelled | 5 × items present / 3 |
| Availability | Availability | Public 5, On request 3, No 1, blank 0 |

**Overall** = mean of the six category scores.

## Adjustments from the intrusion detection framework (Rahman et al., 2026)

| # | Category | Intrusion detection dataset (original) | Phishing email dataset (adjusted) | Code change | Rule or note |
|---|---|---|---|---|---|
| 1 | Attack Coverage | DoS, DDoS, U2R, R2L, Probe | Phishing, Spear_Phishing, BEC, Fraud_Scam, LLM_Phishing | Yes: `ATTACK_COLS` | A class counts if it is labelled or is the dataset's design. |
| 2 | Attack Coverage | — | Spam_Present (not scored) | No | Spam is not an attack; recorded for label quality only. |
| 3 | Provenance | Regular_Traffic | Legitimate_Email | Yes: `score_provence` | Same +1 bonus. |
| 4 | Provenance | Real 5, Emulated 3, Synthetic 1 | Real 5, Mixed 3, Simulated 3, LLM-generated 1 | Yes: source map | Original values still work. |
| 5 | Feature Diversity | Basic_Feature | Header_Feature | Yes: `FEATURE_COLS` | Headers or sender/receiver fields kept. |
| 6 | Feature Diversity | Traffic_Feature | URL_Feature | Yes: `FEATURE_COLS` | URLs recoverable. |
| 7 | Feature Diversity | Content_Feature | Content_Feature | No | Subject and body text. |
| 8 | Documentation | Release_Year | Release_Year | No | First public release, or the dataset paper's year. |
| 9 | Documentation | Meta_Data | Meta_Data | No | TRUE if a paper or data card describes collection, fields and labels. |
| 10 | Documentation | Data_Format (blank = none) | Data_Format (any format counts) | No | Blank means not yet checked. |
| 11 | Evaluation Readiness | Balanced | Balanced | No | Minority class ≥ 40%. Single-class datasets are FALSE. |
| 12 | Evaluation Readiness | Labelled | Labelled | No | Single-class archives are implicitly labelled. Enron and Avocado are FALSE. |
| 13 | Evaluation Readiness | Predefine_Split | Predefine_Split | No | Official train/test splits or folds only. |
| 14 | Availability | Public / On request / No | Same values | No | Licence, account or embargo = On request. Private = No. Unclear = blank. |
| 15 | Scope | Threat datasets | Email datasets only | No | URL feeds, incident databases, platforms and mirrors excluded. |
| 16 | Scope | All datasets in one figure | 20 in the figures, 58 in the supplementary table | No | Every dataset family is represented. |
| 17 | Scope | — | One row per corpus | No | TREC 2005–07 is one row. |
| 18 | Figures | "Threat Dataset…" title | "Phishing Email Dataset…" title | Yes | Heatmap title. |
| 19 | Figures | Fixed figure size | Size scales with the number of datasets | Yes | Heatmap height and bar-chart width. |
| 20 | Figures | Full names | Short names | No | Full names are on the Selection sheet. |

## Selected datasets (20)

| Family | Datasets |
|---|---|
| Real-world corpora (7) | Nazario, phishing_pot, Enron, SpamAssassin, TREC 2005–07, CLAIR (Nigerian Fraud), IWSPA-AP 2018 |
| Curated and merged (5) | Champa Curated (11 sets), Kaggle (Alam), Kaggle (Chakraborty), MeAJOR, APOLLO |
| LLM-generated (6) | Human-LLM (Greco), AI Phishing (Eze & Shamir), E-PhishLLM, PhishFuzzer, NEST-Phish, PhishTwins |
| Organisational context (1) | Avocado |
| Private industry corpus (1) | APWG Corpus (Park) |

They were chosen to cover every dataset family, include the datasets most used in the literature, and span a wide range of scores.
The other 38 datasets are left out mainly because many of their cells cannot be filled yet
(release not confirmed, contents unknown), or because they duplicate a selected dataset.

## Assumptions

- **Class presence.** An attack class counts when it is labelled, or when the whole dataset is that class.
- **Spam is not scored.** Spam is recorded in `Spam_Present`, but it does not count as attack coverage.
- **LLM-generated data scores 1** for Source, like "synthetic" in the threat framework.
  This is a judgement. A sensitivity run with LLM-generated = 3 is recommended.
- **Balanced** means the minority class is at least 40% of the labelled data.
- **Blank cells score 0.** The script reads a blank as FALSE. Blank means "not confirmed", not "absent".

## Limitations

- Values come from dataset papers, data cards and repository pages. They have not all been checked against the downloaded files.
- Some cells are still blank, which lowers those datasets' scores until they are filled.
- The scores rate dataset **properties**, not how well detectors perform on the data.
- Merged datasets reuse the same classic corpora, so high scores for several merges do not mean independent data.

## Citation

If you use this framework, please cite the original paper:

> Rahman, S. M. A., Ullah, A. B., Mohammadian, M., & Alem, Y. F. (2026). A PRISMA-derived deterministic scoring framework for intrusion detection dataset evaluation. *Applied Sciences*, 16(18), 9205. https://doi.org/10.3390/app16189205

```bibtex
@article{rahman2026prisma,
  title   = {A PRISMA-Derived Deterministic Scoring Framework for Intrusion Detection Dataset Evaluation},
  author  = {Rahman, S. M. Aminur and Ullah, Abu Barkat and Mohammadian, Masoud and Alem, Yibeltal F.},
  journal = {Applied Sciences},
  volume  = {16},
  number  = {18},
  pages   = {9205},
  year    = {2026},
  doi     = {10.3390/app16189205}
}
```

The phishing adaptation in this folder is by S. M. Aminur Rahman, University of Canberra
(ORCID: [0009-0001-5229-2645](https://orcid.org/0009-0001-5229-2645)).
