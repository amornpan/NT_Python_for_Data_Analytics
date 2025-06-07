# 📊 Lab Template สำหรับ Module 2: Data Profiling & Preparation

## 🚀 Standard Setup

```python
# Import libraries
from google.colab import drive
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')

# Mount Google Drive
drive.mount('/content/drive')

# Load Lending Club data
DATA_PATH = '/content/drive/MyDrive/NT_Python_for_Data_Analytics/LoanStats_web_14422.csv'
df = pd.read_csv(DATA_PATH, low_memory=False)

print(f"📊 Data loaded successfully!")
print(f"Shape: {df.shape[0]:,} rows × {df.shape[1]:,} columns")
print(f"Memory usage: {df.memory_usage(deep=True).sum()/1024**2:.1f} MB")

# Setup visualization style
plt.style.use('default')
sns.set_style("whitegrid")
sns.set_palette("Set2")
plt.rcParams['figure.figsize'] = (12, 8)
plt.rcParams['font.size'] = 11
```

## 🔍 Essential Data Profiling Functions

```python
def comprehensive_data_profile(df):
    """
    สร้าง comprehensive data profile report
    """
    print("🔍 COMPREHENSIVE DATA PROFILE REPORT")
    print("="*60)
    
    # 1. Basic Information
    print(f"📊 Dataset Overview:")
    print(f"   Shape: {df.shape[0]:,} rows × {df.shape[1]:,} columns")
    print(f"   Memory: {df.memory_usage(deep=True).sum()/1024**2:.1f} MB")
    print(f"   Data types: {df.dtypes.value_counts().to_dict()}")
    
    # 2. Missing Data Analysis
    missing_data = df.isnull().sum()
    missing_pct = 100 * missing_data / len(df)
    missing_df = pd.DataFrame({
        'Missing_Count': missing_data,
        'Missing_Percentage': missing_pct
    }).sort_values('Missing_Percentage', ascending=False)
    
    print(f"\n❓ Missing Data Summary:")
    print(f"   Total missing values: {missing_data.sum():,}")
    print(f"   Columns with missing data: {(missing_data > 0).sum()}")
    print(f"   Most missing columns:")
    print(missing_df.head(10))
    
    # 3. Data Types Summary
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    categorical_cols = df.select_dtypes(include=['object']).columns
    
    print(f"\n📊 Column Types:")
    print(f"   Numeric columns: {len(numeric_cols)}")
    print(f"   Categorical columns: {len(categorical_cols)}")
    
    # 4. Categorical Data Summary
    print(f"\n📝 Categorical Columns Analysis:")
    for col in categorical_cols[:5]:
        print(f"\n   {col}:")
        print(f"     Unique values: {df[col].nunique()}")
        print(f"     Most frequent: {df[col].mode().iloc[0] if not df[col].mode().empty else 'N/A'}")
        print(f"     Missing: {df[col].isnull().sum()} ({df[col].isnull().sum()/len(df)*100:.1f}%)")
    
    # 5. Numeric Data Summary
    if len(numeric_cols) > 0:
        print(f"\n🔢 Numeric Columns Summary:")
        print(df[numeric_cols].describe())
    
    # 6. Data Quality Issues
    print(f"\n⚠️ Data Quality Issues:")
    print(f"   Duplicate rows: {df.duplicated().sum():,}")
    
    # 7. Memory usage by columns
    memory_usage = df.memory_usage(deep=True).sort_values(ascending=False)
    print(f"\n💾 Top 10 Memory Consuming Columns:")
    for col, usage in memory_usage.head(10).items():
        print(f"   {col}: {usage/1024**2:.2f} MB")
    
    return missing_df, numeric_cols, categorical_cols

# Usage
missing_summary, num_cols, cat_cols = comprehensive_data_profile(df)
```

## 📈 Visualization Functions

