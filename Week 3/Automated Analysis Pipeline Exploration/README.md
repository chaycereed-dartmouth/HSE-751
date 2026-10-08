# Diabetes Prediction and Automated Analysis

Google Colab: https://colab.research.google.com/drive/1n-812ua1Dp1o7O7XkT8rUmyTY_mGMK7X?usp=sharing

## Purpose

This notebook examines how eight health measurements differ between patients with and without diabetes, and how well they predict diabetes status. Predictor variables include pregnancies, glucose, diastolic blood pressure, skin thickness, insulin, BMI, predigree, and age. The outcome is a binary diabetes status (0 = no diabetes, 1 = has diabetes).

Before the analysis, the data is loaded, cleaned, and summarized. Several measurements have zero values that indicate a missing measurement (e.g., BMI = 0), so these values are set as NaN, per the datasets documentation. The analysis then proceeds to perform descriptive statistics for each predictor overall and by outcome group, visualization of the distributions between predictors and by outcome, perform inferential analysis to identify whether each predictor differs between the two outcome groups, and uses supervised machine learning models to predict diabetes status.

## Repo Structure

```
Automated Analysis Pipeline Exploration/
├── README.md
├── Automated_Analysis_Pipeline_Notebook.ipynb
├── data/
│   └── raw/
│       └── Example Dataset_Diabetes.csv
```

## Notebook Structure

- **1. Setup**
  - 1.1. Load and Install Libraries
  - 1.2. Upload your dataset
  - 1.3. Define variable groups and data types
  - 1.4. Set Seed
- **2. Data Structure & Cleaning**
  - 2.1. View first 20 rows of data
  - 2.2. View a list of all variables
  - 2.3. View the number of rows and columns in the dataset
  - 2.4. View general information about the dataset
  - 2.5. Describe the data (and data type) for each variable
  - 2.6. Identify known impossible values for later imputation
  - 2.7. Automatically identify other missing values in numeric predictors
  - 2.8. Description of just one variable
  - 2.9. Number of responses and missing values for each variable
  - 2.10. View 20 random rows of data
  - 2.11. Change a variable's data type (e.g., from integer to category)
  - 2.12. Update all variable data types to match expected
- **3. Exploratory Data Analysis**
  - 3.1. Summarize Predictors by Outcome Group
  - 3.2. Pearson's correlation table
  - 3.3. Pearson's correlation heatmap
  - 3.4. Spearman's Rank correlation table
  - 3.5. Spearman's Rank correlation heatmap
  - 3.6. Phi-K correlation heatmap
  - 3.7. Example of a histogram
  - 3.8. Example of a distribution plot
  - 3.9. Generate histograms for all numerical variables
  - 3.10. Example of a violin plot
  - 3.11. Example of a categorical plot
  - 3.12. Define a function to create a boxplot and histogram combo
  - 3.13. Example of a combination boxplot and histogram
  - 3.14. Example of a boxplot with the primary outcome and a continuous variable
  - 3.15. Example of a scatterplot
  - 3.16. Example of a joint plot (3 variables)
  - 3.17. Example fo a joint plot of a different kind (2 variables)
  - 3.18. Anchor exmaple of a joint plot of a different kind (2 variables)
  - 3.19. Example of a line plot
  - 3.20. Example of a strip plot
  - 3.21. Example of a swarm plot
  - 3.22. Boxplot of Predictors by Outcome Group
- **4. Inferential Statistics**
  - 4.1. Import additional inferential statistics libraries
  - 4.2. Example of a one-sample t-test
  - 4.3. Independent samples t-test (specific to this dataset)
  - 4.4. One-way ANOVA (specific to this dataset)
  - 4.5. Chi-square test of independence (specific to this dataset)
  - 4.6. Mann Whitney U
- **5. Supervised Machine Learning (Binary Classification)**
  - 5.1. Encode outcome labels as 0 and 1 (if needed)
  - 5.2. Logistic Regression
    - 5.2.1. Configure and split data for the automated ML pipeline
    - 5.2.2. Define a function to display a confusion matrix
    - 5.2.3. Build logistic regression with automated preprocessing
    - 5.2.4. Automatically preprocess, train, and evaluate ML models
    - 5.2.5. ROC curve and AUC for logistic regression
    - 5.2.6. Select a cutoff using training-set cross-validation
    - 5.2.7. Make confusion matrix using the selected cutoff
    - 5.2.8. Compare training and test F1 at the selected cutoff
  - 5.3. Decision Tree
    - 5.3.1. Import libraries for decision tree training and evaluation
    - 5.3.2. Build and tune decision tree usign cross-validation F1
    - 5.3.3. Function to calculate accuracy, recall, precision, and F1
    - 5.3.4. Display confusion matrix for tuned decision tree
    - 5.3.5. Show the tuned decision tree
    - 5.3.6. List feature importance from the tuned decision tree
    - 5.3.7. Visualize feature importance from the tuned decision tree
    - 5.3.8. Histogram-Based Gradient Boosting

