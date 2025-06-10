# 🎯 Business Questions สำหรับการวิเคราะห์ข้อมูลทุกประเภท

## 📋 สารบัญ
- [📊 Descriptive Analytics](#-descriptive-analytics)
- [📈 Diagnostic Analytics](#-diagnostic-analytics)
- [🔮 Predictive Analytics](#-predictive-analytics)
- [🎯 Prescriptive Analytics](#-prescriptive-analytics)
- [📊 Data Visualization Questions](#-data-visualization-questions)
- [🤖 Machine Learning Applications](#-machine-learning-applications)
- [💼 Business Intelligence & KPIs](#-business-intelligence--kpis)

---

## 📊 Descriptive Analytics
*"What happened?" - อธิบายสิ่งที่เกิดขึ้นในอดีต*

### 🎯 **Portfolio Overview Questions**

#### **1. ภาพรวมธุรกิจ (Business Overview)**
```python
# Q1: การเติบโตของธุรกิจตามช่วงเวลา
import pandas as pd
import numpy as np

# สร้างข้อมูลเวลา
df['issue_d'] = pd.to_datetime(df['issue_d'])
df['issue_year'] = df['issue_d'].dt.year
df['issue_month'] = df['issue_d'].dt.month

monthly_growth = df.groupby([df['issue_d'].dt.to_period('M')]).agg({
    'loan_amnt': ['count', 'sum', 'mean'],
    'int_rate': 'mean'
}).round(2)

print("Monthly Business Growth:")
print(monthly_growth.head(10))
```

**Business Impact**: เข้าใจ trend การเติบโต, seasonal patterns, และการเปลี่ยนแปลงของพฤติกรรมลูกค้า

#### **2. การกระจายตามกลุ่มความเสี่ยง**
```python
# Q2: สัดส่วนสินเชื่อตามเกรด
grade_distribution = df.groupby('grade').agg({
    'loan_amnt': ['count', 'sum', 'mean'],
    'int_rate': 'mean',
    'is_bad_loan': 'mean'
}).round(2)

print("การกระจายสินเชื่อตามเกรด:")
print(grade_distribution)
```

**Business Questions**:
- เกรดไหนมีปริมาณธุรกิจมากที่สุด?
- อัตราดอกเบี้ยสะท้อนความเสี่ยงได้ดีหรือไม่?
- เกรดไหนควรขยายธุรกิจ/ลดความเสี่ยง?

### 💰 **Financial Performance**

#### **3. การวิเคราะห์ผลตอบแทน**
```python
# Q3: ROI Analysis
completed_loans = df[df['loan_status'].isin(['Fully Paid', 'Charged Off'])].copy()
completed_loans['total_return'] = completed_loans['total_pymnt'] + completed_loans['recoveries']
completed_loans['net_return'] = completed_loans['total_return'] - completed_loans['funded_amnt']
completed_loans['roi_pct'] = (completed_loans['net_return'] / completed_loans['funded_amnt']) * 100

# ROI by grade
roi_by_grade = completed_loans.groupby('grade')['roi_pct'].agg(['mean', 'median', 'std'])
print("ROI by Grade:")
print(roi_by_grade)
```

---

## 📈 Diagnostic Analytics
*"Why did it happen?" - วิเคราะห์สาเหตุของสิ่งที่เกิดขึ้น*

### 🔍 **Risk Factor Analysis**

#### **4. การวิเคราะห์ปัจจัยความเสี่ยง**
```python
# Q4: ปัจจัยที่ส่งผลต่อการผิดนัดชำระ
import scipy.stats as stats

# Chi-square test for categorical variables
categorical_vars = ['grade', 'home_ownership', 'purpose', 'verification_status']
risk_analysis = {}

for var in categorical_vars:
    contingency_table = pd.crosstab(df[var], df['is_bad_loan'])
    chi2, p_value, dof, expected = stats.chi2_contingency(contingency_table)
    risk_analysis[var] = {
        'chi2': chi2,
        'p_value': p_value,
        'significant': p_value < 0.05
    }

for var, stats_data in risk_analysis.items():
    print(f"{var}: Chi-square = {stats_data['chi2']:.2f}, p-value = {stats_data['p_value']:.4f}")
```

#### **5. Cohort Analysis**
```python
# Q5: วิเคราะห์ performance ตาม cohort (ปีที่ออกสินเชื่อ)
cohort_performance = df.groupby('issue_year').agg({
    'is_bad_loan': 'mean',
    'loan_amnt': ['count', 'sum'],
    'int_rate': 'mean'
}).round(3)

print("Cohort Performance by Year:")
print(cohort_performance)
```

---

## 🔮 Predictive Analytics
*"What will happen?" - ทำนายเหตุการณ์ในอนาคต*

### 🤖 **Credit Risk Modeling**

#### **6. Default Prediction Model**
```python
# Q6: สร้างโมเดลทำนายการผิดนัดชำระ
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, roc_auc_score
from sklearn.preprocessing import LabelEncoder

# Feature selection
features = [
    'loan_amnt', 'int_rate', 'installment', 'annual_inc', 'dti',
    'fico_range_low', 'fico_range_high', 'revol_util', 'delinq_2yrs', 
    'inq_last_6mths', 'open_acc', 'pub_rec'
]

# Prepare data
X = df[features].fillna(df[features].median())
y = df['is_bad_loan']

# Remove any remaining NaN values
mask = ~(X.isnull().any(axis=1) | y.isnull())
X = X[mask]
y = y[mask]

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)

# Train model
rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)

# Evaluate
y_pred_proba = rf_model.predict_proba(X_test)[:, 1]
auc_score = roc_auc_score(y_test, y_pred_proba)

print(f"AUC Score: {auc_score:.3f}")

# Feature importance
feature_importance = pd.DataFrame({
    'feature': features,
    'importance': rf_model.feature_importances_
}).sort_values('importance', ascending=False)

print("\nTop 5 Most Important Features:")
print(feature_importance.head())
```

---

## 🎯 Prescriptive Analytics
*"What should we do?" - แนะนำการดำเนินการ*

### 🎯 **Customer Acquisition Strategy**

#### **7. การแบ่งกลุ่มลูกค้าเป้าหมาย**
```python
# Q7: Customer segmentation for targeted marketing
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

# Features for segmentation
segmentation_features = ['annual_inc', 'loan_amnt', 'dti', 'fico_range_low']

# Prepare data (remove NaN)
segment_data = df[segmentation_features].dropna()
scaler = StandardScaler()
scaled_data = scaler.fit_transform(segment_data)

# K-means clustering
kmeans = KMeans(n_clusters=4, random_state=42)
segment_labels = kmeans.fit_predict(scaled_data)

# Add segments back to original dataframe
segment_df = df[segmentation_features].dropna().copy()
segment_df['customer_segment'] = segment_labels

# Analyze segments
segment_analysis = segment_df.groupby('customer_segment').agg({
    'annual_inc': ['mean', 'median'],
    'loan_amnt': ['mean', 'median'],
    'fico_range_low': 'mean',
    'dti': 'mean'
}).round(2)

print("Customer Segments Analysis:")
print(segment_analysis)
```

### 🔧 **Operational Optimization**

#### **8. Credit Decision Automation**
```python
# Q8: สร้างระบบตัดสินใจเครดิตอัตโนมัติ
def automated_credit_decision(loan_application):
    """
    ระบบตัดสินใจอัตโนมัติสำหรับการอนุมัติสินเชื่อ
    """
    score = 0
    decision_factors = []
    
    # FICO Score (40% weight)
    fico = loan_application.get('fico_score', 0)
    if fico >= 740:
        score += 40
        decision_factors.append("Excellent FICO score")
    elif fico >= 680:
        score += 30
        decision_factors.append("Good FICO score")
    elif fico >= 620:
        score += 20
        decision_factors.append("Fair FICO score")
    else:
        score += 0
        decision_factors.append("Poor FICO score - high risk")
    
    # DTI Ratio (25% weight)
    dti = loan_application.get('dti', 100)
    if dti <= 15:
        score += 25
        decision_factors.append("Low DTI ratio")
    elif dti <= 25:
        score += 20
        decision_factors.append("Moderate DTI ratio")
    elif dti <= 35:
        score += 10
        decision_factors.append("High DTI ratio")
    else:
        score += 0
        decision_factors.append("Very high DTI ratio - risk factor")
    
    # Income Verification (15% weight)
    verification = loan_application.get('verification_status', 'Not Verified')
    if verification == 'Verified':
        score += 15
        decision_factors.append("Income verified")
    elif verification == 'Source Verified':
        score += 10
        decision_factors.append("Income source verified")
    else:
        score += 0
        decision_factors.append("Income not verified")
    
    # Delinquency History (20% weight)
    delinq = loan_application.get('delinq_2yrs', 0)
    if delinq == 0:
        score += 20
        decision_factors.append("No recent delinquencies")
    elif delinq <= 2:
        score += 10
        decision_factors.append("Some recent delinquencies")
    else:
        score += 0
        decision_factors.append("Multiple delinquencies - major risk")
    
    # Make decision
    if score >= 80:
        decision = "APPROVE"
        suggested_rate = "Best rate tier"
    elif score >= 60:
        decision = "APPROVE"
        suggested_rate = "Standard rate tier"
    elif score >= 40:
        decision = "CONDITIONAL APPROVE"
        suggested_rate = "Higher rate tier + additional verification"
    else:
        decision = "DECLINE"
        suggested_rate = "N/A"
    
    return {
        'score': score,
        'decision': decision,
        'suggested_rate_tier': suggested_rate,
        'decision_factors': decision_factors
    }

# ตัวอย่างการใช้งาน
sample_application = {
    'fico_score': 720,
    'dti': 18.5,
    'verification_status': 'Verified',
    'delinq_2yrs': 0
}

result = automated_credit_decision(sample_application)
print("Credit Decision Result:")
print(f"Score: {result['score']}/100")
print(f"Decision: {result['decision']}")
print(f"Rate Tier: {result['suggested_rate_tier']}")
```

---

## 📊 Data Visualization Questions

### 📈 **Executive Dashboard Questions**

#### **9. Risk Heatmap**
```python
# Q9: สร้าง Risk Heatmap
import matplotlib.pyplot as plt
import seaborn as sns

# Risk by State and Purpose (top states only)
top_states = df['addr_state'].value_counts().head(10).index
df_subset = df[df['addr_state'].isin(top_states)]

risk_matrix = pd.crosstab(
    df_subset['addr_state'], 
    df_subset['purpose'], 
    values=df_subset['is_bad_loan'], 
    aggfunc='mean'
) * 100

# Create heatmap
plt.figure(figsize=(12, 8))
sns.heatmap(risk_matrix, annot=True, cmap='RdYlBu_r', fmt='.1f')
plt.title('Default Rate (%) by State and Purpose')
plt.xlabel('Loan Purpose')
plt.ylabel('State')
plt.tight_layout()
plt.show()
```

#### **10. Performance Trends**
```python
# Q10: Monthly performance trends
monthly_trends = df.groupby(df['issue_d'].dt.to_period('M')).agg({
    'loan_amnt': ['count', 'sum'],
    'int_rate': 'mean',
    'is_bad_loan': 'mean'
}).round(3)

# Plot trends
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# Loan volume
monthly_trends['loan_amnt']['count'].plot(ax=axes[0,0], title='Monthly Loan Count')
monthly_trends['loan_amnt']['sum'].plot(ax=axes[0,1], title='Monthly Loan Amount ($)')
monthly_trends['int_rate']['mean'].plot(ax=axes[1,0], title='Average Interest Rate (%)')
monthly_trends['is_bad_loan']['mean'].plot(ax=axes[1,1], title='Default Rate')

plt.tight_layout()
plt.show()
```

---

## 🤖 Machine Learning Applications

### 🎯 **Classification Problems**

#### **11. Multi-class Loan Status Prediction**
```python
# Q11: ทำนาย loan status แบบ multi-class
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

# Define classes
status_mapping = {
    'Fully Paid': 0,
    'Current': 1,
    'Charged Off': 2,
    'Late (31-120 days)': 3,
    'Late (16-30 days)': 4,
    'In Grace Period': 5
}

# Prepare multi-class target
df_ml = df[df['loan_status'].isin(status_mapping.keys())].copy()
df_ml['status_code'] = df_ml['loan_status'].map(status_mapping)

# Features
features = [
    'loan_amnt', 'int_rate', 'installment', 'annual_inc', 'dti',
    'fico_range_low', 'revol_util', 'delinq_2yrs', 'inq_last_6mths',
    'open_acc', 'pub_rec'
]

# Prepare data
X = df_ml[features].fillna(df_ml[features].median())
y = df_ml['status_code']

# Remove any remaining NaN values
mask = ~(X.isnull().any(axis=1) | y.isnull())
X = X[mask]
y = y[mask]

# Train model
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)

rf_multiclass = RandomForestClassifier(n_estimators=100, random_state=42)
rf_multiclass.fit(X_train, y_train)

y_pred_multi = rf_multiclass.predict(X_test)
print("Multi-class Classification Report:")
print(classification_report(y_test, y_pred_multi))
```

### 📊 **Clustering Applications**

#### **12. Customer Lifetime Value Segmentation**
```python
# Q12: แบ่งกลุ่มลูกค้าตาม CLV
# สร้าง customer-level aggregation (simplified version)
customer_features = df.groupby('member_id').agg({
    'loan_amnt': ['sum', 'count', 'mean'],
    'total_rec_int': 'sum',
    'is_bad_loan': 'mean',
    'int_rate': 'mean'
}).fillna(0)

# Flatten column names
customer_features.columns = ['total_borrowed', 'num_loans', 'avg_loan_size',
                           'total_interest_paid', 'default_rate', 'avg_interest_rate']

# Calculate CLV proxy
customer_features['clv_proxy'] = (
    customer_features['total_interest_paid'] - 
    customer_features['total_borrowed'] * customer_features['default_rate'] * 0.5
)

# Remove outliers and perform clustering
Q1 = customer_features['clv_proxy'].quantile(0.25)
Q3 = customer_features['clv_proxy'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

customer_clean = customer_features[
    (customer_features['clv_proxy'] >= lower_bound) & 
    (customer_features['clv_proxy'] <= upper_bound)
]

# Clustering
cluster_features = ['total_borrowed', 'num_loans', 'total_interest_paid', 'default_rate']
scaler = StandardScaler()
scaled_clv = scaler.fit_transform(customer_clean[cluster_features])

kmeans_clv = KMeans(n_clusters=4, random_state=42)
customer_clean['clv_segment'] = kmeans_clv.fit_predict(scaled_clv)

# Analyze segments
clv_analysis = customer_clean.groupby('clv_segment').agg({
    'total_borrowed': 'mean',
    'num_loans': 'mean',
    'total_interest_paid': 'mean',
    'default_rate': 'mean',
    'clv_proxy': 'mean'
}).round(2)

print("Customer Lifetime Value Segments:")
print(clv_analysis)
```

---

## 💼 Business Intelligence & KPIs

### 📊 **Financial KPIs**

#### **13. ROI และ Risk-Adjusted Returns**
```python
# Q13: คำนวณ KPIs ทางการเงินหลัก
def calculate_financial_kpis(df):
    """
    คำนวณ KPIs ทางการเงินสำคัญ
    """
    kpis = {}
    
    # Portfolio Size
    kpis['total_loans'] = len(df)
    kpis['total_amount'] = df['loan_amnt'].sum()
    kpis['avg_loan_size'] = df['loan_amnt'].mean()
    
    # Risk Metrics
    kpis['default_rate'] = df['is_bad_loan'].mean()
    kpis['charge_off_rate'] = (df['loan_status'] == 'Charged Off').mean()
    
    # Profitability (for completed loans only)
    completed_loans = df[df['loan_status'].isin(['Fully Paid', 'Charged Off'])]
    if len(completed_loans) > 0:
        kpis['total_interest_income'] = completed_loans['total_rec_int'].sum()
        kpis['total_losses'] = completed_loans[
            completed_loans['is_bad_loan'] == 1
        ]['funded_amnt'].sum()
        
        # Net Interest Margin
        kpis['net_interest_margin'] = (
            kpis['total_interest_income'] - kpis['total_losses']
        ) / kpis['total_amount']
    else:
        kpis['total_interest_income'] = 0
        kpis['total_losses'] = 0
        kpis['net_interest_margin'] = 0
    
    return kpis

# คำนวณและแสดง KPIs
financial_kpis = calculate_financial_kpis(df)

print("=== KEY FINANCIAL KPIs ===")
for kpi, value in financial_kpis.items():
    if isinstance(value, float):
        if 'rate' in kpi or 'margin' in kpi:
            print(f"{kpi}: {value:.2%}")
        else:
            print(f"{kpi}: ${value:,.2f}")
    else:
        print(f"{kpi}: {value:,}")
```

#### **14. Performance Benchmarking**
```python
# Q14: เปรียบเทียบ performance กับ benchmark
def benchmark_analysis(df, benchmark_default_rate=0.12, benchmark_nim=0.08):
    """
    เปรียบเทียบ performance กับ industry benchmark
    """
    actual_default_rate = df['is_bad_loan'].mean()
    
    # Calculate actual NIM (simplified)
    completed_loans = df[df['loan_status'].isin(['Fully Paid', 'Charged Off'])]
    if len(completed_loans) > 0:
        total_interest = completed_loans['total_rec_int'].sum()
        total_principal = completed_loans['funded_amnt'].sum()
        actual_nim = total_interest / total_principal if total_principal > 0 else 0
    else:
        actual_nim = 0
    
    benchmark_results = {
        'default_rate': {
            'actual': actual_default_rate,
            'benchmark': benchmark_default_rate,
            'variance': actual_default_rate - benchmark_default_rate,
            'performance': 'Better' if actual_default_rate < benchmark_default_rate else 'Worse'
        },
        'net_interest_margin': {
            'actual': actual_nim,
            'benchmark': benchmark_nim,
            'variance': actual_nim - benchmark_nim,
            'performance': 'Better' if actual_nim > benchmark_nim else 'Worse'
        }
    }
    
    return benchmark_results

benchmark_results = benchmark_analysis(df)

print("=== BENCHMARK ANALYSIS ===")
for metric, data in benchmark_results.items():
    print(f"\n{metric.upper()}:")
    print(f"  Actual: {data['actual']:.2%}")
    print(f"  Benchmark: {data['benchmark']:.2%}")
    print(f"  Variance: {data['variance']:.2%}")
    print(f"  Performance: {data['performance']}")
```

### 📈 **Operational KPIs**

#### **15. Process Efficiency Metrics**
```python
# Q15: วิเคราะห์ประสิทธิภาพการดำเนินงาน
def operational_efficiency_analysis(df):
    """
    วิเคราะห์ KPIs ด้านการดำเนินงาน
    """
    efficiency_metrics = {}
    
    # Funding Efficiency
    efficiency_metrics['funding_ratio'] = (
        df['funded_amnt'].sum() / df['loan_amnt'].sum()
    )
    
    # Portfolio Diversification
    purpose_distribution = df['purpose'].value_counts(normalize=True)
    efficiency_metrics['portfolio_concentration'] = purpose_distribution.iloc[0]
    
    # Grade Distribution Health
    grade_distribution = df['grade'].value_counts(normalize=True)
    efficiency_metrics['grade_a_b_ratio'] = (
        grade_distribution.get('A', 0) + grade_distribution.get('B', 0)
    )
    
    # Geographic Diversification
    state_distribution = df['addr_state'].value_counts(normalize=True)
    efficiency_metrics['geographic_concentration'] = state_distribution.iloc[0]
    
    # Verification Rate
    verification_rate = df['verification_status'].isin(['Verified', 'Source Verified']).mean()
    efficiency_metrics['verification_rate'] = verification_rate
    
    return efficiency_metrics

operational_kpis = operational_efficiency_analysis(df)

print("=== OPERATIONAL EFFICIENCY KPIs ===")
for kpi, value in operational_kpis.items():
    print(f"{kpi}: {value:.2%}")
```

---

## 🎓 การประยุกต์ใช้ในหลักสูตร

### 📚 **วันที่ 1: Essential Data Analytics**
- Questions 1-3: Descriptive Analytics
- Questions 9-10: Basic Visualizations
- Focus: เข้าใจข้อมูล, สร้าง insights เบื้องต้น

### 📊 **วันที่ 2: Data Profiling & Preparation**
- Questions 4-5: Diagnostic Analytics
- Questions 13-15: KPI Development
- Focus: ทำความสะอาดข้อมูล, สร้าง features

### 🤖 **วันที่ 3: Predictive Analytics & ML**
- Questions 6-8: Predictive & Prescriptive Analytics
- Questions 11-12: Advanced ML Applications
- Focus: สร้างโมเดล, การประยุกต์ใช้

### 💡 **เคล็ดลับการใช้โจทย์**

1. **เริ่มจากง่ายไปยาก**: ใช้ descriptive ก่อน แล้วค่อยเพิ่มความซับซ้อน
2. **เชื่อมโยงกับธุรกิจ**: อธิบาย business impact ของแต่ละ analysis
3. **ใช้ข้อมูลจริง**: ยกตัวอย่างจากข้อมูล Lending Club
4. **ตรวจสอบผลลัพธ์**: ใช้หลายวิธีในการ validate insights
5. **สร้าง narrative**: เล่าเรื่องจากข้อมูลให้น่าสนใจ

### 🎯 **การปรับใช้ตามระดับความยาก**

#### **ระดับเริ่มต้น (Beginner)**
- Questions 1-3, 9: Basic descriptive analysis
- เน้นการใช้ pandas groupby, basic plotting
- เรียนรู้การตีความข้อมูล

#### **ระดับกลาง (Intermediate)**
- Questions 4-5, 10-11: Statistical analysis, advanced visualization
- เน้นการใช้ statistical tests, cohort analysis
- เรียนรู้การหาสาเหตุและความสัมพันธ์

#### **ระดับสูง (Advanced)**
- Questions 6-8, 12-15: Machine learning, optimization
- เน้นการสร้างโมเดล ML, business optimization
- เรียนรู้การสร้างระบบตัดสินใจอัตโนมัติ

---

*📖 หมายเหตุ: โจทย์ธุรกิจเหล่านี้ออกแบบมาให้ครอบคลุมทุกด้านของการวิเคราะห์ข้อมูล และสามารถปรับระดับความยากตามความสามารถของผู้เรียน*

---
[⬅️ กลับไปที่ Key Variables](./04_key_variables.md) | [➡️ ไปยัง Case Studies](./06_case_studies.md)