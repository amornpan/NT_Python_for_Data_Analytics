# 🎓 การประยุกต์ใช้ในหลักสูตร

## 📋 สารบัญ
- [📚 วันที่ 1: Essential Data Analytics](#-วันที่-1-essential-data-analytics)
- [📊 วันที่ 2: Data Profiling & Preparation](#-วันที่-2-data-profiling--preparation)
- [🤖 วันที่ 3: Predictive Analytics & ML](#-วันที่-3-predictive-analytics--ml)
- [🔗 การเชื่อมโยงระหว่างวัน](#-การเชื่อมโยงระหว่างวัน)
- [💡 Tips สำหรับผู้สอน](#-tips-สำหรับผู้สอน)

---

## 📚 วันที่ 1: Essential Data Analytics
*เน้นการทำความเข้าใจข้อมูลและการสร้าง insights เบื้องต้น*

### 🎯 **Learning Objectives**
- เข้าใจโครงสร้างและลักษณะของข้อมูล Lending Club
- สามารถใช้ pandas ในการสำรวจและวิเคราะห์ข้อมูลเบื้องต้น
- สร้าง basic visualizations เพื่อสื่อสาร insights
- เข้าใจ business context ของ P2P lending

### 📊 **Morning Session (3 hours)**

#### **Hour 1: Data Introduction & Setup**
```python
# 1.1 Loading และ Basic Exploration
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
df = pd.read_csv('LoanStats_web_14422.csv')

print("=== BASIC DATA EXPLORATION ===")
print(f"Dataset shape: {df.shape}")
print(f"Memory usage: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.head())
```

#### **Hour 2: Business Growth Analysis**
```python
# 1.2 การเติบโตของธุรกิจตามเวลา
df['issue_d'] = pd.to_datetime(df['issue_d'])
df['issue_year'] = df['issue_d'].dt.year

yearly_summary = df.groupby('issue_year').agg({
    'loan_amnt': ['count', 'sum', 'mean'],
    'int_rate': 'mean'
}).round(2)

print("=== BUSINESS GROWTH ANALYSIS ===")
print(yearly_summary)
```

#### **Hour 3: Risk Grade Analysis**
```python
# 1.3 วิเคราะห์ตาม Grade
grade_analysis = df.groupby('grade').agg({
    'loan_amnt': ['count', 'sum', 'mean'],
    'int_rate': 'mean',
    'annual_inc': 'mean'
}).round(2)

print("=== GRADE ANALYSIS ===")
print(grade_analysis)
```

### 🕐 **Afternoon Session (3 hours)**

#### **Hour 4-6: Geographic & Purpose Analysis**
```python
# 1.4 การกระจายทางภูมิศาสตร์และวัตถุประสงค์
top_states = df['addr_state'].value_counts().head(10)
purpose_analysis = df['purpose'].value_counts()

print("=== GEOGRAPHIC & PURPOSE ANALYSIS ===")
print(top_states)
print(purpose_analysis)
```

### 📝 **Day 1 Deliverables**
1. **Data Exploration Report**: สรุปลักษณะของข้อมูล
2. **Business Growth Analysis**: กราฟและ insights การเติบโต
3. **Customer Profile Summary**: ลักษณะของลูกค้าแต่ละ grade

---

## 📊 วันที่ 2: Data Profiling & Preparation
*เน้นการทำความสะอาดข้อมูลและการสร้าง features*

### 🎯 **Learning Objectives**
- ทำความสะอาดและเตรียมข้อมูลสำหรับการวิเคราะห์
- สร้าง derived features และ business metrics
- ทำ statistical analysis และ hypothesis testing
- พัฒนา KPIs และ performance metrics

### 📊 **Morning Session (3 hours)**

#### **Hour 1: Data Quality Assessment**
```python
# 2.1 ประเมินคุณภาพข้อมูล
def assess_data_quality(df):
    quality_report = pd.DataFrame({
        'Column': df.columns,
        'Missing_Count': df.isnull().sum(),
        'Missing_Percentage': (df.isnull().sum() / len(df)) * 100,
        'Unique_Values': df.nunique()
    })
    return quality_report.sort_values('Missing_Percentage', ascending=False)

quality_report = assess_data_quality(df)
print("=== DATA QUALITY REPORT ===")
print(quality_report.head(20))
```

#### **Hour 2: Data Cleaning**
```python
# 2.2 ทำความสะอาดข้อมูล
def clean_lending_data(df):
    df_clean = df.copy()
    
    # Convert percentage columns
    if 'int_rate' in df_clean.columns:
        df_clean['int_rate'] = pd.to_numeric(
            df_clean['int_rate'].astype(str).str.rstrip('%'), 
            errors='coerce'
        )
    
    # Create binary target
    df_clean['is_bad_loan'] = (df_clean['loan_status'] == 'Charged Off').astype(int)
    
    return df_clean

df_clean = clean_lending_data(df)
print(f"Shape after cleaning: {df_clean.shape}")
```

#### **Hour 3: Feature Engineering**
```python
# 2.3 สร้าง Features ใหม่
def create_derived_features(df):
    df_features = df.copy()
    
    # FICO Score average
    if 'fico_range_low' in df.columns and 'fico_range_high' in df.columns:
        df_features['fico_avg'] = (df['fico_range_low'] + df['fico_range_high']) / 2
    
    # Income to loan ratio
    df_features['income_to_loan_ratio'] = df['annual_inc'] / df['loan_amnt']
    
    # Grade to numeric
    grade_mapping = {'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7}
    df_features['grade_numeric'] = df['grade'].map(grade_mapping)
    
    return df_features

df_features = create_derived_features(df_clean)
```

### 🕐 **Afternoon Session (3 hours)**

#### **Hour 4: Statistical Analysis**
```python
# 2.4 การทดสอบทางสถิติ
from scipy import stats

# T-test: FICO scores between good and bad loans
good_loans = df_features[df_features['is_bad_loan'] == 0]['fico_avg'].dropna()
bad_loans = df_features[df_features['is_bad_loan'] == 1]['fico_avg'].dropna()

t_stat, p_value = stats.ttest_ind(good_loans, bad_loans)
print(f"FICO T-test: t-statistic = {t_stat:.2f}, p-value = {p_value:.4f}")

# Chi-square test: Grade vs Default
contingency_table = pd.crosstab(df_features['grade'], df_features['is_bad_loan'])
chi2, p_value, dof, expected = stats.chi2_contingency(contingency_table)
print(f"Grade Chi-square: statistic = {chi2:.2f}, p-value = {p_value:.4f}")
```

#### **Hour 5-6: KPI Development & ML Preparation**
```python
# 2.5 KPIs และเตรียมข้อมูลสำหรับ ML
business_kpis = {
    'total_loans': len(df_features),
    'total_amount': df_features['loan_amnt'].sum(),
    'avg_loan_size': df_features['loan_amnt'].mean(),
    'overall_default_rate': df_features['is_bad_loan'].mean()
}

print("=== BUSINESS KPIs ===")
for kpi, value in business_kpis.items():
    print(f"{kpi}: {value:,.2f}")

# Prepare ML dataset
ml_features = [
    'loan_amnt', 'int_rate', 'annual_inc', 'fico_avg',
    'grade_numeric', 'income_to_loan_ratio'
]

X = df_features[ml_features].fillna(df_features[ml_features].median())
y = df_features['is_bad_loan']
```

### 📝 **Day 2 Deliverables**
1. **Data Quality Report**: รายงานคุณภาพข้อมูลและการแก้ไข
2. **Feature Engineering Summary**: รายการ features ใหม่และเหตุผล
3. **Statistical Analysis Report**: ผลการทดสอบทางสถิติ
4. **Clean Dataset**: ข้อมูลที่พร้อมสำหรับ ML

---

## 🤖 วันที่ 3: Predictive Analytics & ML
*เน้นการสร้างโมเดล Machine Learning และการประยุกต์ใช้*

### 🎯 **Learning Objectives**
- สร้างและประเมินโมเดล classification สำหรับ credit risk
- ตีความผลลัพธ์และสร้าง business recommendations
- พัฒนาระบบ scoring และ decision framework

### 📊 **Morning Session (3 hours)**

#### **Hour 1: Model Building**
```python
# 3.1 สร้างโมเดล Credit Risk
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import roc_auc_score, classification_report

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

print(f"Model AUC Score: {auc_score:.3f}")
```

#### **Hour 2: Feature Importance & Model Interpretation**
```python
# 3.2 วิเคราะห์ Feature Importance
feature_importance = pd.DataFrame({
    'feature': ml_features,
    'importance': rf_model.feature_importances_
}).sort_values('importance', ascending=False)

print("=== TOP RISK FACTORS ===")
print(feature_importance.head())
```

#### **Hour 3: Business Application**
```python
# 3.3 สร้างระบบ Credit Scoring
def create_scoring_system(model, threshold=0.25):
    def score_application(application_data):
        features = pd.DataFrame([application_data])[ml_features]
        risk_probability = model.predict_proba(features)[0, 1]
        
        if risk_probability <= 0.15:
            decision = "APPROVE"
            rate_tier = "Best"
        elif risk_probability <= 0.25:
            decision = "APPROVE"
            rate_tier = "Standard"
        else:
            decision = "DECLINE"
            rate_tier = "N/A"
        
        return {
            'risk_score': int(risk_probability * 1000),
            'decision': decision,
            'rate_tier': rate_tier
        }
    
    return score_application

scoring_function = create_scoring_system(rf_model)
```

### 🕐 **Afternoon Session (3 hours)**

#### **Hour 4-6: Customer Segmentation & Implementation**
```python
# 3.4 Customer Segmentation
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

# Prepare customer features
customer_features = df_features.groupby('member_id').agg({
    'loan_amnt': 'sum',
    'annual_inc': 'first',
    'fico_avg': 'first',
    'is_bad_loan': 'max'
}).fillna(0)

# Clustering
scaler = StandardScaler()
X_cluster = scaler.fit_transform(customer_features)

kmeans = KMeans(n_clusters=4, random_state=42)
customer_features['segment'] = kmeans.fit_predict(X_cluster)

print("=== CUSTOMER SEGMENTS ===")
print(customer_features.groupby('segment').mean())
```

### 📝 **Day 3 Deliverables**
1. **Trained ML Model**: โมเดลที่ผ่านการ optimize แล้ว
2. **Scoring System**: ระบบให้คะแนนลูกค้าใหม่
3. **Customer Segmentation**: การแบ่งกลุ่มลูกค้าและกลยุทธ์
4. **Implementation Guide**: คู่มือการนำไปใช้จริง

---

## 🔗 การเชื่อมโยงระหว่างวัน

### 📈 **Progressive Learning Path**

**Day 1 → Day 2**:
- ใช้ insights จาก descriptive analysis เพื่อกำหนดทิศทางการทำความสะอาดข้อมูล
- Business understanding นำไปสู่การสร้าง relevant features
- Data quality issues ที่พบในวันแรกจะถูกแก้ไขในวันที่สอง

**Day 2 → Day 3**:
- Clean dataset และ engineered features จะใช้ในการสร้างโมเดล
- Statistical insights ช่วยในการเลือก features และตีความผลลัพธ์
- KPIs ที่พัฒนาจะใช้ในการ evaluate business impact

### 🎯 **Skill Building Progression**

| Day | Technical Skills | Business Skills | Tools |
|-----|-----------------|----------------|-------|
| 1 | pandas, matplotlib, basic stats | Domain understanding, data interpretation | Jupyter, pandas |
| 2 | scipy, feature engineering, hypothesis testing | KPI development, statistical thinking | pandas, scipy, seaborn |
| 3 | scikit-learn, model evaluation | Business impact assessment, decision making | sklearn, deployment tools |

---

## 💡 Tips สำหรับผู้สอน

### 📚 **Teaching Strategies**

#### **สำหรับ Day 1**
- **Start with Business**: เริ่มจาก business problem ก่อนเข้าสู่ technical details
- **Interactive Exploration**: ให้นักเรียนสำรวจข้อมูลเองแล้วแชร์ findings
- **Visual First**: เน้นการใช้ visualizations เพื่อสื่อสาร insights
- **Context Building**: เล่าเรื่องของ Lending Club ให้น่าสนใจ

#### **สำหรับ Day 2**
- **Systematic Approach**: สอนแนวทางการทำความสะอาดข้อมูลอย่างเป็นระบบ
- **Statistical Thinking**: เน้นการตีความและการใช้งานมากกว่าสูตร
- **Business Relevance**: เชื่อมโยง features ทุกตัวกับ business logic
- **Quality Focus**: สร้างนิสัยการตรวจสอบคุณภาพข้อมูล

#### **สำหรับ Day 3**
- **Model Understanding**: เน้นการเข้าใจโมเดลมากกว่าการใช้งาน
- **Business Translation**: ฝึกการแปลผลลัพธ์ technical เป็นภาษาธุรกิจ
- **Implementation Reality**: พูดถึงความท้าทายในการนำไปใช้จริง
- **Ethical Considerations**: หารือเรื่อง bias และ fairness

### 🔧 **Technical Preparations**

#### **Environment Setup**
```python
# Required packages
packages = [
    'pandas>=1.3.0',
    'numpy>=1.21.0', 
    'matplotlib>=3.4.0',
    'seaborn>=0.11.0',
    'scikit-learn>=1.0.0',
    'scipy>=1.7.0'
]
```

#### **Common Issues & Solutions**

**Memory Issues**:
```python
# For large datasets
chunk_size = 10000
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    process_chunk(chunk)
```

**Data Loading Problems**:
```python
# Robust data loading
try:
    df = pd.read_csv('data.csv', encoding='utf-8')
except UnicodeDecodeError:
    df = pd.read_csv('data.csv', encoding='latin-1')
```

### 🎯 **Assessment Methods**

#### **Day 1 Assessment**
- **Practical Exercise** (30 minutes): Data exploration challenge
- **Mini-Presentation** (10 minutes): Present one key insight

#### **Day 2 Assessment**
- **Data Cleaning Challenge** (45 minutes): Clean a messy dataset
- **Statistical Analysis** (30 minutes): Perform and interpret tests

#### **Day 3 Assessment**
- **Model Building Project** (60 minutes): Build and evaluate ML model
- **Business Case** (30 minutes): Translate model results to business recommendations

### 📊 **Engagement Techniques**

1. **Real-world Relevance**: เชื่อมโยงกับข่าวการเงินปัจจุบัน
2. **Interactive Polling**: ใช้ polls เพื่อรวบรวม predictions ก่อนดูผลจริง
3. **Group Competitions**: แข่งกันหา insights หรือ build model ที่ดีที่สุด
4. **Case Study Discussions**: อภิปรายผลกระทบต่อธุรกิจ
5. **Error Learning**: ใช้ common mistakes เป็น teaching moments

### ✅ **Learning Objectives Checklist**

**End of Day 1**:
- [ ] สามารถ load และ explore ข้อมูลได้
- [ ] สร้าง basic visualizations
- [ ] เข้าใจ business context
- [ ] ระบุ patterns เบื้องต้น

**End of Day 2**:
- [ ] ทำความสะอาดข้อมูลได้อย่างมีระบบ
- [ ] สร้าง derived features
- [ ] ใช้ statistical tests อย่างถูกต้อง
- [ ] พัฒนา business KPIs

**End of Day 3**:
- [ ] สร้างและประเมิน ML models
- [ ] ตีความผลลัพธ์เชิงธุรกิจ
- [ ] สร้างระบบ scoring
- [ ] วางแผนการ implement

---

*📖 หมายเหตุ: คู่มือการประยุกต์ใช้นี้ออกแบบมาให้มีความยืดหยุ่น สามารถปรับเนื้อหาและระดับความยากได้ตามความสามารถของผู้เรียนและเวลาที่มีอยู่*

---
[⬅️ กลับไปที่ Case Studies](./06_case_studies.md) | [➡️ ไปยัง Exercises](./08_exercises.md)