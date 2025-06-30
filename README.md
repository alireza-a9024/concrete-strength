# Concrete Compressive Strength

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>
<p>
Concrete is the most important material in civil engineering. This project aims to leverage machine learning to predict its compressive strength, enabling better material optimization, reduced testing costs, and more sustainable construction practices.</p>

<h1>Project Overview</h1>
<p>This project focuses on developing accurate machine learning models to predict the compressive strength of concrete based on its mixture components and age. The core purpose is to facilitate the optimization of concrete mixture designs, minimize expensive and time-consuming physical laboratory tests, and contribute to sustainable construction by reducing material waste.</p>

<p>The project rigorously follows the CRISP-DM (Cross-Industry Standard Process for Data Mining) methodology, ensuring a structured and comprehensive approach from the initial understanding of the business problem to the final evaluation of the deployed solution.</p>

<h1>CRISP-DM Methodology</h1>

<h2>1. Business Understanding (Refer to notebooks/1_business_understanding.ipynb)</h2>

Objective: To build robust predictive models for concrete compressive strength with the following goals:

Optimize Mixture Designs: Provide insights for efficient selection of material proportions, leading to optimal strength and performance.

Reduce Testing Costs & Time: Minimize the need for extensive physical laboratory testing, accelerating project timelines and reducing expenses.

Ensure Structural Safety & Reliability: Improve prediction accuracy to enhance the safety and long-term reliability of concrete structures, a critical aspect given the material's widespread use.

Support Sustainable Construction: Contribute to environmental sustainability by enabling more precise material use, optimizing cement content, and thereby reducing the carbon footprint of concrete production.

Key Factors Influencing Strength (Identified in this phase):

Water-Cement Ratio: A fundamental determinant; lower ratios generally correlate with higher strength.

Supplementary Cementitious Materials (SCMs): Materials like slag and flyash are critical for early and long-term strength development, and their precise proportioning is key.

Age: Concrete gains strength over time, with 28 days being a standard reference point for compressive strength.

<h2>2. Data Understanding (Refer to notebooks/2_data_understanding.ipynb)</h2>

Dataset:

