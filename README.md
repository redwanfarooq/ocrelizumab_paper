# Ocrelizumab Manuscript Reproducibility Repository

This repository contains the analysis code and workflow for the manuscript:

**Multimodal discovery of a pathogenic B cell-dependent T cell state in multiple sclerosis**  
*Redwan Farooq, Brian Cutler, Marco Pisa, Subita Balaram Kuttikkatte, Jacqueline Palace, Gabriele C DeLuca, Ralf Gold, Fabian J Theis, Lars Fugger, Kathrine E Attfield*

## Overview

This repository provides the code, configuration files and software environment specifications necessary to reproduce the analyses presented in the manuscript. The workflow is organized into four main stages:
1. Raw data preprocessing, quality control, normalisation and integration of CITE-seq datasets generated for this study
2. Preprocessing, normalisation and integration of published external datasets used for validation analyses
3. Clustering and cell type annotation
4. Downstream analyses

## Repository Structure

```
ocrelizumab_paper/
├── 01_prepare_internal_datasets/   # Snakemake pipelines for preprocessing, QC, normalisation and integration of internal datasets
├── 02_prepare_external_datasets/   # Notebooks for preprocessing, normalisation and integration of external datasets
├── 03_annotate/                    # Notebooks for clustering and cell type annotation
├── 04_analyse/                     # Notebooks for downstream analyses
├── data/                           # Data directory
├── envs/                           # Conda environment specifications
├── results/                        # Analysis outputs
├── scripts/                        # Utility scripts
└── slurm/                          # SLURM job submission scripts
```

## Prerequisites