```python
def create_missing_data_visualization(df):
    """
    สร้างการแสดงผล missing data patterns
    """
    plt.figure(figsize=(15, 10))
    
    # Missing data heatmap
    plt.subplot(2, 2, 1)
    missing_data = df.isnull()
    sns.heatmap(missing_data, cbar=True, yticklabels=False, cmap='viridis')
    plt.title('Missing Data Pattern')
    plt.xlabel('Columns')
    
    # Missing data bar chart
    plt.subplot(2, 2, 2)
    missing_counts = df.isnull().sum().sort_values(ascending=False)
    missing_counts = missing_counts[missing_counts > 0][:20]  # Top 20
    missing_counts.plot(kind='bar')
    plt.title('Top 20 Columns with Missing Data')
    plt.xlabel('Columns')
    plt.ylabel('Missing Count')
    plt.xticks(rotation=45)
    
    # Missing data percentage
    plt.subplot(2, 2, 3)
    missing_pct = (df.isnull().sum() / len(df) * 100).sort_values(ascending=False)
    missing_pct = missing_pct[missing_pct > 0][:20]
    missing_pct.plot(kind='bar', color='orange')
    plt.title('Missing Data Percentage')
    plt.xlabel('Columns')
    plt.ylabel('Missing %')
    plt.xticks(rotation=45)
    
    # Data completeness
    plt.subplot(2, 2, 4)
    completeness = (1 - df.isnull().sum() / len(df)) * 100
    completeness.hist(bins=20, alpha=0.7, color='green')
    plt.title('Data Completeness Distribution')
    plt.xlabel('Completeness %')
    plt.ylabel('Number of Columns')
    
    plt.tight_layout()
    plt.show()

def create_numeric_distribution_plots(df, columns=None, max_cols=6):
    """
    สร้างกราฟแสดงการกระจายของข้อมูลตัวเลข
    """
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if columns:
        numeric_cols = [col for col in columns if col in numeric_cols]
    
    numeric_cols = numeric_cols[:max_cols]  # จำกัดจำนวน
    
    if len(numeric_cols) == 0:
        print("No numeric columns found!")
        return
    
    fig, axes = plt.subplots(2, 3, figsize=(18, 12))
    axes = axes.flatten()
    
    for i, col in enumerate(numeric_cols):
        if i >= len(axes):
            break
            
        # Histogram
        df[col].hist(bins=50, alpha=0.7, ax=axes[i])
        axes[i].set_title(f'Distribution of {col}')
        axes[i].set_xlabel(col)
        axes[i].set_ylabel('Frequency')
        
        # Add statistics
        mean_val = df[col].mean()
        median_val = df[col].median()
        axes[i].axvline(mean_val, color='red', linestyle='--', label=f'Mean: {mean_val:.2f}')
        axes[i].axvline(median_val, color='blue', linestyle='--', label=f'Median: {median_val:.2f}')
        axes[i].legend()
    
    # Hide unused subplots
    for i in range(len(numeric_cols), len(axes)):
        axes[i].set_visible(False)
    
    plt.tight_layout()
    plt.show()

def create_correlation_analysis(df, method='pearson'):
    """
    สร้างการวิเคราะห์ความสัมพันธ์
    """
    numeric_df = df.select_dtypes(include=[np.number])
    
    if numeric_df.shape[1] < 2:
        print("Need at least 2 numeric columns for correlation analysis!")
        return None
    
    plt.figure(figsize=(12, 10))
    
    # Correlation matrix
    corr_matrix = numeric_df.corr(method=method)
    
    # Create heatmap
    mask = np.triu(np.ones_like(corr_matrix, dtype=bool))
    sns.heatmap(corr_matrix, mask=mask, annot=True, cmap='coolwarm', 
                center=0, square=True, linewidths=0.5)
    plt.title(f'Correlation Matrix ({method.title()})')
    plt.tight_layout()
    plt.show()
    
    # Find strong correlations
    strong_corrs = []\n    for i in range(len(corr_matrix.columns)):\n        for j in range(i+1, len(corr_matrix.columns)):\n            corr_val = corr_matrix.iloc[i, j]\n            if abs(corr_val) > 0.5:  # Strong correlation threshold\n                strong_corrs.append({\n                    'Variable 1': corr_matrix.columns[i],\n                    'Variable 2': corr_matrix.columns[j],\n                    'Correlation': corr_val\n                })\n    \n    if strong_corrs:\n        strong_corr_df = pd.DataFrame(strong_corrs)\n        strong_corr_df = strong_corr_df.sort_values('Correlation', key=abs, ascending=False)\n        print(f\"\\n🔗 Strong Correlations (|r| > 0.5):\")\n        print(strong_corr_df)\n    else:\n        print(\"\\n🔗 No strong correlations found (|r| > 0.5)\")\n    \n    return corr_matrix

# Usage examples
create_missing_data_visualization(df)
create_numeric_distribution_plots(df)
corr_matrix = create_correlation_analysis(df)
```

