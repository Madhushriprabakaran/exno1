## NAME:MADHUSHRI
## REG NO:212224040178

# Exno:1
Data Cleaning Process
 
# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Import the required libraries such as Pandas, NumPy, and SciPy.

STEP 2: Read the given datasets using read_csv().

STEP 3: Display the dataset information, missing values, and duplicate records.

STEP 4: Handle missing values using median for numerical columns and mode for categorical columns.

STEP 5: Remove duplicate records and identify outliers using the IQR and Z-score methods.

STEP 6: Save the cleaned datasets to CSV files using to_csv().

# Coding and Output
```
import numpy as np
import pandas as pd
from scipy import stats
df=pd.read_csv("Data_set.csv")
print(df.isnull().sum())
cols=['rating','current_overall_rank','watchers']
for col in cols:
    df[col]=df[col].interpolate()
new_cols=['show_name','aired_on','original_network']
for col in new_cols:
    df[col] = df[col].fillna(df[col].mode()[0])
print(df.isnull().sum())

```

<img width="387" height="396" alt="image" src="https://github.com/user-attachments/assets/e80a2a29-ef14-444a-b33d-b6c20a34b9ef" />


```

import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("SAMPLEIDS.csv")

print(df.head())
print(df.shape)
print(df.columns)
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

df = df.drop_duplicates()


for col in df.select_dtypes(include="number").columns:
    if df[col].isnull().any():
        df[col] = df[col].fillna(df[col].median())

for col in df.select_dtypes(include="object").columns:
    if df[col].isnull().any() and not df[col].mode().empty:
        df[col] = df[col].fillna(df[col].mode()[0])

print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    outliers = df[(df[col] < lower) | (df[col] > upper)]
    print("\nIQR outliers in", col)
    print(outliers[[col]])

for col in df.select_dtypes(include="number").columns:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]
    print("\nZ-score outliers in", col)
    print(outliers[[col]])

df.to_csv("SAMPLEIDS_cleaned.csv", index=False)

print("Cleaned dataset saved successfully!")
```

<img width="900" height="756" alt="image" src="https://github.com/user-attachments/assets/c63cf78c-ee40-466b-bbff-9916b228f2e6" />

<img width="805" height="752" alt="image" src="https://github.com/user-attachments/assets/4a4e255f-3f6f-4f98-8cd6-9c558c1ea62b" />

<img width="652" height="763" alt="image" src="https://github.com/user-attachments/assets/a0cd66ac-bf28-4a8f-88e8-02b60b01c557" />

<img width="461" height="387" alt="image" src="https://github.com/user-attachments/assets/ae564a63-2ca1-4420-909b-50c9464df51e" />


```

import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("heights.csv")

print(df.head())
print(df.describe())
print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nUnusual measurements using IQR in", col)
    print(outliers[[col]])

for col in df.select_dtypes(include="number").columns:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]

    print("\nUnusual measurements using Z-score in", col)
    print(outliers[[col]])
```

<img width="467" height="533" alt="image" src="https://github.com/user-attachments/assets/56a46253-2c87-4541-8119-843bda0a46e4" />



```

import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("iris.csv")

print(df.head())
print(df.describe())

num_cols = df.select_dtypes(include="number").columns

for col in num_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nIQR outliers in", col)
    print(outliers[[col]])

for col in num_cols:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]

    print("\nZ-score outliers in", col)
    print(outliers[[col]])
```

<img width="702" height="762" alt="image" src="https://github.com/user-attachments/assets/b90d09ec-15d0-4ee0-9ac8-5fc68e597cbe" />

<img width="611" height="567" alt="image" src="https://github.com/user-attachments/assets/8e3dce3e-09e6-4f15-a340-2fd2e9c17e45" />

```

import pandas as pd

df = pd.read_csv("Loan_data.csv")

print(df.head())
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

df = df.drop_duplicates()

num_cols = ["ApplicantIncome", "CoapplicantIncome", "LoanAmount","Credit_History","Loan_Amount_Term"]
cat_cols = ["Gender", "Married", "Education", "Self_Employed", "Property_Area","Dependents"]

for col in num_cols:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].median())

for col in cat_cols:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].mode()[0])
        df[col] = df[col].str.strip()

print("\nMissing values after cleaning:")
print(df.isnull().sum())

print("\nCleaned dataset:")
print(df.head())

df.to_csv("Loan_data_cleaned.csv", index=False)

print("Cleaned dataset saved successfully!")
```

<img width="875" height="767" alt="image" src="https://github.com/user-attachments/assets/1aae689e-d4e0-4259-8bde-619d798c3548" />

<img width="927" height="781" alt="image" src="https://github.com/user-attachments/assets/14202c3e-bee5-45d5-a45f-2289b3407ddb" />

<img width="791" height="441" alt="image" src="https://github.com/user-attachments/assets/1c228ac8-ca1c-4fc3-90e9-30ca182e0da6" />

# Summary

In this experiment, Pandas and NumPy libraries were used to perform data cleaning on different datasets. The datasets were loaded using read_csv() and inspected using head(), info(), and describe(). Missing values were handled, and duplicate records were removed where necessary. Outliers were identified using the IQR and Z-score methods. Finally, the cleaned datasets were saved as CSV files using to_csv()

# Result
Thus the given data have been read and performed data cleaning and saved the cleaned data to a file.
