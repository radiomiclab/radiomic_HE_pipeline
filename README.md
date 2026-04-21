# Globus Pallidus T1-Weighted MRI Radiomics for HE Grading

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.[ID].svg)](https://doi.org/10.5281/zenodo.[ID])

> **Status**: manuscript under review  
> **Pipeline version**: 1.0  
> **Generated**: 2026-04-21

---

## Citation

If you use this pipeline, please cite:

> [Authors]. *[Title]*. *[Journal]*, [Year]. DOI: [TBD upon acceptance]

---

## Overview

Fully reproducible machine learning pipeline for non-invasive stratification
of hepatic encephalopathy (HE) severity using T1-weighted MRI radiomic
features of the globus pallidus in cirrhotic patients.

The globus pallidus undergoes structural changes in cirrhosis driven by a
combination of pathological processes: primarily manganese deposition via
porto-systemic shunting, but also degenerative phenomena including astrocytic
oedema and gliosis. This pipeline quantifies whether IBSI-compliant texture
features of this composite signal discriminate HE severity grades.

---

## Study Design

### Outcome definitions

| Outcome | Positive class | Negative class | WHC grades | N | Role |
|---------|---------------|----------------|------------|---|------|
| **O0** | Cirrhosis No HE | Healthy Controls | — | 148 | Biological signal specificity |
| **O1** | Mild HE | No overt HE | WHC 1 vs WHC 0 | 123 | **Primary** |
| **O2** | Moderate-to-Severe HE | Mild HE | WHC ≥ 2 vs WHC 1 | 85 | Exploratory (EPV < 10) |

### Label conversion (WHC → binary)

| WHC grade | Clinical description | O1 label | O2 label |
|-----------|---------------------|----------|----------|
| 0 | No overt HE | 0 (negative) | — (excluded) |
| 1 | Mild (covert) HE | 1 (positive) | 0 (negative) |
| 2 | Moderate HE | — (excluded) | 1 (positive) |
| 3 | Severe HE | — (excluded) | 1 (positive) |

**O0** establishes that radiomic differences reflect the structural changes
of the globus pallidus in cirrhosis (manganese deposition, oedema, gliosis)
rather than HE-specific neurological changes per se.

**O1** is the primary outcome: detection of mild (covert) HE using globus
pallidus radiomics as a non-invasive pre-transplant stratification tool.

**O2** is exploratory due to EPV < 10 and zero bootstrap feature stability;
results are hypothesis-generating only.

---

## Important Note on Feature Extraction and Segmentation

**Radiomic feature extraction and ROI segmentation are not part of this
pipeline.** Each research group uses its own MRI acquisition protocol,
segmentation method, and extraction software. This pipeline assumes that:

1. Bilateral globus pallidus ROIs have been segmented on T1-weighted
   unenhanced MRI images following the operator's own validated procedure.
2. The same **30 IBSI-compliant radiomic features** used in the original
   study have been extracted from those ROIs using an IBSI-compliant tool
   (e.g., PyRadiomics v3.0.1, bin-width 25 HU, 3D extraction, `original_`
   prefix).
3. The extracted features are stored as CSV files with the exact column
   names documented in `dataset_dictionary.ipynb`.

**Using a different feature set or extraction configuration will invalidate
the pre-trained model coefficients and the reproducibility of results.**

---

## Repository Structure

```
.
├── radiomic_pipeline_v1.ipynb       # Main analysis pipeline (Cells 0-21)
├── dataset_dictionary.ipynb         # Column schema, IBSI codes, CSV templates
├── requirements.txt
├── config.yaml
├── README.md
├── LICENSE
├── data/
│   ├── radiomic_HE_dataset.csv         # Internal cohort (n=168)
│   ├── healthy_signa.csv               # Healthy controls — Signa 1.5T
│   ├── healthy_discovery.csv           # Healthy controls — Discovery 3.0T
│   ├── radiomic_HE_dataset_extA.csv    # External validation cohort A
│   └── radiomic_HE_dataset_extB.csv    # External validation cohort B
└── outputs/
    ├── figures/                         # TIFF + PDF (colour + grayscale)
    ├── tables/                          # DOCX submission tables
    ├── csv/                             # All CSV data exports
    └── manuscript_snapshot.json
```

---

## Requirements

```bash
python >= 3.9   # tested on 3.11.14
pip install -r requirements.txt
```

---

## Usage

### Step 1 — Create empty dataset templates

Open and run `dataset_dictionary.ipynb`. This creates the `./data/`
subdirectory and generates five empty CSV templates with the correct
39-column header.

### Step 2 — Populate with your own data

Fill in each CSV following the column schema in `dataset_dictionary.ipynb`.
Run the validation cell to verify the schema before proceeding.

### Step 3 — Run the pipeline

```bash
pip install -r requirements.txt   # once
jupyter notebook radiomic_pipeline_v1.ipynb
```

Execute all cells sequentially (Cell 0 → Cell 21). All figures and
statistical tables are displayed inline in the notebook.
Cell 21 generates `requirements.txt`, `config.yaml`, and this `README.md`.

> **Note on `config.yaml`:** this file is a documentation artefact for
> reviewers and collaborators — it is **not** read programmatically by the
> pipeline. All parameters are defined directly in Cell 1 of the pipeline
> notebook. If you need to change a parameter (e.g., scanner name, seed,
> number of bootstrap resamples), edit Cell 1; then re-run Cell 21 to
> regenerate a consistent `config.yaml`.

---

## Cell Map

| Cell | Content |
|------|---------|
| 0 | Environment setup |
| 1 | Imports, seeds, constants, helpers |
| 2 | Table 1: baseline characteristics |
| 3 | Data loading: internal cohort |
| 4 | Data loading: healthy controls (O0) |
| 5 | EDA: cohort characterisation + age–feature correlation |
| 6 | Subset creation O0, O1, O2 |
| 7 | Nested CV + ComBat + EPV fix (all outcomes) |
| 8 | Performance metrics + DeLong + calibration |
| 9 | Calibration reliability diagrams |
| 10 | Decision Curve Analysis — Fig 4 |
| 11 | SHAP feature importance — Fig 3 |
| 12 | Bootstrap LASSO stability |
| 13 | AUC grouped bar chart — Fig 2 |
| 14 | Cross-system validation |
| 15 | External validation (Ext-A, Ext-B) |
| 16 | Domain shift analysis |
| 17 | ComBat diagnostics |
| 18 | RadScore equations |
| 19 | Supplementary Table ST1 |
| 20 | Reproducibility check + session info |
| **21** | **Repository files (requirements.txt, config.yaml, README.md)** |

---

## Compliance

| Standard | Implementation |
|----------|----------------|
| **TRIPOD-AI** (Collins et al., *BMJ* 2024) | Items documented per cell header |
| **IBSI** (Zwanenburg et al., *Radiology* 2020) | PyRadiomics 3.0.1, prefix `original_`, bin-width 25 HU, 3D |
| **PEP 8** | snake_case, 79-char lines, Google-style docstrings |
| **Open science** | requirements.txt, config.yaml, README.md, snapshot JSON |

---

## Feature Selection Rationale

30 IBSI-compliant features were selected from first-order statistics and
four texture matrix families (GLCM, GLDM, GLRLM, GLSZM). Wavelet-derived
and shape features were deliberately excluded: wavelet features depend on
decomposition parameters not universally standardised across software, while
shape features are not appropriate for a signal driven by paramagnetic
deposition and degenerative tissue changes (oedema, gliosis). This
conservative selection prioritises institutional transferability — any centre
with PyRadiomics >= 3.0 or equivalent IBSI-compliant software can replicate
extraction without custom configuration.

---

## Biological Signal Specificity (Outcome 0)

Outcome 0 (n=148) establishes that radiomic differences reflect the
structural changes of the globus pallidus in cirrhosis (manganese deposition,
oedema, gliosis), not HE-specific neurological changes per se.
Age-feature correlation analysis in healthy controls confirmed that no
features reached FDR significance (0/30 features FDR-significant); the
three dominant O1 features showed no age correlation (all |r| <= 0.17,
all p >= 0.13).

---

## Reproducibility

| Parameter | Value |
|-----------|-------|
| `RANDOM_STATE` | 42 |
| `BOOT_SEED` | 42 |
| `PYTHONHASHSEED` | 42 |
| CV folds | 5 |
| Bootstrap resamples | 2000 |
| Permutation iterations | 2000 |

---

## License

MIT License — see `LICENSE` for details.

---
*Auto-generated by Cell 21 on 2026-04-21 15:06*
