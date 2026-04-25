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
4. Run the notebook `notebooks/02_data_cleaning.ipynb` to filter invalid survey codes and generate the target variable.
5. Run the notebook `notebooks/03_imputation_and_eda.ipynb` to perform MICE imputation, view baseline plots, and generate the final `NHANES_Ready_for_Causal.csv`.
