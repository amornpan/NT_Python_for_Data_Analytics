# 🏋️ แบบฝึกหัดและโจทย์ปฏิบัติ

## 📋 สารบัญ
- [🎯 แบบฝึกหัดวันที่ 1: Data Exploration](#-แบบฝึกหัดวันที่-1-data-exploration)
- [📊 แบบฝึกหัดวันที่ 2: Data Preparation](#-แบบฝึกหัดวันที่-2-data-preparation)
- [🤖 แบบฝึกหัดวันที่ 3: Machine Learning](#-แบบฝึกหัดวันที่-3-machine-learning)
- [🎪 Challenge Problems](#-challenge-problems)
- [🏆 Mini Projects](#-mini-projects)

---

## 🎯 แบบฝึกหัดวันที่ 1: Data Exploration

### 📊 **Exercise 1.1: Basic Data Exploration (15 นาที)**

**เป้าหมาย**: ทำความเข้าใจโครงสร้างข้อมูล Lending Club

```python
# โหลดข้อมูลและตอบคำถามต่อไปนี้
import pandas as pd
import numpy as np

df = pd.read_csv('LoanStats_web_14422.csv')

# คำถาม:
# 1. ข้อมูลมีกี่แถว กี่คอลัมน์?
# 2. คอลัมน์ไหนมี missing values มากที่สุด 5 อันดับแรก?
# 3. ประเภทข้อมูล (data types) มีอะไรบ้าง?
# 4. ข้อมูลใช้หน่วยความจำเท่าไหร่?

# เขียนโค้ดของคุณที่นี่:


```

**เฉลย**:
```python
# 1. Shape ของข้อมูล
print(f"Dataset shape: {df.shape}")
print(f"Rows: {df.shape[0]:,}, Columns: {df.shape[1]}")

# 2. Missing values top 5
missing_values = df.isnull().sum().sort_values(ascending=False)
print("\nTop 5 columns with missing values:")
print(missing_values.head())

# 3. Data types
print("\nData types distribution:")
print(df.dtypes.value_counts())

# 4. Memory usage
memory_mb = df.memory_usage(deep=True).sum() / 1024**2
print(f"\nMemory usage: {memory_mb:.2f} MB")
```

### 📈 **Exercise 1.2: Business Growth Analysis (20 นาที)**

**เป้าหมาย**: วิเคราะห์การเติบโตของธุรกิจตามเวลา

```python
# ให้วิเคราะห์การเติบโตของธุรกิจ Lending Club
# 1. แปลง issue_d เป็น datetime
# 2. สร้างคอลัมน์ปี (issue_year) และเดือน (issue_month)
# 3. คำนวณสถิติรายปี:
#    - จำนวนสินเชื่อ
#    - ยอดรวม (ล้านดอลลาร์)
#    - ขนาดสินเชื่อเฉลี่ย
#    - อัตราดอกเบี้ยเฉลี่ย
# 4. สร้างกราฟแสดงแนวโน้มการเติบโต

# เขียนโค้ดของคุณที่นี่:


```

**เฉลย**:
```python
import matplotlib.pyplot as plt

# 1. แปลงวันที่
df['issue_d'] = pd.to_datetime(df['issue_d'])
df['issue_year'] = df['issue_d'].dt.year
df['issue_month'] = df['issue_d'].dt.month

# 2. สถิติรายปี
yearly_stats = df.groupby('issue_year').agg({
    'loan_amnt': ['count', 'sum', 'mean'],
    'int_rate': 'mean'
}).round(2)

# Flatten column names
yearly_stats.columns = ['loan_count', 'total_amount', 'avg_loan_size', 'avg_int_rate']
yearly_stats['total_amount_millions'] = yearly_stats['total_amount'] / 1000000

print("Yearly Growth Statistics:")
print(yearly_stats)

# 3. สร้างกราฟ
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(15, 10))

# จำนวนสินเชื่อ
yearly_stats['loan_count'].plot(ax=ax1, marker='o')
ax1.set_title('จำนวนสินเชื่อรายปี')
ax1.set_ylabel('จำนวน')

# ยอดรวม
yearly_stats['total_amount_millions'].plot(ax=ax2, marker='o', color='green')
ax2.set_title('ยอดสินเชื่อรวมรายปี (ล้านดอลลาร์)')
ax2.set_ylabel('ล้านดอลลาร์')

# ขนาดเฉลี่ย
yearly_stats['avg_loan_size'].plot(ax=ax3, marker='o', color='orange')
ax3.set_title('ขนาดสินเชื่อเฉลี่ยรายปี')
ax3.set_ylabel('ดอลลาร์')

# อัตราดอกเบี้ยเฉลี่ย
yearly_stats['avg_int_rate'].plot(ax=ax4, marker='o', color='red')
ax4.set_title('อัตราดอกเบี้ยเฉลี่ยรายปี')
ax4.set_ylabel('เปอร์เซ็นต์')

plt.tight_layout()
plt.show()
```

### 🎯 **Exercise 1.3: Risk Grade Analysis (25 นาที)**

**เป้าหมาย**: วิเคราะห์การกระจายความเสี่ยงตาม Grade

```python
# วิเคราะห์ข้อมูลตาม Grade:
# 1. สร้างตาราง cross-tabulation ระหว่าง grade และ loan_status
# 2. คำนวณ default rate สำหรับแต่ละ grade
# 3. สร้าง visualization แสดงการกระจายและความเสี่ยง
# 4. หาความสัมพันธ์ระหว่าง grade กับ:
#    - อัตราดอกเบี้ย
#    - ขนาดสินเชื่อ
#    - รายได้ของผู้กู้

# เขียนโค้ดของคุณที่นี่:


```

**เฉลย**:
```python
import seaborn as sns

# 1. Cross-tabulation
grade_status = pd.crosstab(df['grade'], df['loan_status'], margins=True)
print("Grade vs Loan Status Cross-tabulation:")
print(grade_status)

# 2. Default rate calculation
df['is_default'] = df['loan_status'].isin(['Charged Off', 'Default']).astype(int)
default_rates = df.groupby('grade')['is_default'].mean().sort_index()

print("\nDefault Rate by Grade:")
for grade, rate in default_rates.items():
    print(f"Grade {grade}: {rate:.2%}")

# 3. Grade analysis comprehensive
grade_analysis = df.groupby('grade').agg({
    'loan_amnt': ['count', 'mean'],
    'int_rate': 'mean',
    'annual_inc': 'median',
    'is_default': 'mean'
}).round(3)

grade_analysis.columns = ['count', 'avg_loan_amount', 'avg_interest_rate', 
                         'median_income', 'default_rate']

print("\nComprehensive Grade Analysis:")
print(grade_analysis)

# 4. Visualizations
fig, axes = plt.subplots(2, 2, figsize=(15, 12))

# Distribution by grade
df['grade'].value_counts().sort_index().plot(kind='bar', ax=axes[0,0])
axes[0,0].set_title('จำนวนสินเชื่อตาม Grade')
axes[0,0].set_ylabel('จำนวน')

# Default rate by grade
default_rates.plot(kind='bar', ax=axes[0,1], color='red')
axes[0,1].set_title('อัตราการผิดนัดชำระตาม Grade')
axes[0,1].set_ylabel('Default Rate')

# Interest rate by grade
grade_analysis['avg_interest_rate'].plot(kind='bar', ax=axes[1,0], color='green')
axes[1,0].set_title('อัตราดอกเบี้ยเฉลี่ยตาม Grade')
axes[1,0].set_ylabel('Interest Rate (%)')

# Loan amount by grade
grade_analysis['avg_loan_amount'].plot(kind='bar', ax=axes[1,1], color='blue')
axes[1,1].set_title('ขนาดสินเชื่อเฉลี่ยตาม Grade')
axes[1,1].set_ylabel('Loan Amount ($)')

plt.tight_layout()
plt.show()
```

---

## 📊 แบบฝึกหัดวันที่ 2: Data Preparation

### 🧹 **Exercise 2.1: Data Cleaning Challenge (30 นาที)**

**เป้าหมาย**: ทำความสะอาดข้อมูลอย่างเป็นระบบ

```python
# Data Cleaning Tasks:
# 1. ระบุและจัดการ missing values:
#    - คอลัมน์ไหนควร drop (missing > 50%)
#    - คอลัมน์ไหนควร impute และใช้วิธีไหน
# 2. แปลงประเภทข้อมูล:
#    - int_rate จาก string เป็น float
#    - revol_util จาก string เป็น float
#    - term จาก string เป็น numeric
# 3. จัดการ outliers:
#    - หา outliers ใน annual_inc
#    - ตัดสินใจว่าจะจัดการอย่างไร
# 4. สร้าง binary target variable สำหรับ ML

def clean_data(df):
    """
    ฟังก์ชันทำความสะอาดข้อมูล
    """
    # เขียนโค้ดของคุณที่นี่
    pass

# Test your function
df_clean = clean_data(df.copy())
```

**เฉลย**:
```python
def clean_data(df):
    """
    ฟังก์ชันทำความสะอาดข้อมูล Lending Club
    """
    df_clean = df.copy()
    
    # 1. จัดการ Missing Values
    # หาคอลัมน์ที่ missing > 50%
    missing_pct = (df_clean.isnull().sum() / len(df_clean)) * 100
    high_missing_cols = missing_pct[missing_pct > 50].index.tolist()
    
    print(f"Dropping {len(high_missing_cols)} columns with >50% missing values")
    df_clean = df_clean.drop(columns=high_missing_cols)
    
    # 2. แปลงประเภทข้อมูล
    # Interest rate
    if 'int_rate' in df_clean.columns:
        df_clean['int_rate'] = pd.to_numeric(
            df_clean['int_rate'].astype(str).str.rstrip('%'), 
            errors='coerce'
        )
    
    # Revolving utilization
    if 'revol_util' in df_clean.columns:
        df_clean['revol_util'] = pd.to_numeric(
            df_clean['revol_util'].astype(str).str.rstrip('%'), 
            errors='coerce'
        )
    
    # Term to numeric (36 months -> 36)
    if 'term' in df_clean.columns:
        df_clean['term_months'] = pd.to_numeric(
            df_clean['term'].astype(str).str.extract('(\d+)')[0], 
            errors='coerce'
        )
    
    # 3. จัดการ Outliers (annual_inc)
    if 'annual_inc' in df_clean.columns:
        Q1 = df_clean['annual_inc'].quantile(0.25)
        Q3 = df_clean['annual_inc'].quantile(0.75)
        IQR = Q3 - Q1
        lower_bound = Q1 - 1.5 * IQR
        upper_bound = Q3 + 1.5 * IQR
        
        # Cap outliers instead of removing
        df_clean['annual_inc_capped'] = df_clean['annual_inc'].clip(
            lower=lower_bound, upper=upper_bound
        )
    
    # 4. สร้าง Target Variable
    bad_statuses = ['Charged Off', 'Default', 'Late (31-120 days)']
    df_clean['is_bad_loan'] = df_clean['loan_status'].isin(bad_statuses).astype(int)
    
    # 5. Fill remaining missing values
    numeric_cols = df_clean.select_dtypes(include=[np.number]).columns
    for col in numeric_cols:
        df_clean[col] = df_clean[col].fillna(df_clean[col].median())
    
    print(f"Original shape: {df.shape}")
    print(f"Cleaned shape: {df_clean.shape}")
    print(f"Missing values remaining: {df_clean.isnull().sum().sum()}")
    
    return df_clean

# Test the function
df_clean = clean_data(df.copy())
```

### 🔧 **Exercise 2.2: Feature Engineering (25 นาที)**

**เป้าหมาย**: สร้าง features ใหม่ที่มีประโยชน์

```python
# Feature Engineering Tasks:
# 1. Financial Ratios:
#    - Debt-to-Income ratio improvement
#    - Payment-to-Income ratio
#    - Credit utilization categories
# 2. Credit Profile:
#    - FICO score average
#    - Credit age (from earliest_cr_line)
#    - Credit diversity score
# 3. Risk Indicators:
#    - Grade numeric conversion
#    - Delinquency score
#    - Inquiry intensity
# 4. Categorical Encoding:
#    - Purpose grouping
#    - State risk levels
#    - Employment length categories

def engineer_features(df):
    """
    สร้าง features ใหม่จากข้อมูลเดิม
    """
    # เขียนโค้ดของคุณที่นี่
    pass

# Test your function
df_features = engineer_features(df_clean.copy())
```

**เฉลย**:
```python
def engineer_features(df):
    """
    สร้าง features ใหม่จากข้อมูล Lending Club
    """
    df_features = df.copy()
    
    # 1. Financial Ratios
    # Payment to Income Ratio (monthly payment / monthly income)
    if 'installment' in df_features.columns and 'annual_inc' in df_features.columns:
        monthly_income = df_features['annual_inc'] / 12
        df_features['payment_to_income_ratio'] = df_features['installment'] / monthly_income
    
    # Loan to Income Ratio
    if 'loan_amnt' in df_features.columns and 'annual_inc' in df_features.columns:
        df_features['loan_to_income_ratio'] = df_features['loan_amnt'] / df_features['annual_inc']
    
    # Credit Utilization Categories
    if 'revol_util' in df_features.columns:
        def categorize_util(util):
            if pd.isna(util):
                return 'Unknown'
            elif util <= 10:
                return 'Excellent'
            elif util <= 30:
                return 'Good'
            elif util <= 50:
                return 'Fair'
            else:
                return 'Poor'
        
        df_features['revol_util_category'] = df_features['revol_util'].apply(categorize_util)
    
    # 2. Credit Profile
    # FICO Score Average
    if 'fico_range_low' in df_features.columns and 'fico_range_high' in df_features.columns:
        df_features['fico_avg'] = (df_features['fico_range_low'] + df_features['fico_range_high']) / 2
    
    # Credit History Length (if earliest_cr_line exists)
    if 'earliest_cr_line' in df_features.columns and 'issue_d' in df_features.columns:
        df_features['earliest_cr_line'] = pd.to_datetime(df_features['earliest_cr_line'], errors='coerce')
        df_features['issue_d'] = pd.to_datetime(df_features['issue_d'], errors='coerce')
        
        df_features['credit_history_years'] = (
            df_features['issue_d'] - df_features['earliest_cr_line']
        ).dt.days / 365.25
    
    # Credit Diversity (accounts types)
    if 'total_acc' in df_features.columns and 'open_acc' in df_features.columns:
        df_features['closed_acc'] = df_features['total_acc'] - df_features['open_acc']
        df_features['account_closure_rate'] = df_features['closed_acc'] / df_features['total_acc']
    
    # 3. Risk Indicators
    # Grade to Numeric
    if 'grade' in df_features.columns:
        grade_mapping = {'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7}
        df_features['grade_numeric'] = df_features['grade'].map(grade_mapping)
    
    # Delinquency Risk Score
    delinq_cols = ['delinq_2yrs', 'pub_rec', 'pub_rec_bankruptcies']
    available_delinq_cols = [col for col in delinq_cols if col in df_features.columns]
    
    if available_delinq_cols:
        df_features['delinquency_score'] = df_features[available_delinq_cols].fillna(0).sum(axis=1)
    
    # Inquiry Intensity
    if 'inq_last_6mths' in df_features.columns:
        def categorize_inquiries(inq):
            if pd.isna(inq) or inq == 0:
                return 'No_Activity'
            elif inq <= 2:
                return 'Low_Activity'
            elif inq <= 4:
                return 'Medium_Activity'
            else:
                return 'High_Activity'
        
        df_features['inquiry_intensity'] = df_features['inq_last_6mths'].apply(categorize_inquiries)
    
    # 4. Categorical Encoding
    # Purpose Grouping
    if 'purpose' in df_features.columns:
        purpose_groups = {
            'debt_consolidation': 'Debt_Management',
            'credit_card': 'Debt_Management',
            'home_improvement': 'Home_Investment',
            'major_purchase': 'Major_Purchase',
            'small_business': 'Business',
            'car': 'Transportation',
            'medical': 'Emergency',
            'moving': 'Life_Event',
            'vacation': 'Lifestyle',
            'wedding': 'Life_Event',
            'house': 'Home_Investment',
            'renewable_energy': 'Home_Investment',
            'educational': 'Education'
        }
        
        df_features['purpose_group'] = df_features['purpose'].map(purpose_groups).fillna('Other')
    
    # Employment Length Categories
    if 'emp_length' in df_features.columns:
        def categorize_emp_length(emp_length):
            if pd.isna(emp_length) or emp_length == 'n/a':
                return 'Unknown'
            elif '< 1' in str(emp_length):
                return 'Less_than_1_year'
            elif any(x in str(emp_length) for x in ['1 year', '2 years', '3 years']):
                return 'Short_term'
            elif any(x in str(emp_length) for x in ['4 years', '5 years', '6 years', '7 years']):
                return 'Medium_term'
            else:
                return 'Long_term'
        
        df_features['emp_length_category'] = df_features['emp_length'].apply(categorize_emp_length)
    
    print(f"Features added. New shape: {df_features.shape}")
    
    # Show new features
    original_cols = set(df.columns)
    new_cols = [col for col in df_features.columns if col not in original_cols]
    print(f"New features created: {new_cols}")
    
    return df_features

# Test the function
df_features = engineer_features(df_clean.copy())
```

### 📈 **Exercise 2.3: Statistical Analysis (20 นาที)**

**เป้าหมาย**: ทำการทดสอบทางสถิติเพื่อหาความสัมพันธ์

```python
# Statistical Testing Tasks:
# 1. T-test: เปรียบเทียบ FICO scores ระหว่าง good และ bad loans
# 2. Chi-square test: ทดสอบความสัมพันธ์ระหว่าง grade และ default
# 3. ANOVA: เปรียบเทียบ annual income ระหว่าง purpose groups
# 4. Correlation analysis: หาความสัมพันธ์ระหว่างตัวแปรตัวเลข

from scipy import stats

def statistical_analysis(df):
    """
    ทำการวิเคราะห์ทางสถิติ
    """
    # เขียนโค้ดของคุณที่นี่
    pass

# Run analysis
results = statistical_analysis(df_features)
```

**เฉลย**:
```python
from scipy import stats
import warnings
warnings.filterwarnings('ignore')

def statistical_analysis(df):
    """
    การวิเคราะห์ทางสถิติสำหรับข้อมูล Lending Club
    """
    results = {}
    
    print("=== STATISTICAL ANALYSIS RESULTS ===\n")
    
    # 1. T-test: FICO scores between good and bad loans
    if 'fico_avg' in df.columns and 'is_bad_loan' in df.columns:
        good_loans = df[df['is_bad_loan'] == 0]['fico_avg'].dropna()
        bad_loans = df[df['is_bad_loan'] == 1]['fico_avg'].dropna()
        
        t_stat, p_value = stats.ttest_ind(good_loans, bad_loans)
        
        results['fico_ttest'] = {
            't_statistic': t_stat,
            'p_value': p_value,
            'good_mean': good_loans.mean(),
            'bad_mean': bad_loans.mean(),
            'significant': p_value < 0.05
        }
        
        print("1. T-test: FICO Scores (Good vs Bad Loans)")
        print(f"   Good loans FICO mean: {good_loans.mean():.2f}")
        print(f"   Bad loans FICO mean: {bad_loans.mean():.2f}")
        print(f"   T-statistic: {t_stat:.4f}")
        print(f"   P-value: {p_value:.2e}")
        print(f"   Significant difference: {'Yes' if p_value < 0.05 else 'No'}\n")
    
    # 2. Chi-square test: Grade vs Default
    if 'grade' in df.columns and 'is_bad_loan' in df.columns:
        contingency_table = pd.crosstab(df['grade'], df['is_bad_loan'])
        chi2, p_value, dof, expected = stats.chi2_contingency(contingency_table)
        
        results['grade_chi2'] = {
            'chi2_statistic': chi2,
            'p_value': p_value,
            'degrees_of_freedom': dof,
            'significant': p_value < 0.05
        }
        
        print("2. Chi-square test: Grade vs Default")
        print(f"   Chi-square statistic: {chi2:.4f}")
        print(f"   P-value: {p_value:.2e}")
        print(f"   Degrees of freedom: {dof}")
        print(f"   Significant association: {'Yes' if p_value < 0.05 else 'No'}\n")
    
    # 3. ANOVA: Annual income across purpose groups
    if 'annual_inc' in df.columns and 'purpose_group' in df.columns:
        purpose_groups = []
        group_names = []
        
        for group in df['purpose_group'].unique():
            if pd.notna(group):
                group_data = df[df['purpose_group'] == group]['annual_inc'].dropna()
                if len(group_data) > 5:  # Minimum sample size
                    purpose_groups.append(group_data)
                    group_names.append(group)
        
        if len(purpose_groups) >= 2:
            f_stat, p_value = stats.f_oneway(*purpose_groups)
            
            results['income_anova'] = {
                'f_statistic': f_stat,
                'p_value': p_value,
                'groups_tested': group_names,
                'significant': p_value < 0.05
            }
            
            print("3. ANOVA: Annual Income across Purpose Groups")
            print(f"   F-statistic: {f_stat:.4f}")
            print(f"   P-value: {p_value:.4f}")
            print(f"   Groups tested: {len(group_names)}")
            print(f"   Significant difference: {'Yes' if p_value < 0.05 else 'No'}\n")
    
    # 4. Correlation Analysis
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    corr_cols = ['fico_avg', 'annual_inc', 'dti', 'revol_util', 'loan_amnt', 
                'grade_numeric', 'is_bad_loan']
    available_corr_cols = [col for col in corr_cols if col in numeric_cols]
    
    if len(available_corr_cols) >= 3:
        correlation_matrix = df[available_corr_cols].corr()
        
        # Find high correlations with target
        target_corr = correlation_matrix['is_bad_loan'].abs().sort_values(ascending=False)
        
        results['correlations'] = {
            'correlation_matrix': correlation_matrix,
            'target_correlations': target_corr
        }
        
        print("4. Correlation Analysis with Target Variable (is_bad_loan)")
        print("   Top correlations with default risk:")
        for var, corr in target_corr.head(6).items():
            if var != 'is_bad_loan':
                print(f"   {var}: {corr:.3f}")
    
    return results

# Run analysis
results = statistical_analysis(df_features)
```

---

## 🤖 แบบฝึกหัดวันที่ 3: Machine Learning

### 🎯 **Exercise 3.1: Binary Classification Model (40 นาที)**

**เป้าหมาย**: สร้างโมเดลทำนายการผิดนัดชำระ

```python
# Machine Learning Tasks:
# 1. เตรียมข้อมูลสำหรับ ML:
#    - เลือก features ที่เหมาะสม
#    - จัดการ missing values
#    - แบ่งข้อมูล train/test
# 2. สร้างและเปรียบเทียบโมเดล:
#    - Logistic Regression
#    - Random Forest
#    - Gradient Boosting
# 3. ประเมินผลโมเดล:
#    - Accuracy, Precision, Recall, F1-score
#    - ROC-AUC score
#    - Confusion Matrix
# 4. Feature importance analysis

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.metrics import classification_report, roc_auc_score, confusion_matrix

def build_ml_models(df):
    """
    สร้างและเปรียบเทียบโมเดล ML
    """
    # เขียนโค้ดของคุณที่นี่
    pass

# Build and evaluate models
model_results = build_ml_models(df_features)
```

**เฉลย**:
```python
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.metrics import classification_report, roc_auc_score, confusion_matrix
from sklearn.preprocessing import StandardScaler, LabelEncoder
import matplotlib.pyplot as plt
import seaborn as sns

def build_ml_models(df):
    """
    สร้างและเปรียบเทียบโมเดล ML สำหรับ credit risk prediction
    """
    # 1. Feature Selection
    # Numeric features
    numeric_features = [
        'loan_amnt', 'int_rate', 'installment', 'annual_inc', 'dti',
        'fico_avg', 'revol_util', 'delinq_2yrs', 'inq_last_6mths',
        'open_acc', 'pub_rec', 'grade_numeric'
    ]
    
    # Add engineered features if they exist
    engineered_features = [
        'payment_to_income_ratio', 'loan_to_income_ratio', 
        'credit_history_years', 'delinquency_score'
    ]
    
    # Select features that exist in dataframe
    available_features = [f for f in numeric_features + engineered_features 
                         if f in df.columns]
    
    print(f"Using {len(available_features)} features for modeling:")
    print(available_features)
    
    # 2. Prepare data
    X = df[available_features].copy()
    y = df['is_bad_loan'].copy()
    
    # Handle missing values
    X = X.fillna(X.median())
    
    # Remove rows with missing target
    mask = ~y.isnull()
    X = X[mask]
    y = y[mask]
    
    print(f"Final dataset shape: {X.shape}")
    print(f"Default rate: {y.mean():.2%}")
    
    # 3. Train-test split
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.3, random_state=42, stratify=y
    )
    
    # Scale features for logistic regression
    scaler = StandardScaler()
    X_train_scaled = scaler.fit_transform(X_train)
    X_test_scaled = scaler.transform(X_test)
    
    # 4. Train models
    models = {
        'Logistic Regression': LogisticRegression(random_state=42, max_iter=1000),
        'Random Forest': RandomForestClassifier(n_estimators=100, random_state=42),
        'Gradient Boosting': GradientBoostingClassifier(n_estimators=100, random_state=42)
    }
    
    results = {}
    
    print("\n=== MODEL TRAINING AND EVALUATION ===")
    
    for name, model in models.items():
        print(f"\nTraining {name}...")
        
        # Use scaled data for logistic regression, original for tree-based models
        if name == 'Logistic Regression':
            model.fit(X_train_scaled, y_train)
            y_pred_proba = model.predict_proba(X_test_scaled)[:, 1]
            y_pred = model.predict(X_test_scaled)
        else:
            model.fit(X_train, y_train)
            y_pred_proba = model.predict_proba(X_test)[:, 1]
            y_pred = model.predict(X_test)
        
        # Calculate metrics
        auc_score = roc_auc_score(y_test, y_pred_proba)
        
        results[name] = {
            'model': model,
            'auc_score': auc_score,
            'y_pred': y_pred,
            'y_pred_proba': y_pred_proba,
            'classification_report': classification_report(y_test, y_pred, output_dict=True)
        }
        
        print(f"{name} Results:")
        print(f"  AUC Score: {auc_score:.4f}")
        print(f"  Classification Report:")
        print(classification_report(y_test, y_pred))
    
    # 5. Feature Importance (Random Forest)
    if 'Random Forest' in results:
        rf_model = results['Random Forest']['model']
        feature_importance = pd.DataFrame({
            'feature': available_features,
            'importance': rf_model.feature_importances_
        }).sort_values('importance', ascending=False)
        
        print("\n=== FEATURE IMPORTANCE (Random Forest) ===")
        print(feature_importance.head(10))
        
        # Plot feature importance
        plt.figure(figsize=(10, 6))
        sns.barplot(data=feature_importance.head(10), y='feature', x='importance')
        plt.title('Top 10 Feature Importance (Random Forest)')
        plt.xlabel('Importance')
        plt.tight_layout()
        plt.show()
        
        results['feature_importance'] = feature_importance
    
    # 6. Model Comparison
    model_comparison = pd.DataFrame({
        'Model': list(results.keys()),
        'AUC Score': [results[model]['auc_score'] for model in results.keys()],
        'Precision': [results[model]['classification_report']['1']['precision'] for model in results.keys()],
        'Recall': [results[model]['classification_report']['1']['recall'] for model in results.keys()],
        'F1-Score': [results[model]['classification_report']['1']['f1-score'] for model in results.keys()]
    }).round(4)
    
    print("\n=== MODEL COMPARISON ===")
    print(model_comparison)
    
    # Plot model comparison
    fig, axes = plt.subplots(1, 2, figsize=(15, 5))
    
    # AUC comparison
    model_comparison.set_index('Model')['AUC Score'].plot(kind='bar', ax=axes[0])
    axes[0].set_title('AUC Score Comparison')
    axes[0].set_ylabel('AUC Score')
    axes[0].tick_params(axis='x', rotation=45)
    
    # Metrics comparison
    metrics_data = model_comparison.set_index('Model')[['Precision', 'Recall', 'F1-Score']]
    metrics_data.plot(kind='bar', ax=axes[1])
    axes[1].set_title('Performance Metrics Comparison')
    axes[1].set_ylabel('Score')
    axes[1].tick_params(axis='x', rotation=45)
    axes[1].legend()
    
    plt.tight_layout()
    plt.show()
    
    results['model_comparison'] = model_comparison
    results['test_data'] = (X_test, y_test)
    
    return results

# Build and evaluate models
model_results = build_ml_models(df_features)
```

### 📊 **Exercise 3.2: Customer Segmentation (30 นาที)**

**เป้าหมาย**: แบ่งกลุ่มลูกค้าด้วย Clustering

```python
# Customer Segmentation Tasks:
# 1. เตรียมข้อมูลสำหรับ clustering:
#    - เลือก features ที่เหมาะสม
#    - Normalize ข้อมูล
# 2. หาจำนวน clusters ที่เหมาะสม (Elbow method)
# 3. ทำ K-means clustering
# 4. วิเคราะห์ลักษณะของแต่ละ segment
# 5. สร้าง business recommendations

from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

def customer_segmentation(df):
    """
    แบ่งกลุ่มลูกค้าด้วย K-means clustering
    """
    # เขียนโค้ดของคุณที่นี่
    pass

# Perform customer segmentation
segmentation_results = customer_segmentation(df_features)
```

**เฉลย**:
```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
import matplotlib.pyplot as plt
import seaborn as sns

def customer_segmentation(df):
    """
    แบ่งกลุ่มลูกค้าด้วย K-means clustering
    """
    # 1. Select features for clustering
    clustering_features = [
        'annual_inc', 'loan_amnt', 'fico_avg', 'dti', 
        'revol_util', 'grade_numeric'
    ]
    
    # Check available features
    available_features = [f for f in clustering_features if f in df.columns]
    print(f"Using features for clustering: {available_features}")
    
    # Prepare data
    cluster_data = df[available_features].copy()
    
    # Handle missing values
    cluster_data = cluster_data.fillna(cluster_data.median())
    
    # Remove extreme outliers
    for col in cluster_data.columns:
        Q1 = cluster_data[col].quantile(0.01)
        Q3 = cluster_data[col].quantile(0.99)
        cluster_data[col] = cluster_data[col].clip(lower=Q1, upper=Q3)
    
    print(f"Data shape for clustering: {cluster_data.shape}")
    
    # 2. Normalize features
    scaler = StandardScaler()
    cluster_data_scaled = scaler.fit_transform(cluster_data)
    
    # 3. Find optimal number of clusters (Elbow method)
    inertias = []
    k_range = range(2, 11)
    
    for k in k_range:
        kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
        kmeans.fit(cluster_data_scaled)
        inertias.append(kmeans.inertia_)
    
    # Plot elbow curve
    plt.figure(figsize=(10, 6))
    plt.plot(k_range, inertias, 'bo-')
    plt.xlabel('Number of Clusters (k)')
    plt.ylabel('Inertia')
    plt.title('Elbow Method for Optimal k')
    plt.grid(True)
    plt.show()
    
    # 4. Perform K-means with optimal k (let's use 4)
    optimal_k = 4
    kmeans = KMeans(n_clusters=optimal_k, random_state=42, n_init=10)
    cluster_labels = kmeans.fit_predict(cluster_data_scaled)
    
    # Add cluster labels to original data
    cluster_data['cluster'] = cluster_labels
    
    print(f"\nCluster distribution:")
    print(cluster_data['cluster'].value_counts().sort_index())
    
    # 5. Analyze segments
    segment_analysis = cluster_data.groupby('cluster').agg({
        'annual_inc': ['mean', 'median'],
        'loan_amnt': ['mean', 'median'],
        'fico_avg': 'mean',
        'dti': 'mean',
        'revol_util': 'mean',
        'grade_numeric': 'mean'
    }).round(2)
    
    print("\n=== SEGMENT ANALYSIS ===")
    print(segment_analysis)
    
    # 6. Create segment profiles
    segments = {}
    for i in range(optimal_k):
        segment_data = cluster_data[cluster_data['cluster'] == i]
        
        segments[f'Segment_{i}'] = {
            'size': len(segment_data),
            'percentage': len(segment_data) / len(cluster_data) * 100,
            'avg_income': segment_data['annual_inc'].mean(),
            'avg_loan_amount': segment_data['loan_amnt'].mean(),
            'avg_fico': segment_data['fico_avg'].mean(),
            'avg_dti': segment_data['dti'].mean(),
            'avg_revol_util': segment_data['revol_util'].mean(),
            'avg_grade': segment_data['grade_numeric'].mean()
        }
    
    # 7. Segment characterization
    print("\n=== SEGMENT CHARACTERIZATION ===")
    
    segment_names = {
        0: "Conservative Borrowers",
        1: "High-Value Customers", 
        2: "Risk-Seeking Borrowers",
        3: "Budget-Conscious Borrowers"
    }
    
    for i, (segment_key, segment_info) in enumerate(segments.items()):
        segment_name = segment_names.get(i, f"Segment {i}")
        
        print(f"\n{segment_name} (Cluster {i}):")
        print(f"  Size: {segment_info['size']:,} customers ({segment_info['percentage']:.1f}%)")
        print(f"  Average Income: ${segment_info['avg_income']:,.0f}")
        print(f"  Average Loan Amount: ${segment_info['avg_loan_amount']:,.0f}")
        print(f"  Average FICO Score: {segment_info['avg_fico']:.0f}")
        print(f"  Average DTI: {segment_info['avg_dti']:.1f}%")
        print(f"  Average Grade: {segment_info['avg_grade']:.1f}")
    
    # 8. Visualizations
    fig, axes = plt.subplots(2, 3, figsize=(18, 12))
    
    # Income vs Loan Amount
    scatter = axes[0,0].scatter(cluster_data['annual_inc'], cluster_data['loan_amnt'], 
                               c=cluster_labels, cmap='viridis', alpha=0.6)
    axes[0,0].set_xlabel('Annual Income')
    axes[0,0].set_ylabel('Loan Amount')
    axes[0,0].set_title('Clusters: Income vs Loan Amount')
    plt.colorbar(scatter, ax=axes[0,0])
    
    # FICO vs DTI
    scatter = axes[0,1].scatter(cluster_data['fico_avg'], cluster_data['dti'], 
                               c=cluster_labels, cmap='viridis', alpha=0.6)
    axes[0,1].set_xlabel('FICO Score')
    axes[0,1].set_ylabel('DTI Ratio')
    axes[0,1].set_title('Clusters: FICO vs DTI')
    plt.colorbar(scatter, ax=axes[0,1])
    
    # Grade distribution by cluster
    grade_dist = pd.crosstab(cluster_data['cluster'], cluster_data['grade_numeric'])
    grade_dist.plot(kind='bar', stacked=True, ax=axes[0,2])
    axes[0,2].set_title('Grade Distribution by Cluster')
    axes[0,2].set_xlabel('Cluster')
    axes[0,2].legend(title='Grade Numeric')
    
    # Box plots for key features
    cluster_data.boxplot(column='annual_inc', by='cluster', ax=axes[1,0])
    axes[1,0].set_title('Income Distribution by Cluster')
    
    cluster_data.boxplot(column='loan_amnt', by='cluster', ax=axes[1,1])
    axes[1,1].set_title('Loan Amount Distribution by Cluster')
    
    cluster_data.boxplot(column='fico_avg', by='cluster', ax=axes[1,2])
    axes[1,2].set_title('FICO Score Distribution by Cluster')
    
    plt.tight_layout()
    plt.show()
    
    # 9. Business recommendations
    recommendations = {
        0: {
            "name": "Conservative Borrowers",
            "strategy": "Low-risk, competitive rates",
            "products": "Standard loans, loyalty programs",
            "marketing": "Trust and security messaging"
        },
        1: {
            "name": "High-Value Customers", 
            "strategy": "Premium service, larger loans",
            "products": "High-value loans, exclusive rates",
            "marketing": "VIP treatment, direct relationship management"
        },
        2: {
            "name": "Risk-Seeking Borrowers",
            "strategy": "Careful underwriting, higher rates",
            "products": "Smaller loans, financial education",
            "marketing": "Responsible lending, gradual credit building"
        },
        3: {
            "name": "Budget-Conscious Borrowers",
            "strategy": "Value proposition, transparent pricing",
            "products": "Small loans, financial tools",
            "marketing": "Cost savings, educational content"
        }
    }
    
    print("\n=== BUSINESS RECOMMENDATIONS ===")
    for cluster_id, rec in recommendations.items():
        print(f"\n{rec['name']} (Cluster {cluster_id}):")
        print(f"  Strategy: {rec['strategy']}")
        print(f"  Products: {rec['products']}")
        print(f"  Marketing: {rec['marketing']}")
    
    return {
        'cluster_data': cluster_data,
        'segment_analysis': segment_analysis,
        'segments': segments,
        'recommendations': recommendations,
        'kmeans_model': kmeans,
        'scaler': scaler
    }

# Perform customer segmentation
segmentation_results = customer_segmentation(df_features)
```

---

## 🎪 Challenge Problems

### 🏆 **Challenge 1: Credit Approval System (45 นาที)**

**เป้าหมาย**: สร้างระบบอนุมัติสินเชื่ออัตโนมัติ

สร้างระบบที่สามารถ:
1. รับข้อมูลใบสมัครใหม่
2. คำนวณ risk score
3. ตัดสินใจอนุมัติ/ปฏิเสธ
4. กำหนดอัตราดอกเบี้ยที่เหมาะสม
5. ให้เหตุผลการตัดสินใจ

### 🏆 **Challenge 2: Portfolio Optimization (60 นาที)**

**เป้าหมาย**: หาส่วนผสมที่เหมาะสมของ portfolio

วัตถุประสงค์:
- เพิ่มผลตอบแทนเป็น 15%
- ลดความเสี่ยงเป็น 8%
- กระจายการลงทุนในหลาย grades
- สร้างกลยุทธ์ rebalancing

### 🏆 **Challenge 3: Early Warning System (50 นาที)**

**เป้าหมาย**: ระบบเตือนภัยล่วงหน้า

ระบบต้องสามารถ:
1. ระบุสินเชื่อที่มีความเสี่ยงสูง
2. จัดอันดับตามระดับความเสี่ยง
3. แนะนำการดำเนินการ
4. ติดตามผลลัพธ์

---

## 🏆 Mini Projects

### 📊 **Project 1: Credit Risk Dashboard (90 นาที)**

สร้าง interactive dashboard ที่แสดง:
- Portfolio overview และ KPIs
- Risk distribution และ trends
- Performance metrics ตาม segments
- Geographic analysis
- Alerts และ recommendations

### 📈 **Project 2: Customer Lifetime Value Model (120 นาที)**

พัฒนาโมเดลที่:
- ทำนาย customer lifetime value
- แบ่งกลุ่มลูกค้าตาม CLV
- สร้างกลยุทธ์การตลาดตามกลุ่ม
- คำนวณ ROI ของแต่ละ segment

### 🎯 **Project 3: A/B Testing Framework (100 นาที)**

ออกแบบ framework สำหรับ:
- ทดสอบกลยุทธ์ pricing ใหม่
- เปรียบเทียบ approval criteria
- วัดผลกระทบต่อธุรกิจ
- Statistical significance testing

---

## 💡 เคล็ดลับการทำแบบฝึกหัด

### 🎯 **สำหรับผู้เริ่มต้น**
1. **เริ่มจากง่าย**: ทำ basic exercises ก่อน
2. **ดูเฉลยหลังพยายาม**: อย่าดูเฉลยทันที
3. **ทำความเข้าใจ**: อ่านโค้ดและอธิบายได้
4. **ปรับแต่ง**: ลองเปลี่ยนพารามิเตอร์

### 🔧 **สำหรับระดับกลาง**
1. **เน้นคุณภาพ**: ใส่ใจ code quality
2. **เพิ่มความซับซ้อน**: ลองวิธีที่ยากขึ้น
3. **ตีความผล**: อธิบาย business implications
4. **เปรียบเทียบ**: ลองหลายวิธีและเปรียบเทียบ

### 🚀 **สำหรับระดับสูง**
1. **สร้างของใหม่**: ออกแบบ solutions เอง
2. **ปรับปรุง**: หาวิธีทำให้ดีขึ้น
3. **ประยุกต์ใช้**: นำไปใช้กับข้อมูลอื่น
4. **แชร์ความรู้**: สอนคนอื่น

### 📝 **การส่งงาน**
- **โค้ด**: สะอาด มี comments
- **ผลลัพธ์**: แสดงผลชัดเจน
- **การตีความ**: อธิบาย insights
- **ข้อเสนอแนะ**: business recommendations

---

*📖 หมายเหตุ: แบบฝึกหัดเหล่านี้ออกแบบมาให้สอดคล้องกับเนื้อหาในหลักสูตร และสามารถปรับระดับความยากได้ตามความสามารถของผู้เรียน*

---
[⬅️ กลับไปที่ Course Applications](./07_course_applications.md) | [➡️ ไปยัง Learning Outcomes](./09_learning_outcomes.md)