## New Features & Additional Modifications
---

1. First, I moved all imports and libraries to the beginning of the notebook and removed any duplicate imports. Previously, the same packages and functions were imported multiple times throughout the notebook. Keeping them in one place makes it clear what the notebook depends on, avoids reapeated/redundant code (DRY), and makes it easier to identify which packages need to be installed or their versions specified.

2. Second, I refactored how the dataset is loaded. The notebook now first checks if a data file already exists in the Google Colab environment, and if so, uses it directly. If not, it prompts the user to upload one and raises an error if more or less than one file is uploaded. Once the file is located, the notebook identifies the extension and reads it with the matching pandas reader function. To improve this, I created a dictionary with each reader function (e.g., pd.read_csv, pd.read_excel) as the key and its supported file extensions as the value. Since the key is a reader function, it can be applied directly to the file without the need for a separate if/elif statement for each different reader type. With thats said, we still handle delimited text files (e.g., .csv, .tsv. txt) separately using an if/elif statement since they require additional arguments (i.e., sep, engine). If the extension does not match a predefined file type, then we will raise an error and flag it to the user. This update allows the notebook to be rerun without reuploading the data each time, and for the allowable extension / file type to be updated easily from a single dictionary.

3. Third, I defined the outcome and predictor variables uprfont in a single variables dataframe, along with each's variables role (e.g., predictor, outcome), expected data type (e.g., int64, float64), display label (e.g., Glucose (mg/dL)), and a binary flag for whether a zero value indicates missingness (True/False). This allows for the variables to be easily referenced and grouped by role, data type, or zero indicating missingness. Whenever possible, these groups or variables (e.g., outcome, predictors, primary_predictor, variables_to_replace_zeros) are used instead of hardcoded values throughout the script. This approach makes it possible for variabels to be added, removeed, or updated in one place, rather than updating every individual cell that uses them. Finally, it allows for display labels for visualizations to be easily referenced using labels[variable], making these labels dynamic based on the label being used and provides an overall more readable and interpretable visualization.

4. Fourth, I updated all variable data types to match their defined expected type. Using the data types defined in the variables dataframe, the notebook first checks checks each variable's current type against the expected, and only converts / prints a message for variables that do not match. This ensures that each variable is the correct data type before any analysis, and makes any required conversions visible to the user in the output.

5. Fifth, I added section labels and headers throughout the notebook, so that each step (e.g., setup, data structure & cleaning, exploratory data analysis) is clearly separated and easy to navigate.

6. Sixth, I cleaned up the histogram and boxplot figures by adding a title and x and y axis labels to each plot. I also updated the plotting code to use the predictor and outcome variables defined at the beginning of the notebook, instead of hardcoding each variable name individually (where possible, avoiding any code below the do not change lines). This keeps the plots consistent with the eachother, and the rest of the notebook, and any change to the variable definitions or predictors of interest is automatically reflected in the figures.

7. Seventh, I added a summary of predictors by outcome group (i.e., no diabetes, diabetes), which reports the means for each group, along with the difference in means between groups. This is now the first step in the exploratory data analysis section and allows for a quick comparison between groups before moving onto the visualizations and inferential statistics tests.

8. Eighth, I added boxplots for all predictors by outcome group (i.e., no diabetes, diabetes), displayed as a single layout. These compliment the summary of predictors by outcome group and predictor histograms, showing the difference in distribution of each predictor (e.g., BMI, Glucose, Pregnancies) between the two groups.

