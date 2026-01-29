# Dataset_Score

This repository contains the code and descriptor tables used to operationalise a PRISMA-guided, property-based scoring framework for cybersecurity threat datasets. It generates (i) a scored table of datasets across six categories and (ii) the plots used for comparative visualisation (heatmap, category line chart, radar chart, and overall bar chart).

## Repository contents (key artefacts)
- `Full_Dataset1.csv`  
  Primary dataset descriptor table (property coding inputs used by the scoring pipeline).
- `dataset_scores_table.csv`  
  Generated output table containing category scores and the overall mean score (0–5).
- `Final_Scoring_Code.ipynb` / `Datasetscoring.ipynb`  
  Executable notebooks implementing scoring and figure generation.
- `Final/`  
  Folder containing the final workflow version (if applicable).

## Reproducibility and environment
- **Python:** 3.10+ recommended  
- **Dependencies:** listed in `requirements.txt`  
- **Operating system:** platform-independent (tested with standard scientific Python stack)

## Quickstart (minimal reproduction)
1. Clone the repository:
   ```bash
   git clone https://github.com/uweaminur/Dataset_Score.git
   cd Dataset_Score