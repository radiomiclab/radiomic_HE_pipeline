# MRI-based radiomics pipeline for Hepatic Encephalopathy (HE) stratification

Official implementation and documentation for the multicenter study on Hepatic Encephalopathy (HE) stratification using MRI-based radiomics.

## Repository Contents
- `HE_Radiomics_Pipeline.ipynb`: Main analytical pipeline (LASSO selection, Bootstrap validation, Nomograms, DCA, SHAP).
- `dataset_dictionary.ipynb`: **Core Utility** to initialize the environment and recreate the empty dataset structure (CSV skeleton) required for the analysis.
- `data/`: Target directory for the study dataset.
- `requirements.txt` / `environment.yml`: Dependency lists for environment replication.

## Dataset Implementation
To ensure reproducibility and multicenter compatibility, this repository does not distribute a pre-filled dataset but provides the **`dataset_dictionary.ipynb`** notebook. Running that utility will:
1. Generate the `/data` directory.
2. Create the `radiomic_HE_dataset.csv` skeleton with the exact headers and technical mapping (e.g., `original_` prefixes) used in the study.

## Quick Start
1. **Environment**: Install dependencies via `pip install -r requirements.txt`.
2. **Setup**: Run `dataset_dictionary.ipynb` to generate the empty dataset template.
3. **Analysis**: Once the data is populated, execute `HE_Radiomics_Pipeline.ipynb` to reproduce all models and figures.

## Citation Requirements
Users and reviewers are requested to cite the Zenodo DOI associated with this record:
> *Anonymous Authors (2026). "Quantitative Brain MRI Enhances Clinical Stratification of Hepatic Encephalopathy Beyond MELD: A Multicenter Study". Software and Data Repository. Zenodo. DOI: [Insert Zenodo DOI Here]*

---
*Anonymized for Double-Blind Peer Review.*