9. Ninth, I added a Mann-Whitney U test comparing each predictor between outcome groups (i.e., no diabetes, diabetes). Since pregnancies, insulin, and pedigree all appeared to be right skewed, the prior tests that comapred means (e.g., Welch's t-test) may have been influenced by extreme values. The Mann-Whitney U test is a non-parametric altnerative that compares ranks rather than means. It does not rely on the normal distribution assumption and is less sensitive to outliers, making it a good fit for these predictors and a useful robustness check. The test was written as a modular function that could be applied across all predictors (using the dynamic predictors and outcome variables, not hardcoded values), with included Bonferroni adjustment for multiple comparisons across all 8 predictors.

10. Finally, I added a histogram-based gradient boosting model as an additional supervised machine learning model, which builds the tree sequentially and each new tree corrects for errors of the previous ones. Unlike the other two models, it can handle missing values directly without imputation. Also, to address the imbalanced outcome (35/65), class weighting was used to give more weight to the diabetes group using the class_weight = "balanced argument". The model was trained and tested on the samle split as the logistic regression and decision tree models, adn was evaluated using AUC, recall, and accuracy. 

## Data Source & Provenance

The data source is the Pima Indians Diabetes Dataset, which was originally collected by the National Institute of Diabetes and Digestive and Kidney Diseases. The dataset contains health measurements for 768 female patients and a binary target variable indicating whether each patient has diabetes.

Several measurements record 0 where no reading was taken (glucose, diastolic blood pressure, skin thickness, insulin, and BMI). This is a known characteristic of the dataset (https://search.r-project.org/CRAN/refmans/mlbench/html/PimaIndiansDiabetes.html), and those values are treated as missing in Section 6.

| Column | Description | Units | Type |
|---|---|---|---|
| `Pregnancies` | Number of times pregnant | count | Integer |
| `Glucose` | Plasma glucose concentration | mg/dL | Integer |
| `D_BP` | Diastolic blood pressure | mmHg | Integer |
| `Skin_Thickness` | Tricep skin fold thickness | mm | Integer |
| `Insulin` | 2-hour serum insulin | mmU/mL | Integer |
| `BMI` | Body Mass Index | kg/m^2 | Float |
| `Pedigree` | Diabetes pedigree function | unitless | Float |
| `Age` | Age | years | Integer |
| `Outcome` | Diabetes Status | 0 = No diabetes, 1 = Has diabetes | Integer |

## Required Software & Libraries

Google Colab. Package versions are defined in and installed by the notebook.

| | Version |
|---|---|
| `Python` | 3.13.15 (Colab runtime) |
| `pandas` | 2.2.3 |
| `numpy` | 2.1.3 |
| `google` | 3.0.0 |
| `matplotlib` | 3.10.0 |
| `scipy` | 1.16.3 |
| `seaborn` | 0.13.2 |
| `sklearn` | 1.6.1 |
| `statsmodels` | 0.15.0 |
| `phik` | 0.12.5 |

Standard libraries (`os`, `sys`, `io`, `platform`, `subprocess`, `textwrap`, `pathlib`, `math`) are bundled with Python and are not installed separately.

## Instructions for Executing Notebook

1. Open `Automated_Analysis_Pipeline_Notebook.ipynb` in Google Colab using the link at the top of this README.
2. Upload the dataset to `/content`, or wait for the upload prompt in Section 1.2.
2. Select **Run all**.

The notebook uses a single random seed (`SEED = 123`) and produces the same output on every run.

## Assumptions and Limitations

- Several measurements recorded 0 where no reading was taken. The reason these measurements were not collected or recorded is unknown and may be systematic, which could bias the results.
- Insulin (374 zeros / missing values) and skin thickness (227 zeros / missing values) were the most affected by missingness, reducing the sample size and limiting the robustness of the findings for these variables.
- The t-tests assume an approximate normality for each group, which may be violated for pregnancies, insulin, and pedigree which appear to be right skewed.
- The Student's t-test assumes equal variance between groups, which Welch's t-test does not.
- The cohort is female patients aged 21 and older, limiting generalizability to the broader population.

## Acknowledgements

Thanks to the National Institute of Diabetes and Digestive and Kidney Diseases of collecting the original data, Kaggle and the UC Irvine Machine Learning Repository for making it available, and the course for providing the copy used here.

## AI Use Statement

Generative AI (Claude) was used as a coding, learning, and editing assistant for the following:

- Python syntax and package questions, such as building a dictionary-based file reader and checking/converting data types.
- Brainstorming, implementing, and understanding inferential statistic methods (e.g., Mann Whitney U) and supervised machine learning (e.g., histogram-based gradient boosting) models, including help understanding strengths, weaknesses, assumptions, and interpreting results.
- Brainstorming and editing notebook and README structure / contents, and learning reproducibility and automation best practices

I verified and reviewed all work. I take full responsibility for the accuracy and legitimacy of all work submitted.

## Computational Environment Information

Google Colab:

```
Python implementation: CPython
Python version       : 3.13.16
IPython version      : 7.34.0

Compiler    : GCC 13.3.0
OS          : Linux
Release     : 6.6.122+
Machine     : x86_64
Processor   : x86_64
CPU cores   : 2
Architecture: 64bit

google     : 3.0.0
matplotlib : 3.10.0
numpy      : 2.1.3
pandas     : 2.2.3
phik       : 0.12.5
scipy      : 1.16.3
seaborn    : 0.13.2
sklearn    : 1.6.1
statsmodels: 0.15.0
```