Source: [Kaggle.com Machine Learning Repository](https://www.kaggle.com/datasets/maajdl/yeh-concret-data?select=Concrete_Data_Yeh.csv)

Size: Contains 1030 unique samples of concrete mixtures.

Features: Comprises 8 input features detailing the composition (e.g., cement, water, aggregates, superplasticizer, slag, flyash) and age of the concrete, along with 1 target variable (csMPa - compressive strength in Megapascals).

Key Findings from Exploratory Data Analysis (EDA):

Data Quality: The dataset was found to be complete with no missing values.

Duplicates: A small number of exact duplicate records were identified.

Distributions: Features such as age, slag, and flyash exhibited right-skewed distributions, while others like cement showed relatively normal distributions. The target variable, csMPa, spanned a range from 2.33 to 82.6 MPa.

Outliers: Outliers were detected using IQR and Z-score methods. A deliberate decision was made to retain these outliers, as they likely represent valid, albeit extreme, experimental data points crucial for capturing the full variability of real-world concrete mixes.

Correlations: Strong positive correlations were observed between cement content and strength, and age and strength. Conversely, water content showed a negative correlation with strength. Several non-linear relationships were also evident, suggesting the need for models capable of capturing such complexities.

<h2>3. Data Preparation (Refer to notebooks/3_data_preparation.ipynb)</h2>

This phase involved preparing the raw data for effective model training.

Duplicate Handling: Only exact duplicate records were removed to ensure data integrity and avoid redundancy. As previously noted, outliers and near-duplicates were preserved.

Feature Scaling: All input features (excluding the target csMPa) were scaled using StandardScaler. This transformation standardizes features to a mean of 0 and a standard deviation of 1, which is vital for optimizing algorithms sensitive to feature magnitudes (e.g., Linear Regression, Neural Networks).

PCA Analysis: Principal Component Analysis was performed to explore potential dimensionality reduction. The cumulative explained variance by principal components was analyzed, and a PCA-transformed dataset (specifically using the first 6 principal components which captured a significant portion of variance) was generated for use in certain modeling approaches (e.g., Linear Regression on PCA components).

<h2>4. Modeling (Refer to notebooks/4_modeling.ipynb)</h2>

A variety of machine learning regression models were implemented, trained, and compared for their ability to accurately predict concrete compressive strength. The models investigated include:

Linear Regression with PCA: Applied to the dataset reduced by PCA (using the top 6 principal components).

Random Forest Regressor: A robust ensemble method trained on the original, scaled features.

Decision Tree Regressor: A single tree model trained on the original, scaled features.

Neural Network (MLP): A multi-layer perceptron implemented using Keras, trained on the original, scaled features.

Evaluation Metrics:
Model performance was primarily assessed using:

R² Score (Coefficient of Determination): Measures the proportion of variance in the dependent variable that is predictable from the independent variables. A higher R² indicates a better fit.

RMSE (Root Mean Squared Error): Quantifies the average magnitude of the errors. RMSE is in the same units as the target variable (MPa) and significantly penalizes larger errors. This metric was prioritized over MAE (Mean Absolute Error) due to the safety-critical nature of concrete strength prediction, where substantial prediction errors can have severe consequences. RMSE's continuous differentiability also makes it well-suited for gradient-based optimization algorithms used in models like Neural Networks.

<h2>5. Evaluation(Refer to notebooks/5_evaluation.ipynb)</h2>

This final stage involved a comprehensive assessment of the models from both a technical and business perspective.

Summary of Model Performance:
(Replace with your specific best model and its scores from your 4_modeling.ipynb notebook output)

Best Performing Model: Random Forest Regressor

R² Score: 0.9073

RMSE: 5.2575 MPa

The Random Forest Regressor consistently demonstrated the highest predictive accuracy and robustness across the evaluation metrics.

<h1>Key Results</h1>
The project successfully developed a highly accurate predictive model for concrete compressive strength. The Random Forest Regressor emerged as the top performer, exhibiting excellent generalization capabilities with a high R² score and a low RMSE. This indicates its strong ability to predict concrete strength reliably even for unseen concrete mixture designs.

Significant Business Value Achieved:

Cost Savings: The model significantly reduces the reliance on costly and time-intensive physical laboratory testing.

Quality Improvements: Provides more consistent and accurate predictions of concrete strength, enhancing quality control throughout the construction process.

Sustainability Impact: Enables optimization of material usage, particularly cement, leading to reduced waste and a lower carbon footprint in concrete production.

<h1>Limitations</h1>
Accuracy for High Strength Concrete: The model's predictive accuracy was observed to be slightly reduced for very high strength concrete mixes, potentially due to fewer samples in this range in the training data.

Limited Extrapolation: The model's performance might degrade when applied to concrete mixtures that are significantly outside the range of the chemical compositions or ages present in the training dataset.

Environmental Factors: Critical external variables such as specific curing conditions (temperature, humidity) and other environmental factors, which are known to impact concrete strength, were not available in the dataset and thus not incorporated into the models.

<h1>Future Improvements</h1>
Data Collection & Enrichment:

Prioritize gathering more diverse data, especially for high-strength concrete formulations.

Integrate environmental conditions (e.g., curing temperature and humidity profiles) into the dataset.

Include additional durability indicators (e.g., permeability, shrinkage) to enable broader predictions.

Model Enhancements:

Explore advanced ensemble methods (e.g., Gradient Boosting Machines like XGBoost or LightGBM, or Stacking) for potentially further improved performance.

Investigate more complex deep learning architectures if a larger and more varied dataset becomes available.

Consider time series analysis techniques if granular, time-dependent strength development data becomes accessible.

Deployment & Integration:

Develop a user-friendly web-based prediction interface or a mobile application for practical use in the field.

Integrate the predictive model directly with existing concrete mix design software to streamline engineering workflows.

## Project Organization

```
├── LICENSE            <- Open-source license if one is chosen
├── Makefile           <- Makefile with convenience commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default mkdocs project; see www.mkdocs.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── pyproject.toml     <- Project configuration file with package metadata for
│                         concrete_compressive_strength and configuration for tools like black
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.cfg          <- Configuration file for flake8
│
└── concrete_compressive_strength   <- Source code for use in this project.
    │
    ├── __init__.py             <- Makes concrete_compressive_strength a Python module
    │
    ├── config.py               <- Store useful variables and configuration
    │
    ├── dataset.py              <- Scripts to download or generate data
    │
    ├── features.py             <- Code to create features for modeling
    │
    ├── modeling
    │   ├── __init__.py
    │   ├── predict.py          <- Code to run model inference with trained models
    │   └── train.py            <- Code to train models
    │
    └── plots.py                <- Code to create visualizations
```

---
