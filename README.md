# DATA-PIPELINE-DEVELOPMENT
import pandas as pd
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.impute import SimpleImputer

# Step 1: Extract (Load raw data)
df = pd.read_csv("raw_data.csv")

# Step 2: Preprocess
# Separate numerical and categorical columns
num_cols = df.select_dtypes(include=["float64", "int64"]).columns
cat_cols = df.select_dtypes(include=["object"]).columns

# Handle missing values
num_imputer = SimpleImputer(strategy='mean')
df[num_cols] = num_imputer.fit_transform(df[num_cols])

cat_imputer = SimpleImputer(strategy='most_frequent')
df[cat_cols] = cat_imputer.fit_transform(df[cat_cols])

# Encode categorical columns
for col in cat_cols:
    le = LabelEncoder()
    df[col] = le.fit_transform(df[col])

# Step 3: Transform (Scale features)
scaler = StandardScaler()
df[num_cols] = scaler.fit_transform(df[num_cols])

# Step 4: Load (Save preprocessed data)
df.to_csv("cleaned_data.csv", index=False)
print("✅ ETL process complete. Output saved to 'cleaned_data.csv'")
pandas

scikit-learn
pip install pandas scikit-learn
raw_data.csv
Out put:
Name,Age,Gender,Salary
Alice,25,Female,50000
Bob,,Male,60000
Charlie,35,,55000
David,40,Male,
Eve,29,Female,62000
cleaned_data.csv
Name,Age,Gender,Salary
0,-1.4986,0,-1.1832
1,0.0000,1,0.5071
2,1.2874,0,-0.1695
3,2.1499,1,0.0000
4,-1.9387,0,0.8457