### System Requirements
- Linux operating system (tested on Ubuntu 22.04)
- High-performance computing cluster with NVIDIA CUDA-compatible GPU recommended (tested on [MRC WIMM JADE HPC cluster](https://www.imm.ox.ac.uk/facilities/ccb-high-performance-computing/jade-hpc-cluster))

### Software Dependencies

#### Snakemake Pipelines
The Snakemake pipelines in [`01_prepare_internal_datasets/`](01_prepare_internal_datasets/) contain their own Conda environment specifications and installation instructions.

#### Global Environment
All software dependencies for the Jupyter notebooks are specified in [`envs/environment.yaml`](envs/environment.yaml)

To create the Conda environment, first install Anaconda or Miniconda (see [installation instructions](https://docs.conda.io/projects/conda/en/stable/user-guide/install/index.html)), then run:
```bash
conda env create -f envs/environment.yaml
conda activate ocrelizumab_paper
```

Next, to install the Jupyter kernel for running R notebooks, run:
```bash
Rscript -e 'IRkernel::installspec(name = "ir-ocrelizumab_paper", displayname = "R 4.3 (ocrelizumab_paper)")'
```

## Data Availability

### Input Data
Internal datasets:
- FASTQ files for internal CITE-seq datasets will be deposited in EGA (accession pending)
- Processed data for internal CITE-seq datasets have been deposited in GEO (accession GSE316688):
    - Unfiltered per-sample GEX and ADT count matrices
    - QC-filtered and annotated datasets (Seurat/RDS and MuData/H5MU formats) - download to `data/processed/cite_seq/cohort_treatment_naive/annotated/` and `data/processed/cite_seq/cohort_nonresponders/annotated/`

External datasets:
- Cantoni et al. (2025): Synapse (accession syn51730532) - download to `data/processed/external/cantoni/source/`
- Hayashi et al. (2026): GEO (accession GSE133028 and GSE291328) - download to `data/processed/external/hayashi/source/`
- Absinta et al. (2021): GEO (accession GSE180759) - download to `data/processed/external/lesion_rims/absinta/source/`
- Lerma-Martin et al. (2024): EGA (accession EGAC50000000231) - download to `data/processed/external/lesion_rims/lerma_martin/source/`
- Kaufmann et al. (2021): GEO (accession GSE144744) - download to `data/processed/external/kaufmann/source/`

### Metadata

Internal datasets:
- Anonymised donor-level metadata for internal CITE-seq datasets are provided in [`data/metadata/`](data/metadata/).

External datasets:
- Additional manually-curated metadata for external datasets (from supplementary materials of original publications) are provided in [`data/processed/external/`](data/processed/external/).

### Reference Data

Specific reference data files used in the analyses are provided in [`data/ref/`](data/ref/). Additional reference files (e.g. public datasets and CellTypist models for reference mapping and cell type annotation as specified in notebooks) should be downloaded to this directory.

### cNMF Gene Programs and starCAT Spectra

Precomputed cNMF gene programs and starCAT spectra from the discovery cohort dataset are provided in [`results/tables/cite_seq/cohort_treatment_naive/gep/`](results/tables/cite_seq/cohort_treatment_naive/gep/).

## General Notes

- All Snakemake pipelines and notebooks are numbered and should be executed in order; subsequent stages depend on outputs from previous stages.
- Computationally intensive analysis steps were run on an HPC cluster using SLURM job submission scripts provided in the [`slurm/`](slurm/) directory; these steps are indicated within notebooks at the relevant points. These shell scripts will need to be adapted to the specific HPC environment and job scheduler in use.
- Relative file paths are used in all notebooks; ensure that the repository structure is maintained when running the code. Shell scripts include a `BASEDIR` variable that should be set to the root directory of the copy of repository on the local system.

## Index

The following table indicates the notebooks that generate each figure, table and data file included in the manuscript:

| Figure/Table/File         | Notebook                                                                                   |
|---------------------------|-------------------------------------------------------------------------------------------|
| Fig. 1B                   | [`04_analyse/08_clinical_demographics.ipynb`](04_analyse/08_clinical_demographics.ipynb)   |
| Fig. 1C-G                 | [`04_analyse/09_cohort_treatment_naive_global.ipynb`](04_analyse/09_cohort_treatment_naive_global.ipynb) |
| Fig. 3A-D                 | [`04_analyse/10_cohort_treatment_naive_b_cells.ipynb`](04_analyse/10_cohort_treatment_naive_b_cells.ipynb) |
| Fig. 4A-F                 | [`04_analyse/11_cohort_treatment_naive_t_cells.ipynb`](04_analyse/11_cohort_treatment_naive_t_cells.ipynb) |
| Fig. 5B-C                 | [`04_analyse/15_csf_validation.ipynb`](04_analyse/15_csf_validation.ipynb)                 |
| Fig. 5E                   | [`04_analyse/16_tcr_clonotype_validation.ipynb`](04_analyse/16_tcr_clonotype_validation.ipynb) |
| Fig. 5G                   | [`04_analyse/17_brain_tissue_validation.ipynb`](04_analyse/17_brain_tissue_validation.ipynb) |
| Fig. 6B-D                 | [`04_analyse/18_natalizumab_validation.ipynb`](04_analyse/18_natalizumab_validation.ipynb) |
| Supplementary Fig. 1      | [`04_analyse/01_meld.ipynb`](04_analyse/01_meld.ipynb)                                     |
| Supplementary Fig. 2      | [`04_analyse/02_cnmf.ipynb`](04_analyse/02_cnmf.ipynb)                                     |
| Supplementary Fig. 3,5-8  | [`04_analyse/03_gep_visualisation.ipynb`](04_analyse/03_gep_visualisation.ipynb)          |
| Supplementary Fig. 4A-C   | [`04_analyse/09_cohort_treatment_naive_global.ipynb`](04_analyse/09_cohort_treatment_naive_global.ipynb) |
| Supplementary Fig. 9A-C   | [`04_analyse/11_cohort_treatment_naive_t_cells.ipynb`](04_analyse/11_cohort_treatment_naive_t_cells.ipynb) |
| Supplementary Fig. 10A-C  | [`04_analyse/12_cohort_treatment_naive_cd20dim_t_cells.ipynb`](04_analyse/12_cohort_treatment_naive_cd20dim_t_cells.ipynb) |
| Supplementary Fig. 11A-B  | [`04_analyse/14_cohort_nonresponders.ipynb`](04_analyse/14_cohort_nonresponders.ipynb)    |
| Supplementary Fig. 12A-E  | [`04_analyse/15_csf_validation.ipynb`](04_analyse/15_csf_validation.ipynb)                |
| Supplementary Fig. 13A-D  | [`04_analyse/16_tcr_clonality_validation.ipynb`](04_analyse/16_tcr_clonality_validation.ipynb) |
| Supplementary Fig. 14A-D  | [`04_analyse/17_brain_tissue_validation.ipynb`](04_analyse/17_brain_tissue_validation.ipynb) |
| Supplementary Fig. 15A-B  | [`04_analyse/18_natalizumab_validation.ipynb`](04_analyse/18_natalizumab_validation.ipynb) |
| Supplementary Fig. 16-17  | [`04_analyse/06_shap_program_perturbation.ipynb`](04_analyse/06_shap_program_perturbation.ipynb) |
| Supplementary Table 1     | [`04_analyse/08_clinical_demographics.ipynb`](04_analyse/08_clinical_demographics.ipynb)   |
| Supplementary Table 2     | [`03_annotate/09_cluster_names.ipynb`](03_annotate/09_cluster_names.ipynb)                |
| Supplementary Data 1-2    | [`03_annotate/02_subcluster_annotate_cohort_treatment_naive.ipynb`](03_annotate/02_subcluster_annotate_cohort_treatment_naive.ipynb) |
| Supplementary Data 3      | [`04_analyse/02_cnmf.ipynb`](04_analyse/02_cnmf.ipynb)                                     |
| Supplementary Data 4      | [`04_analyse/04_gep_pathway_enrichment.ipynb`](04_analyse/04_gep_pathway_enrichment.ipynb) |
| Supplementary Data 5      | [`04_analyse/07_totalvi.ipynb`](04_analyse/07_totalvi.ipynb)                              |


## License

All code in this repository is published under the MIT License (see the [LICENSE](LICENSE) file for details).

## Contact

For questions about the code, please contact Redwan Farooq ([redwan.farooq@ndcn.ox.ac.uk](mailto:redwan.farooq@ndcn.ox.ac.uk)).

---

**Last updated**: 10/07/2026