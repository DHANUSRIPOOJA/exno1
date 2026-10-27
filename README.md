# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output

# To upload the datasets
```
from google.colab import files

uploaded = files.upload()
```
# OUTPUT
<img width="1721" height="322" alt="image" src="https://github.com/user-attachments/assets/23b5329d-f57e-4713-8c67-bc89c1ec1a43" />

# Load and inspect the datasets
```
import pandas as pd

df1 = pd.read_csv("heights.csv")
df2 = pd.read_csv("Data_set.csv")

print("HEIGHTS DATASET")
display(df1.head())
print(df1.info())
print("Missing values:\n", df1.isnull().sum())

print("\nDATA_SET DATASET")
display(df2.head())
print(df2.info())
print("Missing values:\n", df2.isnull().sum())
```
# OUTPUT
<img width="1712" height="581" alt="image" src="https://github.com/user-attachments/assets/3ade6bb8-6c62-4bf5-9e7c-2fb1ad8e7a3f" />
<img width="1717" height="842" alt="image" src="https://github.com/user-attachments/assets/5ba736b9-afc9-45f7-ab89-94d91d503ab8" />

# Clean both datasets
```
def clean_data(df):
    df = df.copy()

    # Remove duplicate rows
    df = df.drop_duplicates()

    # Fill missing numerical values with median
    for col in df.select_dtypes(include="number").columns:
        df[col] = df[col].fillna(df[col].median())

    # Fill missing text values with mode
    for col in df.select_dtypes(include="object").columns:
        if df[col].isnull().any() and not df[col].mode().empty:
            df[col] = df[col].fillna(df[col].mode()[0])

    # Detect and cap numerical outliers using IQR
    for col in df.select_dtypes(include="number").columns:
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1

        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR

        df[col] = df[col].clip(lower, upper)

    return df

cleaned_heights = clean_data(df1)
cleaned_data = clean_data(df2)

print("Heights dataset cleaned successfully!")
display(cleaned_heights.head())

print("Data_set cleaned successfully!")
display(cleaned_data.head())
```

# OUTPUT
<img width="1543" height="530" alt="image" src="https://github.com/user-attachments/assets/44919b8e-df69-4a1d-9422-2f7b0d7613de" />



# Verify and download cleaned datasets
```
print("Heights - Missing values:")
print(cleaned_heights.isnull().sum())

print("\nData_set - Missing values:")
print(cleaned_data.isnull().sum())

cleaned_heights.to_csv("cleaned_heights.csv", index=False)
cleaned_data.to_csv("cleaned_Data_set.csv", index=False)

from google.colab import files

files.download("cleaned_heights.csv")
files.download("cleaned_Data_set.csv")
```
# OUTPUT
<img width="367" height="357" alt="image" src="https://github.com/user-attachments/assets/c3536038-4d82-4cc5-a4e5-fb1ca3c67666" />
<img width="275" height="410" alt="image" src="https://github.com/user-attachments/assets/343f1b5e-88eb-4bf8-9478-938cb032fbce" />




# Result

The datasets were successfully cleaned using Python by handling missing values, removing duplicates, and detecting and capping outliers.
