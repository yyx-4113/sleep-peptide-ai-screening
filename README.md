# AI-Driven Virtual Screening of Sleep-Promoting Bioactive Peptides from Fujian Medicinal-Food Ingredients

[![DOI](https://img.shields.io/badge/DOI-10.17605%2FOSF.IO%2FXXXXX-blue)](https://osf.io/XXXXX)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Complete computational pipeline integrating ensemble deep learning, multi-target docking, and a single-cell-derived virtual cell model for discovering sleep-promoting peptides from medicinal-food sources.**

---

## Overview

This repository contains all code and processed data for the manuscript:

> Yang YX. AI-Driven Virtual Screening of Sleep-Promoting Bioactive Peptides from Fujian Medicinal-Food Ingredients: Integrating Ensemble Deep Learning, Multi-Target Docking, and a Single-Cell-Derived Virtual Cell Model. *Submitted to Int J Mol Sci*. 2026.

### Pipeline Architecture

```
UniProt proteins (186) -> Virtual Digestion -> 52,517 peptides
    |
    v
468-dim Feature Encoding -> RF (AUC=0.998) + BiLSTM (AUC=0.995) -> TOP 60 candidates
    |
    v
5 Receptors (MC4R/Keap1/OX2R/5-HT1A/GABA-AR) x 25 peptides = 75 docking calculations
    |
    v
Pomc Neuron Virtual Cell (38 genes, 56 edges) -> Rescue Scores
    |
    v
Multi-Dimensional Integrated Scoring -> TOP 15 Final Candidates
```

---

## Repository Structure

```
.
├── README.md                        # This file
├── scripts/                         # All computational modules (Python 3.14)
│   ├── 01_virtual_digestion_v2.py   # Module 1: UniProt API + virtual digestion
│   ├── 02_peptide_features.py       # Module 2: 468-dim feature encoding
│   ├── 03_virtual_cell_model.py     # Module 3: Pomc GRN + receptor-TF mapping
│   ├── 03b_virtual_cell_enhanced.py # Module 3b: Enhanced 56-edge GRN
│   ├── 04a_rf_model_training.py     # Module 4a: Random Forest training
│   ├── 04b_bilstm_fast.py           # Module 4b: BiLSTM-Attention training
│   ├── 05_molecular_docking.py      # Module 5: Receptor prep + ligand generation
│   ├── 05f_final_docking.py         # Module 5f: Batch Vina docking execution
│   ├── 05g_ox2r_5ht1a_docking.py    # Module 5g: OX2R + 5-HT1A docking
│   ├── 06_integrated_scoring.py     # Module 6: Multi-dimensional ranking
│   ├── 07_figures_and_tables.py     # Publication figures generation
│   ├── 08_md_preparation.py         # GROMACS MD input file preparation
│   ├── 09_paper_figures.py          # Paper-specific figures
│   ├── pdbqt_utils.py               # PDBQT format conversion utilities
│   └── 10_graphical_abstract.py     # Graphical abstract generation
├── data/                            # Key processed data files
│   ├── all_peptides_merged.csv      # 52,517-peptide library
│   ├── docking_energy_matrix.csv    # Peptide x receptor binding energies
│   ├── top15_final_candidates.csv   # TOP 15 ranked candidates
│   ├── pomc_grn_adjacency.csv       # GRN adjacency matrix
│   └── ...                          # Additional data files
└── results/                         # Full results (mirrors tcm/ structure)
```

---

## Dependencies

```
Python >= 3.10
numpy, scipy, pandas, scikit-learn >= 1.8
PyTorch >= 2.0
RDKit >= 2023.03
AutoDock Vina >= 1.2.0 (external executable)
```

Install Python dependencies:
```bash
pip install numpy scipy pandas scikit-learn torch rdkit matplotlib shap optuna
```

Download AutoDock Vina: https://github.com/ccsb-scripps/AutoDock-Vina/releases

---

## Quick Start

### 1. Peptide Library Construction (Module 1)
```bash
python scripts/01_virtual_digestion_v2.py
# Output: data/all_peptides_merged.csv (52,517 peptides)
```

### 2. Feature Engineering (Module 2)
```bash
python scripts/02_peptide_features.py
# Output: 468-dim feature matrix + BIOPEP labels
```

### 3. AI Model Training (Module 4)
```bash
python scripts/04a_rf_model_training.py   # Random Forest
python scripts/04b_bilstm_fast.py         # BiLSTM-Attention
# Output: trained models + TOP 60 predictions
```

### 4. Molecular Docking (Module 5)
```bash
# Ensure vina.exe is in scripts/ or on PATH
python scripts/05f_final_docking.py
# Output: docking_energy_matrix.csv
```

### 5. Virtual Cell Model (Module 3)
```bash
python scripts/03b_virtual_cell_enhanced.py
# Output: Rescue scores + sensitivity analysis
```

### 6. Integrated Scoring (Module 6)
```bash
python scripts/06_integrated_scoring.py
# Output: TOP 15 final candidates
```

---

## Data Sources

All raw data from public repositories:
- **UniProtKB**: https://www.uniprot.org/ (accessed 2026-05-29)
- **PDB**: https://www.rcsb.org/ (6W25, 2FLU, 7TAC, 7E2Z, 6D6T)
- **GEO**: GSE137665 (Jha et al., Nat Commun 2022)
- **BIOPEP**: https://biochemia.uwm.edu.pl/biopep/

---

## Citation

If you use this code or data in your research, please cite:

```
Yang YX. AI-Driven Virtual Screening of Sleep-Promoting Bioactive Peptides 
from Fujian Medicinal-Food Ingredients. Int J Mol Sci. 2026.
```

Pre-registration: https://osf.io/XXXXX

---

## License

MIT License. See LICENSE file for details.

---

## Contact

Yang Yongxin (杨永新)
Fujian University of Traditional Chinese Medicine Affiliated Second People's Hospital
[Email]
[ORCID]
