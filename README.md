##  Project Overview
This repository contains the official codebase for our DDA4210 course project. We aim to transcend the limitations of traditional predictive machine learning by employing **Causal Machine Learning** frameworks. Specifically, we investigate the underlying structural causal mechanisms between modern lifestyle factors (e.g., sleep deprivation, sedentary behavior) and the prevalence of depression, adjusting for complex unobserved confounders.

# Quantifying-the-Effects-of-Sedentary-Behavior-and-Sleep-on-Depression-using-Causal-Machine-Learning
This project aims to overcome the limitations of traditional machine learning in medical data—which tends to prioritize prediction over causality—and to deeply explore the true causal links between modern lifestyles (such as sleep deprivation and sedentary behavior) and the incidence of depression.
Data Access:
The raw data for this project is sourced from NHANES (2017-2018). Due to size limitations, the data files are not included in this repository.
To reproduce our results:

Download the .XPT files from the CDC NHANES website.
Place them in a folder named data/raw/ in your Drive.
Run the notebook notebooks/01_data_merging.ipynb to generate the merged dataset.

Data Preprocessing & Advanced Imputation
Following the initial data merge, we conducted rigorous data cleaning tailored to the NHANES survey design. We recoded survey artifacts (e.g., "Refused" or "Don't know") as missing values (`NaN`) to prevent extreme bias, and derived our continuous target variable, `PHQ9_Score`. 

Crucially, to handle missing covariates without discarding valuable samples, we implemented **MICE (Multiple Imputation by Chained Equations)** via `sklearn.IterativeImputer`. This advanced machine learning technique preserves the statistical integrity of our dataset. We also conducted exploratory data analysis (EDA) to visualize baseline distributions and superficial correlations.

To reproduce our preprocessing results:
4. Run the notebook `notebooks/data_cleaning.ipynb` to filter invalid survey codes and generate the target variable.
5. Run the notebook `notebooks/imputation_and_eda.ipynb` to perform MICE imputation, view baseline plots, and generate the final `NHANES_Ready_for_Causal.csv`.

Causal Discovery & Directed Acyclic Graph (DAG) Generation
To uncover the hidden causal structure rather than mere correlations, we applied the **PC Algorithm** using the `causal-learn` library. 

**Algorithm Correction via Background Knowledge:**
Our purely data-driven PC algorithm initially oriented arrows toward invariant demographic traits. To resolve this, we injected **Domain Background Knowledge**, strictly forbidding any edges pointing towards `Age` and `Gender`. This resulted in a logically sound DAG.

**Key Finding: The "Reverse Causality" Paradigm**
The corrected DAG revealed a profound insight that challenges conventional assumptions. Instead of lifestyle choices causing depression, the algorithm strongly identified **Depression (`PHQ9_Score`) as the causal parent of lifestyle disruptions (`Sleep_Hours` and `Sedentary_Mins`)**. Clinically, this aligns with psychiatric literature: depressive episodes inherently induce insomnia and psychomotor retardation (sedentary behavior), cascading further into impacts on income and BMI.

To reproduce the causal graph:
6. Run the notebook `notebooks/causal_discovery.ipynb` to generate the corrected DAG.

Subgroup Analysis & Algorithmic Robustness
To elevate the academic rigor of our project, we conducted a deep dive into the causal structures by altering target definitions and validating across algorithmic families.

**1. Thresholding Effect (Severe MDD):**
We binarized the continuous PHQ-9 score using a clinical threshold ($\ge 15$) to isolate Severe Major Depressive Disorder (MDD). The resulting DAG showed a structural paradigm shift: the causal links between depression and lifestyle (sleep/sedentary) vanished, likely due to bipolar extreme variances (e.g., hypersomnia vs. insomnia). However, Severe MDD maintained highly robust causal arrows pointing directly to elevated **BMI** and reduced **Income**, highlighting the profound metabolic and socioeconomic devastation caused by severe mental illness.

**2. Cross-Validation via LiNGAM (Ongoing):**
To ensure our findings were not artifacts of the constraint-based PC algorithm, we cross-validated the causal skeleton using **DirectLiNGAM**, a Functional Causal Model (FCM) that leverages non-Gaussian data distributions.