## 🧹 Data Cleaning Functions

```python
def detect_outliers_iqr(df, column):
    """
    ตรวจจับ outliers ด้วยวิธี IQR
    """
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    
    outliers = df[(df[column] < lower_bound) | (df[column] > upper_bound)]
    
    print(f\"📊 Outlier Analysis for {column}:\")\n    print(f\"   Q1: {Q1:.2f}\")\n    print(f\"   Q3: {Q3:.2f}\")\n    print(f\"   IQR: {IQR:.2f}\")\n    print(f\"   Lower bound: {lower_bound:.2f}\")\n    print(f\"   Upper bound: {upper_bound:.2f}\")\n    print(f\"   Number of outliers: {len(outliers)} ({len(outliers)/len(df)*100:.1f}%)\")\n    \n    return outliers, lower_bound, upper_bound

def clean_data_pipeline(df, target_columns=None):
    """
    พื้นฐานการทำความสะอาดข้อมูล
    """
    df_clean = df.copy()\n    \n    print(\"🧹 DATA CLEANING PIPELINE\")\n    print(\"=\"*40)\n    \n    # 1. Remove duplicates\n    initial_rows = len(df_clean)\n    df_clean = df_clean.drop_duplicates()\n    removed_dups = initial_rows - len(df_clean)\n    print(f\"1. Removed {removed_dups} duplicate rows\")\n    \n    # 2. Handle missing values for specific columns\n    if target_columns:\n        for col in target_columns:\n            if col in df_clean.columns:\n                missing_before = df_clean[col].isnull().sum()\n                \n                if df_clean[col].dtype in ['object']:\n                    # Fill categorical with mode\n                    mode_val = df_clean[col].mode().iloc[0] if not df_clean[col].mode().empty else 'Unknown'\n                    df_clean[col].fillna(mode_val, inplace=True)\n                else:\n                    # Fill numeric with median\n                    median_val = df_clean[col].median()\n                    df_clean[col].fillna(median_val, inplace=True)\n                \n                missing_after = df_clean[col].isnull().sum()\n                print(f\"2. {col}: Filled {missing_before - missing_after} missing values\")\n    \n    # 3. Basic data type conversions\n    print(f\"3. Final dataset shape: {df_clean.shape}\")\n    print(f\"   Total missing values: {df_clean.isnull().sum().sum()}\")\n    \n    return df_clean

# Example usage
if 'loan_amnt' in df.columns:\n    outliers, lower, upper = detect_outliers_iqr(df, 'loan_amnt')\n\n# Clean key columns\nkey_columns = ['loan_amnt', 'int_rate', 'annual_inc', 'dti']\navailable_columns = [col for col in key_columns if col in df.columns]\ndf_cleaned = clean_data_pipeline(df, available_columns)
```

## 🎯 Ready-to-Use Analysis Templates

