# capstone-project-eda
Repository part of a bigger project: Adversarial AI in Wealth Management, a Capstone Project for [IE University](https://www.ie.edu/university/studies/projects/adversarial-artificial-intelligence-wealth-management/).

### Packages Involved
- `Pandas`: used for dataset manipulation. Alternatively could have used `polars`.
  - Used Pandas.pd.get_dummies for quick OneHotEncoding-like result. Ordinal Variable treated differently.
- `ydata_profiling.ProfileReport`: Used to automatically generate EDA report.
- `sklearn.preprocessing.OrdinalEncoder`: Used to handle Ordinal Variables.
- `sklearn.impute.KNNImputer` : Used to fill in missing values. Not recommended for larger datasets. But for a dataset of ~15,000 records, it works fine. 

### EDA
- Attatched to repository as index.html. Check it out [here](https://ckranon.github.io/capstone-project/).

#### Missing Value Report

| ydata_profiling alert | Alert Type |
| -------------------------- | ----- |
|Income has 2250 (15.0%) missing values 	| Missing |
|Credit Score has 2250 (15.0%) missing values 	|Missing|
|Loan Amount has 2250 (15.0%) missing values 	|Missing|
|Assets Value has 2250 (15.0%) missing values 	|Missing|
|Number of Dependents has 2250 (15.0%) missing values 	|Missing|
|Previous Defaults has 2250 (15.0%) missing values | 	Missing |
|Debt-to-Income Ratio has unique values 	| Unique |
|Years at Current Job has 727 (4.8%) | zeros | 


#### Categorical Ordinal Variables 
- Credit Risk Rating (Low-Medium-High)
- Level of Education (High School, Bachelor's, Master's, PhD)
- Payment History (Poor-Fair-Good-Excellent)

#### Categorical Nominal
- Gender
- Marital Status
- Loan Purpose
- Unemployment Status

### Coding

#### Saving Report to .html file.
```{python}
report.to_file("report.html")  # Save report as an HTML file
```

#### Removing Location Data
At present, I cannot handle location data. I attempted to concatenate City and Country Column, but was unsuccessful.
```{python}
data = data.drop(['City', 'State', 'Country'], axis=1)
```

#### Encdoing Datset
```{python}
ordinal_mapping = {
    'Education Level': ['High School', "Bachelor's", "Master's", "PhD"],
    'Risk Rating': ["Low", "Medium", "High"],
    'Payment History': ["Poor", "Fair", "Good", "Excellent"]
}
encoder = OrdinalEncoder(categories=[ordinal_mapping[col] for col in ordinal_mapping])
data[list(ordinal_mapping.keys())] = encoder.fit_transform(data[list(ordinal_mapping.keys())])

data = pd.get_dummies(data)
```

#### 5-NN Mean imputation
```{python}
imputer = KNNImputer(n_neighbors=5)
data = pd.DataFrame(imputer.fit_transform(data), columns=data.columns)
```

#### Export File
Final dataset exported in .parquet format.
