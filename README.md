# Concrete Compressive Strength Prediction

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

<p>Concrete is the most important material in civil engineering. This project aims to leverage machine learning to predict its compressive strength, enabling better material optimization, reduced testing costs, and more sustainable construction practices.</p>

## Project Overview

This project focuses on developing accurate machine learning models to predict the compressive strength of concrete based on its mixture components and age. The core purpose is to facilitate the optimization of concrete mixture designs, minimize expensive and time-consuming physical laboratory tests (which traditionally take 28 days), and contribute to sustainable construction by safely reducing expensive cement usage.

The project rigorously follows the CRISP-DM (Cross-Industry Standard Process for Data Mining) methodology, ensuring a structured and comprehensive approach from the initial understanding of the business problem to the final evaluation of the deployed solution.

---

## CRISP-DM Methodology

### 1. Business Understanding (Refer to `notebooks/1_business_understanding.ipynb`)

**Objective:** To build robust predictive models for concrete compressive strength with the following goals:

- **Optimize Mixture Designs:** Provide insights for efficient selection of material proportions, leading to optimal strength and performance.
- **Reduce Testing Costs & Time:** Minimize the need for extensive physical laboratory testing, accelerating project timelines and reducing expenses.
- **Support Sustainable Construction:** Contribute to environmental sustainability by enabling more precise material use, optimizing cement content, and thereby reducing the carbon footprint of concrete production.

**Key Factors Influencing Strength:**

- **Water-Cement Ratio:** A fundamental determinant; lower ratios generally correlate with higher strength.
- **Supplementary Cementitious Materials (SCMs):** Materials like slag and fly ash are critical for early and long-term strength development, offering eco-friendly alternatives to pure cement.
- **Age:** Concrete gains strength over time, with 28 days being a standard reference point for compressive strength.

### 2. Data Understanding (Refer to `notebooks/2_data_understanding.ipynb`)

**Dataset:**

- **Source:** [Kaggle Machine Learning Repository](https://www.kaggle.com/datasets/maajdl/yeh-concret-data?select=Concrete_Data_Yeh.csv)
- **Size:** Contains 1030 unique samples of concrete mixtures.
- **Features:** 8 input features detailing the composition (e.g., cement, water, aggregates, superplasticizer, slag, flyash) and age of the concrete, along with 1 target variable (`csMPa` - compressive strength in Megapascals).

**Key Findings from Exploratory Data Analysis (EDA):**

- **Data Quality:** The dataset is exceptionally clean with zero missing values.
- **Duplicates:** Exactly 25 exact duplicate records were identified and marked for removal.
- **Outlier Strategy:** While visual exploration indicated some extreme values at the edges of the distributions, a deliberate decision was made to _preserve_ all natural data points. We explicitly bypassed strict statistical outlier removal to maintain the physical reality of edge-case concrete mixtures.

### 3. Data Preparation (Refer to `notebooks/3_data_preparation.ipynb`)

This phase involved preparing the raw data into a machine-learning-ready format:

- **Duplicate Handling:** The 25 exact duplicate records were removed to ensure data integrity and prevent model bias.
- **Data Splitting:** Data was split into an 80/20 train-test split _prior_ to any transformations to strictly prevent data leakage.
- **Feature Scaling:** All 8 input features were scaled using `StandardScaler` (fitted exclusively on the training data) to ensure a fair, apples-to-apples comparison across algorithms. The target variable was left unscaled to keep predictions in native MPa units.
- **Interpretability Preservation:** Dimensionality reduction techniques like PCA were intentionally bypassed to maintain 100% original feature interpretability for civil engineering stakeholders.

### 4. Modeling (Refer to `notebooks/4_modeling.ipynb`)

Three machine learning regression models were implemented, tuned, and evaluated:

1.  **Linear Regression:** Used as a baseline model to prove the non-linear complexity of the problem.
2.  **Decision Tree Regressor:** A non-linear tree model tuned via `GridSearchCV`.
3.  **Random Forest Regressor:** A robust ensemble method optimized via `GridSearchCV`.

**Bias-Variance Diagnosis & Regularization:**
During evaluation, it was discovered that a standard Random Forest was over-fitting the training data (Train R²: ~0.98 vs Test R²: ~0.87). To address this, strict hyperparameter constraints (reducing `max_depth` and increasing `min_samples_leaf`) were applied to regularize the model, successfully closing the overfitting gap and improving real-world generalization.

### 5. Evaluation (Refer to `notebooks/5_evaluation.ipynb`)

**Summary of Model Performance:**

- **Best Performing Model:** Regularized Random Forest Regressor
- **Test R² Score:** ~0.90
- **Test RMSE:** ~5.42 MPa
- **Train R² Score:** ~0.96 _(Showing a healthy, reduced overfitting gap)_

**Explainable AI (Feature Importance):**
The regularized model confirmed that **Age** and **Cement** content are the primary drivers of compressive strength, followed closely by **Water** content. Aggregates (fine and coarse) were proven to act mostly as filler volume with minimal direct impact on the chemical curing strength.

---

## Key Results & Business Value Achieved

The project successfully developed a highly accurate predictive model that acts as a **"Virtual Testing Lab"**.

- **Cost Savings & Speed:** The model effectively predicts strength within a tight ~5.42 MPa margin of error, allowing mix designers to simulate strength instantly rather than waiting 28 days for physical cylinders to cure.
- **Sustainability:** Engineers can use the model to confidently substitute expensive, high-carbon Portland cement for eco-friendly alternatives (Slag/Fly ash) while mathematically guaranteeing structural safety.

## Limitations & Future Improvements

- **Environmental Factors:** The dataset assumes standard laboratory curing conditions. Future iterations should collect and include environmental variables like curing temperature and ambient humidity.
- **Deployment Architecture:** The next planned phase is to deploy the saved `.pkl` model into a Microservices Architecture. The Python model will be wrapped in a **FastAPI** service, orchestrated by a **Java Spring Boot** backend, and served to end-users via a reactive **Angular** frontend UI, all containerized using **Docker**.

---

## Project Organization

```text
├── .gitignore         <- Files to be ignored by git
├── Makefile           <- Makefile with convenience commands
├── README.md          <- The top-level README for developers using this project.
├── pyproject.toml     <- Project configuration file
├── requirements.txt   <- The requirements file for reproducing the analysis environment
├── test_data.py       <- Script/tests for data validation
│
├── concrete_compressive_strength <- Source code for use in this project
│
├── data
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default mkdocs project; see www.mkdocs.org for details
│
├── figures            <- Generated graphics and charts (e.g., Feature Importances, Residuals)
│
├── models             <- Trained and serialized models (e.g., standard_scaler.pkl, random_forest_model.pkl)
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         and a short `-` delimited description (e.g., `1_business_understanding.ipynb`).
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
└── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
```
