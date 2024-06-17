## Walmart-Sales-Project
## Overview

In this project, I will clean a dataset containing Walmart sales data and analyze various fields such as sales, unemployment rate, CPI, fuel price, etc. The goal is to gain insights and answer several business questions based on the cleaned dataset.

## Step 1: Download and Load the Data

First, download the dataset from the provided link https://www.kaggle.com/datasets/mikhail1681/walmart-sales and upload it. Once uploaded, I will load the data into a Pandas DataFrame for further analysis.

## Step 2: Data Cleaning

I will perform the following data cleaning tasks using Python (Pandas):
- Sort data by store number (ascending) and then by date (ascending)
- Ensure the date is in the format MM-DD-YYYY.
- Round Weekly Sales to the nearest 2 decimal places.
- Round Temperature to the nearest whole number.
- Round Fuel Price to the nearest 2 decimal places.
- Round CPI to the nearest 3 decimal places.
- Round Unemployment to the nearest 3 decimal places.
- Ensure there is no missing data.

### Here is the Python code to perform these tasks:

## 1. Load the dataset
```
import pandas as pd
Walmart_data = pd.read_csv('Walmart_Sales.csv')
```

## 2. Sort data by store number and date
```
Walmart_data = Walmart_data.sort_values(by=['Store', 'Date'])
Walmart_data.head()
```

## 3. Convert Date to MM-DD-YYYY format
```
Walmart_data['Date'] = pd.to_datetime(Walmart_data['Date'], format='mixed').dt.strftime('%m-%d-%Y')
```

## 4. Round Weekly Sales to the nearest 2 decimal places
```
Walmart_data['Weekly_Sales'] = Walmart_data['Weekly_Sales'].round(2)
```

## 5. Round Temperature to the nearest whole number
```
Walmart_data['Temperature'] = Walmart_data['Temperature'].round()
```

## 6. Round Fuel Price to the nearest 2 decimal places
```
Walmart_data['Fuel_Price'] = Walmart_data['Fuel_Price'].round(2)
```

## 7. Round CPI to the nearest 3 decimal places
```
Walmart_data['CPI'] = Walmart_data['CPI'].round(3)
```

## 8. Round Unemployment to the nearest 3 decimal places
```
Walmart_data['Unemployment'] = Walmart_data['Unemployment'].round(3)
```

## 9. Check for missing data
```
missing_data = Walmart_data.isnull().sum()
```

## 10. Fill missing data
```
data = Walmart_data.fillna(method='ffill')
print("Data cleaning completed.")
print(data.head())
print("Missing data count after cleaning:", missing_data)
```

## Step 3: Business Questions Analysis

I will answer the following business questions based on the cleaned data:

- Which holidays affect weekly sales the most?
- Which stores have the lowest and highest unemployment rate?
- Is there any correlation between CPI and Weekly Sales? How does the correlation differ when the Holiday Flag is 0 versus when the Holiday Flag is 1?
- Why is Fuel Price included in this dataset? What conclusions can be made about Fuel Price compared to other fields?


## Analysis Code 
```
import matplotlib.pyplot as plt
import seaborn as sns

# Analysis of holidays affecting weekly sales
holiday_sales = data[data['Holiday_Flag'] == 1]
average_holiday_sales = holiday_sales.groupby('Date')['Weekly_Sales'].mean().sort_values(ascending=False)
print("Holidays affecting weekly sales the most:")
print(average_holiday_sales)

# Stores with lowest and highest unemployment rate
lowest_unemployment = data.loc[data['Unemployment'].idxmin()]
highest_unemployment = data.loc[data['Unemployment'].idxmax()]
print("Store with the lowest unemployment rate:")
print(lowest_unemployment[['Store', 'Unemployment']])
print("Store with the highest unemployment rate:")
print(highest_unemployment[['Store', 'Unemployment']])

# Correlation between CPI and Weekly Sales
correlation = data[['CPI', 'Weekly_Sales']].corr().iloc[0, 1]
print(f"Overall correlation between CPI and Weekly Sales: {correlation}")

# Correlation with Holiday Flag
correlation_non_holiday = data[data['Holiday_Flag'] == 0][['CPI', 'Weekly_Sales']].corr().iloc[0, 1]
correlation_holiday = data[data['Holiday_Flag'] == 1][['CPI', 'Weekly_Sales']].corr().iloc[0, 1]
print(f"Correlation between CPI and Weekly Sales (Non-Holiday): {correlation_non_holiday}")
print(f"Correlation between CPI and Weekly Sales (Holiday): {correlation_holiday}")

# Fuel Price analysis
plt.figure(figsize=(10, 6))
sns.lineplot(data=data, x='Date', y='Fuel_Price', label='Fuel Price')
plt.xticks(rotation=45)
plt.title('Fuel Price Over Time')
plt.show()

print("Fuel Price compared to other fields:")
print(data[['Fuel_Price', 'Weekly_Sales', 'CPI', 'Unemployment']].corr())

# Conclusions about Fuel Price
# Generate correlation matrix to understand relationships
fuel_price_correlation = data[['Fuel_Price', 'Weekly_Sales', 'CPI', 'Unemployment']].corr()
print(fuel_price_correlation)
```
