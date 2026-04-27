Beyond Prediction: Quantifying the Causal Impact of Depression on Lifestyle and Metabolism using Advanced Machine Learning
Executive Summary
This project aims to overcome the limitations of traditional machine learning in medical data—which tends to prioritize prediction over causality—and to deeply explore the true causal links between depression and modern lifestyle/metabolic factors. 

Instead of assuming the conventional paradigm (e.g., "obesity and sedentary behavior cause depression"), we utilized a rigorous Causal Machine Learning pipeline. We quantified three causal pathways derived from structural learning. The effect of depression (PHQ-9) on sedentary behavior (ATE = +4.06 min/point) and on the income-to-poverty ratio (ATE = −0.067/point) was robustly confirmed by both linear back-door adjustment and Double Machine Learning (DML), surviving placebo, subset, and unobserved-confounder refutations. In contrast, the effect of binarized severe MDD on BMI was significant under IPTW (+2.13 kg/m²) but not under DML, reflecting estimation instability. However, Conditional Average Treatment Effect (CATE) analysis via Causal Forests revealed a profound heterogeneous reality: young adults (≤30) suffer severe weight gain (+1.04 BMI), while older adults experience weight loss. We present the lifestyle/socioeconomic pathways as confirmatory, and the metabolic pathway as highly heterogeneous, strictly aligning with NIH guidelines for transparent causal reporting.

Data Access & Preprocessing
The raw data is sourced from NHANES (2017-2018). Due to size limitations, the .XPT files are not included in this repository. 

Advanced Imputation Strategy:
To prepare for rigorous sensitivity analysis, we processed missing covariates using three distinct methods: Median, Linear regression, and MICE via Random Forests (RF). This allowed us to test the topological robustness of our causal discovery algorithms across different data distributions.

To reproduce our preprocessing:

Download the .XPT files from the CDC NHANES website and place them in data/raw/.
Run notebooks/data_merging.ipynb to generate the merged dataset.
Run notebooks/data_cleaning.ipynb to filter invalid survey artifacts (e.g., "Refused").
Run notebooks/imputation_and_eda.ipynb to generate the three imputed datasets and EDA visualizations.
Phase 1: Causal Discovery & Topological Robustness
To uncover the hidden causal skeleton, we conducted a massive cross-validation across three distinct algorithmic families: PC (Constraint-based), LiNGAM (Functional-based), and GES (Score-based).

The "Reverse Causality" Paradigm: Across algorithms, the data strongly rejected the assumption that lifestyle causes depression. Instead, it identified Depression (PHQ-9) as the causal parent disrupting lifestyle (Sedentary Mins) and socioeconomic status (Income Ratio).
Algorithmic Robustness & Thresholding: We tested both continuous PHQ-9 and binarized Severe MDD (Score ≥ 15). GES proved to be the most topologically robust algorithm, maintaining stable directed edges pointing from Severe MDD to BMI and Income across all three imputation datasets, whereas PC and LiNGAM failed the stability test.
To reproduce the causal graphs:
5. Run notebooks/causal_discovery.ipynb and causal_v2.ipynb.

Phase 2: Causal Inference & Strict Refutations (DoWhy)
Based on the GES-generated Directed Acyclic Graph (DAG), we utilized the Microsoft DoWhy framework to quantify the Average Treatment Effect (ATE) using Linear Regression and Inverse Probability of Treatment Weighting (IPTW).

Path 1 (Behavioral): PHQ9 
→
→ Sedentary Mins (ATE = +4.06 min per PHQ9 point).
Path 2 (Socioeconomic): PHQ9 
→
→ Income Ratio (ATE = -0.067 per PHQ9 point).
Refutations: Both pathways perfectly survived Placebo tests (effect dropped to 0), Subset refutations (removing 20% data), and the Unobserved Common Cause test.
To reproduce the DoWhy inference:
6. Run notebooks/advanced_causal_econml.ipynb.

Phase 3: The Metabolic Plot Twist (Double Machine Learning)
For the Severe_MDD -> BMI pathway, basic IPTW suggested an alarming ATE of +2.13. However, as an Advanced Machine Learning project, we challenged this linear assumption by deploying Double Machine Learning (DML) via Causal Forests (EconML library) to strip away complex non-linear confounders.

Key Finding: The Power of CATE (Heterogeneous Effects)
DML revealed that the overall ATE crossed zero (not statistically significant). However, the Conditional Average Treatment Effect (CATE) uncovered why:

Young Adults (≤30): +1.04 BMI (Severe depression induces stress-eating and metabolic disruption).
Older Adults (≥60): -0.22 BMI (Severe depression induces anorexia and muscle atrophy).
The overall effect was cancelled out by this extreme age-based heterogeneity. This highlights the ultimate value of Causal ML: Treatment and interventions must be targeted, not one-size-fits-all.

To reproduce the DML & CATE analysis:
7. Run notebooks/advanced_causal_econml.ipynb.