```python
# Template 1: Quick Data Overview
def quick_overview():\n    print(\"📊 QUICK DATA OVERVIEW\")\n    print(\"=\"*30)\n    print(f\"Shape: {df.shape}\")\n    print(f\"Missing values: {df.isnull().sum().sum()}\")\n    print(f\"Duplicates: {df.duplicated().sum()}\")\n    print(f\"\\nFirst 5 rows:\")\n    return df.head()\n\n# Template 2: Column Analysis\ndef analyze_column(column_name):\n    if column_name not in df.columns:\n        print(f\"Column '{column_name}' not found!\")\n        return\n    \n    print(f\"📊 ANALYSIS: {column_name}\")\n    print(\"=\"*40)\n    \n    if df[column_name].dtype == 'object':\n        # Categorical analysis\n        print(f\"Type: Categorical\")\n        print(f\"Unique values: {df[column_name].nunique()}\")\n        print(f\"Missing values: {df[column_name].isnull().sum()}\")\n        print(f\"\\nTop 10 values:\")\n        print(df[column_name].value_counts().head(10))\n    else:\n        # Numeric analysis\n        print(f\"Type: Numeric\")\n        print(f\"Missing values: {df[column_name].isnull().sum()}\")\n        print(f\"\\nStatistics:\")\n        print(df[column_name].describe())\n        \n        # Create visualization\n        plt.figure(figsize=(12, 4))\n        \n        plt.subplot(1, 2, 1)\n        df[column_name].hist(bins=50, alpha=0.7)\n        plt.title(f'Distribution of {column_name}')\n        \n        plt.subplot(1, 2, 2)\n        df[column_name].plot(kind='box')\n        plt.title(f'Box Plot of {column_name}')\n        \n        plt.tight_layout()\n        plt.show()

# Template 3: Compare two variables\ndef compare_variables(var1, var2):\n    if var1 not in df.columns or var2 not in df.columns:\n        print(\"One or both variables not found!\")\n        return\n    \n    print(f\"📊 COMPARING: {var1} vs {var2}\")\n    print(\"=\"*50)\n    \n    # Both numeric\n    if df[var1].dtype != 'object' and df[var2].dtype != 'object':\n        correlation = df[var1].corr(df[var2])\n        print(f\"Correlation: {correlation:.3f}\")\n        \n        plt.figure(figsize=(10, 6))\n        plt.scatter(df[var1], df[var2], alpha=0.5)\n        plt.xlabel(var1)\n        plt.ylabel(var2)\n        plt.title(f'{var1} vs {var2} (r = {correlation:.3f})')\n        plt.show()\n    \n    # One categorical, one numeric\n    elif df[var1].dtype == 'object' and df[var2].dtype != 'object':\n        plt.figure(figsize=(12, 6))\n        df.boxplot(column=var2, by=var1)\n        plt.title(f'{var2} by {var1}')\n        plt.xticks(rotation=45)\n        plt.show()\n    \n    elif df[var1].dtype != 'object' and df[var2].dtype == 'object':\n        plt.figure(figsize=(12, 6))\n        df.boxplot(column=var1, by=var2)\n        plt.title(f'{var1} by {var2}')\n        plt.xticks(rotation=45)\n        plt.show()

# Usage examples:
# quick_overview()
# analyze_column('loan_amnt')\n# compare_variables('loan_amnt', 'int_rate')
```

## 💡 Tips & Best Practices

```python
# Performance tips\nprint(\"💡 PERFORMANCE TIPS:\")\nprint(\"1. Use df.sample(n=1000) for initial exploration\")\nprint(\"2. Use df.info(memory_usage='deep') to check memory\")\nprint(\"3. Use pd.read_csv(file, nrows=1000) to test with small data\")\nprint(\"4. Convert object columns to category if appropriate\")\nprint(\"5. Use df.dtypes to check data types before analysis\")\n\n# Sample for testing\ndf_sample = df.sample(n=1000)\nprint(f\"\\n📝 Created sample dataset: {df_sample.shape}\")\nprint(\"Use df_sample for quick testing!\")\n```

*ใช้ templates เหล่านี้ในการทำ Labs ต่างๆ ในโมดูล Data Profiling & Preparation*