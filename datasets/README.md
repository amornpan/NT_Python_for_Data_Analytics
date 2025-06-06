# Datasets

## Primary Dataset
### LoanStats_web_143103.csv
**Description:** Lending Club loan data with 143 features
**Size:** Large dataset with comprehensive loan information
**Use Cases:** 
- Data profiling and preparation exercises
- Classification (loan default prediction)
- Regression (interest rate prediction)

**Key Fields:**
- `loan_amnt`: Loan amount
- `funded_amnt`: Funded amount
- `int_rate`: Interest rate
- `grade`: Loan grade
- `loan_status`: Current status of loan (target variable for classification)
- `annual_inc`: Annual income
- `dti`: Debt-to-income ratio
- `emp_length`: Employment length
- `home_ownership`: Home ownership status
- `purpose`: Purpose of loan

## Additional Sample Datasets
### sample_sales_data.csv
**Description:** Sample retail sales data
**Use:** Basic pandas operations and visualization

### customer_demographics.csv
**Description:** Customer demographic information
**Use:** Data profiling exercises

### stock_prices.csv
**Description:** Historical stock price data
**Use:** Time series analysis basics

## Data Loading Instructions

### For Google Colab:
```python
# Upload to Google Drive first, then:
from google.colab import drive
drive.mount('/content/drive')

import pandas as pd
df = pd.read_csv('/content/drive/MyDrive/path_to_file/LoanStats_web_143103.csv')
```

### Alternative - Direct Upload:
```python
from google.colab import files
uploaded = files.upload()
df = pd.read_csv('LoanStats_web_143103.csv')
```

## Data Dictionary
ดู `data_dictionary.md` สำหรับรายละเอียดครomplete ของแต่ละ field
