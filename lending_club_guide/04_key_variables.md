# 🔍 ตัวแปรสำคัญและความหมาย

## 📋 สารบัญ
- [🎯 ตัวแปรหลักสำคัญ](#-ตัวแปรหลักสำคัญ)
- [👤 ข้อมูลผู้กู้ (Borrower Profile)](#-ข้อมูลผู้กู้-borrower-profile)
- [💰 ข้อมูลสินเชื่อ (Loan Details)](#-ข้อมูลสินเชื่อ-loan-details)
- [💳 ประวัติเครดิต (Credit History)](#-ประวัติเครดิต-credit-history)
- [📊 ข้อมูลการชำระ (Payment Information)](#-ข้อมูลการชำระ-payment-information)
- [📈 ตัวแปรสำหรับการวิเคราะห์](#-ตัวแปรสำหรับการวิเคราะห์)
- [⚠️ ข้อควรระวังในการใช้งาน](#️-ข้อควรระวังในการใช้งาน)

---

## 🎯 ตัวแปรหลักสำคัญ

### 🎪 **Target Variables (ตัวแปรเป้าหมาย)**

| ตัวแปร | ประเภท | ความหมาย | ค่าที่เป็นไปได้ |
|--------|--------|-----------|----------------|
| **`loan_status`** | Categorical | สถานะปัจจุบันของสินเชื่อ | Fully Paid, Charged Off, Current, Late (31-120 days), In Grace Period, Late (16-30 days), Default |

#### **การแปลงสำหรับ Binary Classification**
```python
# แปลง loan_status เป็น binary
def create_binary_target(loan_status):
    good_status = ['Fully Paid', 'Current']
    bad_status = ['Charged Off', 'Default', 'Late (31-120 days)', 'Late (16-30 days)']
    
    if loan_status in good_status:
        return 0  # Good loan
    elif loan_status in bad_status:
        return 1  # Bad loan
    else:
        return None  # Exclude from analysis

# การใช้งาน
df['is_bad_loan'] = df['loan_status'].apply(create_binary_target)
```

### 🔑 **Key Predictors (ตัวแปรพยากรณ์หลัก)**

| ตัวแปร | ความสำคัญ | Business Impact |
|--------|-----------|-----------------|
| `grade` / `sub_grade` | ⭐⭐⭐⭐⭐ | การจัดเกรดโดย Lending Club |
| `fico_range_low` / `fico_range_high` | ⭐⭐⭐⭐⭐ | คะแนนเครดิตมาตรฐาน |
| `dti` | ⭐⭐⭐⭐ | อัตราส่วนหนี้ต่อรายได้ |
| `annual_inc` | ⭐⭐⭐⭐ | ความสามารถในการชำระ |
| `revol_util` | ⭐⭐⭐⭐ | การใช้วงเงินเครดิต |
| `delinq_2yrs` | ⭐⭐⭐⭐ | ประวัติการผิดนัดชำระ |
| `inq_last_6mths` | ⭐⭐⭐ | การสอบถามเครดิตล่าสุด |
| `open_acc` | ⭐⭐⭐ | จำนวนบัญชีเครดิตที่เปิดอยู่ |

---

## 👤 ข้อมูลผู้กู้ (Borrower Profile)

### 💼 **ข้อมูลการทำงาน (Employment Information)**

| ตัวแปร | ประเภท | ความหมาย | ตัวอย่าง | การใช้งาน |
|--------|--------|-----------|----------|-----------|
| `emp_title` | Text | ตำแหน่งงาน | "Teacher", "Manager", "Engineer" | Text Analysis, Job Category |
| `emp_length` | Categorical | ระยะเวลาการทำงาน | "< 1 year", "2 years", "10+ years" | Stability Indicator |
| `annual_inc` | Numeric | รายได้ต่อปี ($) | 65000.0 | Payment Capacity |
| `verification_status` | Categorical | สถานะการตรวจสอบรายได้ | "Verified", "Source Verified", "Not Verified" | Data Quality |

#### **การประมวลผล Employment Length**
```python
# แปลง emp_length เป็นตัวเลข
def process_emp_length(emp_length):
    if pd.isna(emp_length) or emp_length == 'n/a':
        return 0
    elif '< 1' in str(emp_length):
        return 0.5
    elif '10+' in str(emp_length):
        return 10
    else:
        return float(str(emp_length).split()[0])

df['emp_length_numeric'] = df['emp_length'].apply(process_emp_length)
```

### 🏠 **ข้อมูลที่อยู่อาศัย (Housing Information)**

| ตัวแปร | ประเภท | ความหมาย | ค่าที่เป็นไปได้ | ความหมายทางธุรกิจ |
|--------|--------|-----------|----------------|-------------------|
| `home_ownership` | Categorical | สถานะที่อยู่อาศัย | RENT, OWN, MORTGAGE, OTHER | Financial Stability |
| `addr_state` | Categorical | รัฐที่อยู่อาศัย | CA, NY, TX, FL, etc. | Geographic Risk |
| `zip_code` | Categorical | รหัสไปรษณีย์ (3 หลักแรก) | 481xx, 100xx | Regional Analysis |

#### **การวิเคราะห์ Geographic Risk**
```python
# สร้าง risk score ตามรัฐ
state_default_rates = df.groupby('addr_state')['is_bad_loan'].mean().sort_values(ascending=False)

# กำหนดระดับความเสี่ยงตามรัฐ
def assign_state_risk(state):
    if state in state_default_rates.head(10).index:
        return 'High Risk'
    elif state in state_default_rates.tail(10).index:
        return 'Low Risk'
    else:
        return 'Medium Risk'

df['state_risk_level'] = df['addr_state'].apply(assign_state_risk)
```

---

## 💰 ข้อมูลสินเชื่อ (Loan Details)

### 📊 **ข้อมูลพื้นฐานสินเชื่อ**

| ตัวแปร | ประเภท | ความหมาย | ช่วงค่า | การใช้ในโมเดล |
|--------|--------|-----------|---------|---------------|
| `loan_amnt` | Numeric | จำนวนเงินกู้ที่ขอ ($) | 1,000 - 40,000 | Loan Size Risk |
| `funded_amnt` | Numeric | จำนวนเงินที่ได้รับจริง ($) | ≤ loan_amnt | Market Demand |
| `funded_amnt_inv` | Numeric | จำนวนเงินจากนักลงทุน ($) | ≤ funded_amnt | Investor Appetite |
| `int_rate` | Numeric | อัตราดอกเบี้ย (%) | 5.32 - 28.99 | Risk Pricing |
| `installment` | Numeric | ยอดผ่อนรายเดือน ($) | คำนวณจาก loan_amnt + int_rate | Payment Burden |

#### **การคำนวณตัวแปรเพิ่มเติม**
```python
# Payment to Income Ratio
df['payment_to_income'] = (df['installment'] * 12) / df['annual_inc']

# Funding Ratio
df['funding_ratio'] = df['funded_amnt'] / df['loan_amnt']

# Loan Amount Categories
def categorize_loan_amount(amount):
    if amount < 5000:
        return 'Small'
    elif amount < 15000:
        return 'Medium'
    elif amount < 25000:
        return 'Large'
    else:
        return 'Very Large'

df['loan_size_category'] = df['loan_amnt'].apply(categorize_loan_amount)
```

### 🎯 **ข้อมูลเกรดและระยะเวลา**

| ตัวแปร | ประเภท | ความหมาย | ค่าที่เป็นไปได้ | ความสำคัญ |
|--------|--------|-----------|----------------|-----------|
| `grade` | Categorical | เกรดสินเชื่อหลัก | A, B, C, D, E, F, G | ⭐⭐⭐⭐⭐ |
| `sub_grade` | Categorical | เกรดสินเชื่อย่อย | A1-A5, B1-B5, ..., G1-G5 | ⭐⭐⭐⭐⭐ |
| `term` | Categorical | ระยะเวลาผ่อนชำระ | "36 months", "60 months" | ⭐⭐⭐⭐ |

#### **Grade Risk Analysis**
```python
# สร้าง Grade Risk Score
grade_mapping = {'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7}
df['grade_numeric'] = df['grade'].map(grade_mapping)

# Sub-grade Risk Score
def subgrade_to_numeric(subgrade):
    if pd.isna(subgrade):
        return None
    grade_map = {'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7}
    grade = subgrade[0]
    number = int(subgrade[1])
    return grade_map[grade] + (number - 1) * 0.2

df['subgrade_numeric'] = df['sub_grade'].apply(subgrade_to_numeric)
```

### 🎪 **วัตถุประสงค์การกู้**

| ตัวแปร | ประเภท | ความหมาย | การกระจาย | Risk Level |
|--------|--------|-----------|-----------|-----------|
| `purpose` | Categorical | วัตถุประสงค์การกู้ | debt_consolidation (59%), credit_card (18%) | แตกต่างกันตามประเภท |
| `title` | Text | หัวข้อสินเชื่อ | Free text description | Text Analysis |

#### **Purpose Risk Analysis**
```python
# วิเคราะห์ความเสี่ยงตามวัตถุประสงค์
purpose_risk = df.groupby('purpose').agg({
    'is_bad_loan': ['mean', 'count'],
    'int_rate': 'mean',
    'loan_amnt': 'mean'
}).round(3)

# จัดกลุ่มความเสี่ยงตามวัตถุประสงค์
high_risk_purposes = ['small_business', 'renewable_energy', 'educational']
medium_risk_purposes = ['debt_consolidation', 'credit_card', 'home_improvement']
low_risk_purposes = ['car', 'major_purchase', 'medical']

def categorize_purpose_risk(purpose):
    if purpose in high_risk_purposes:
        return 'High Risk'
    elif purpose in medium_risk_purposes:
        return 'Medium Risk'
    else:
        return 'Low Risk'

df['purpose_risk_category'] = df['purpose'].apply(categorize_purpose_risk)
```

---

## 💳 ประวัติเครดิต (Credit History)

### 📊 **คะแนนเครดิต (Credit Score)**

| ตัวแปร | ประเภท | ความหมาย | ช่วงค่า | ความสำคัญ |
|--------|--------|-----------|---------|-----------|
| `fico_range_low` | Numeric | FICO Score ขั้นต่ำ | 580-850 | ⭐⭐⭐⭐⭐ |
| `fico_range_high` | Numeric | FICO Score สูงสุด | 584-850 | ⭐⭐⭐⭐⭐ |
| `last_fico_range_low` | Numeric | FICO Score ล่าสุด (ขั้นต่ำ) | Updated score | ⭐⭐⭐⭐ |
| `last_fico_range_high` | Numeric | FICO Score ล่าสุด (สูงสุด) | Updated score | ⭐⭐⭐⭐ |

#### **FICO Score Processing**
```python
# สร้าง FICO Score เฉลี่ย
df['fico_score_avg'] = (df['fico_range_low'] + df['fico_range_high']) / 2
df['last_fico_score_avg'] = (df['last_fico_range_low'] + df['last_fico_range_high']) / 2

# FICO Score Categories
def categorize_fico(score):
    if score >= 740:
        return 'Excellent'
    elif score >= 680:
        return 'Good'
    elif score >= 620:
        return 'Fair'
    else:
        return 'Poor'

df['fico_category'] = df['fico_score_avg'].apply(categorize_fico)

# FICO Score Change
df['fico_change'] = df['last_fico_score_avg'] - df['fico_score_avg']
```

### 🔍 **ประวัติการผิดนัดชำระ (Delinquency History)**

| ตัวแปร | ประเภท | ความหมาย | การตีความ | Risk Impact |
|--------|--------|-----------|-----------|-------------|
| `delinq_2yrs` | Numeric | จำนวนครั้งผิดนัดใน 2 ปี | 0 = ดี, >0 = มีปัญหา | ⭐⭐⭐⭐⭐ |
| `delinq_amnt` | Numeric | จำนวนเงินที่ค้างชำระ | $0+ | ⭐⭐⭐⭐ |
| `mths_since_last_delinq` | Numeric | เดือนนับจากการผิดนัดครั้งล่าสุด | มากกว่า = ดีกว่า | ⭐⭐⭐⭐ |
| `pub_rec` | Numeric | จำนวนบันทึกสาธารณะ | 0 = ดี | ⭐⭐⭐⭐ |
| `pub_rec_bankruptcies` | Numeric | จำนวนการล้มละลาย | 0 = ดี | ⭐⭐⭐⭐⭐ |

#### **Delinquency Risk Score**
```python
# สร้าง Delinquency Risk Score
def calculate_delinq_score(row):
    score = 0
    
    # เพิ่มคะแนนจากการผิดนัดใน 2 ปี
    score += row['delinq_2yrs'] * 20
    
    # เพิ่มคะแนนจากบันทึกสาธารณะ
    score += row['pub_rec'] * 15
    
    # เพิ่มคะแนนจากการล้มละลาย
    score += row['pub_rec_bankruptcies'] * 50
    
    # ลดคะแนนถ้าผิดนัดนานแล้ว
    if not pd.isna(row['mths_since_last_delinq']):
        if row['mths_since_last_delinq'] > 24:
            score *= 0.5
        elif row['mths_since_last_delinq'] > 12:
            score *= 0.7
    
    return score

df['delinquency_risk_score'] = df.apply(calculate_delinq_score, axis=1)
```

### 💰 **การใช้เครดิต (Credit Utilization)**

| ตัวแปร | ประเภท | ความหมาย | ช่วงค่าดี | ความสำคัญ |
|--------|--------|-----------|-----------|-----------|
| `revol_bal` | Numeric | ยอดหนี้หมุนเวียนคงเหลือ ($) | ต่ำกว่าดี | ⭐⭐⭐ |
| `revol_util` | Numeric | อัตราการใช้วงเงินหมุนเวียน (%) | < 30% | ⭐⭐⭐⭐ |
| `total_acc` | Numeric | จำนวนบัญชีเครดิตทั้งหมด | 10-20 | ⭐⭐⭐ |
| `open_acc` | Numeric | จำนวนบัญชีเครดิตที่เปิดอยู่ | สมดุล | ⭐⭐⭐ |

#### **Credit Utilization Analysis**
```python
# Credit Utilization Categories
def categorize_revol_util(util):
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

df['revol_util_category'] = df['revol_util'].apply(categorize_revol_util)

# Credit Account Diversity
df['account_diversity'] = df['total_acc'] - df['open_acc']  # Closed accounts
df['account_utilization'] = df['open_acc'] / df['total_acc']  # Active ratio
```

### 🔍 **การสอบถามเครดิต (Credit Inquiries)**

| ตัวแปร | ประเภท | ความหมาย | การตีความ | Risk Signal |
|--------|--------|-----------|-----------|-------------|
| `inq_last_6mths` | Numeric | การสอบถามเครดิตใน 6 เดือน | > 2 = มีความเสี่ยง | ⭐⭐⭐⭐ |
| `inq_last_12m` | Numeric | การสอบถามเครดิตใน 12 เดือน | > 4 = มีความเสี่ยง | ⭐⭐⭐ |
| `mths_since_recent_inq` | Numeric | เดือนนับจากการสอบถามล่าสุด | มากกว่าดีกว่า | ⭐⭐⭐ |

#### **Inquiry Risk Analysis**
```python
# Credit Shopping Behavior
def analyze_inquiry_pattern(row):
    recent_inq = row['inq_last_6mths']
    total_inq = row['inq_last_12m'] if not pd.isna(row['inq_last_12m']) else recent_inq
    
    if recent_inq == 0:
        return 'No Recent Activity'
    elif recent_inq <= 2 and total_inq <= 4:
        return 'Normal Shopping'
    elif recent_inq <= 4 and total_inq <= 8:
        return 'Active Shopping'
    else:
        return 'Credit Seeking'

df['inquiry_pattern'] = df.apply(analyze_inquiry_pattern, axis=1)
```

---

## 📊 ข้อมูลการชำระ (Payment Information)

### 💰 **ข้อมูลการชำระและคงเหลือ**

| ตัวแปร | ประเภท | ความหมาย | การใช้งาน | สำหรับ Analysis |
|--------|--------|-----------|-----------|----------------|
| `out_prncp` | Numeric | เงินต้นคงเหลือ ($) | Portfolio Management | Current Performance |
| `out_prncp_inv` | Numeric | เงินต้นคงเหลือ (นักลงทุน) ($) | Investor Returns | Current Performance |
| `total_pymnt` | Numeric | ยอดชำระรวม ($) | ROI Calculation | Historical Performance |
| `total_pymnt_inv` | Numeric | ยอดชำระรวม (นักลงทุน) ($) | Investor Returns | Historical Performance |
| `total_rec_prncp` | Numeric | เงินต้นที่ได้รับคืน ($) | Principal Recovery | Historical Performance |
| `total_rec_int` | Numeric | ดอกเบี้ยที่ได้รับ ($) | Interest Income | Historical Performance |

#### **Performance Metrics Calculation**
```python
# ROI สำหรับนักลงทุน
def calculate_investor_roi(row):
    if row['funded_amnt_inv'] > 0:
        total_return = row['total_pymnt_inv'] + row['recoveries']
        roi = (total_return - row['funded_amnt_inv']) / row['funded_amnt_inv']
        return roi
    return None

df['investor_roi'] = df.apply(calculate_investor_roi, axis=1)

# Payment Progress
df['payment_progress'] = (df['total_rec_prncp'] / df['funded_amnt']).fillna(0)

# Interest vs Principal
df['interest_ratio'] = df['total_rec_int'] / (df['total_rec_int'] + df['total_rec_prncp'])
```

### 📅 **ข้อมูลวันที่สำคัญ**

| ตัวแปร | ประเภท | ความหมาย | การใช้งาน | Derived Features |
|--------|--------|-----------|-----------|------------------|
| `issue_d` | Date | วันที่ออกสินเชื่อ | Cohort Analysis | Loan Age |
| `earliest_cr_line` | Date | วันที่เปิดเครดิตครั้งแรก | Credit History Length | Credit Age |
| `last_pymnt_d` | Date | วันที่ชำระครั้งสุดท้าย | Current Status | Days Since Payment |
| `last_credit_pull_d` | Date | วันที่ดึงเครดิตล่าสุด | Data Freshness | Monitoring |

#### **Date-based Feature Engineering**
```python
# แปลงวันที่
date_columns = ['issue_d', 'earliest_cr_line', 'last_pymnt_d']
for col in date_columns:
    df[col] = pd.to_datetime(df[col], errors='coerce')

# Credit History Length
df['credit_history_length'] = (df['issue_d'] - df['earliest_cr_line']).dt.days / 365.25

# Loan Age (สำหรับ current loans)
reference_date = pd.to_datetime('2015-01-01')  # Reference date
df['loan_age_months'] = ((reference_date - df['issue_d']).dt.days / 30.44).round()

# Seasonality
df['issue_month'] = df['issue_d'].dt.month
df['issue_quarter'] = df['issue_d'].dt.quarter
df['issue_year'] = df['issue_d'].dt.year
```

---

## 📈 ตัวแปรสำหรับการวิเคราะห์

### 🎯 **Feature Engineering สำหรับ ML Models**

#### **1. Risk Ratios**
```python
# Debt-to-Income Ratio
df['dti_cleaned'] = pd.to_numeric(df['dti'], errors='coerce')

# Payment-to-Income Ratio
df['payment_to_income'] = (df['installment'] * 12) / df['annual_inc']

# Total Debt Ratio
df['total_debt_ratio'] = (df['dti_cleaned'] * df['annual_inc'] / 100 + df['installment'] * 12) / df['annual_inc']
```

#### **2. Credit Health Scores**
```python
# Credit Health Score (0-100)
def calculate_credit_health(row):
    score = 50  # Base score
    
    # FICO Score contribution (40%)
    if not pd.isna(row['fico_score_avg']):
        fico_normalized = (row['fico_score_avg'] - 300) / (850 - 300)
        score += fico_normalized * 40
    
    # Utilization contribution (25%)
    if not pd.isna(row['revol_util']):
        util_score = max(0, (100 - row['revol_util']) / 100) * 25
        score += util_score
    
    # Delinquency contribution (-30%)
    delinq_penalty = row['delinq_2yrs'] * 10
    score -= delinq_penalty
    
    # Public records penalty
    pub_rec_penalty = row['pub_rec'] * 15
    score -= pub_rec_penalty
    
    return max(0, min(100, score))

df['credit_health_score'] = df.apply(calculate_credit_health, axis=1)
```

#### **3. Behavioral Indicators**
```python
# Credit Seeking Behavior
df['credit_seeking_intensity'] = (
    df['inq_last_6mths'].fillna(0) * 2 + 
    df['acc_open_past_24mths'].fillna(0)
)

# Financial Stability
df['employment_stability'] = df['emp_length_numeric'] * df['annual_inc'] / 1000

# Debt Management
df['debt_management_score'] = (
    100 - df['revol_util'].fillna(50) - 
    df['delinq_2yrs'].fillna(0) * 20 - 
    df['pub_rec'].fillna(0) * 30
)
```

### 🎨 **Categorical Encoding Strategies**

#### **1. Target Encoding**
```python
def target_encode(df, categorical_col, target_col, smoothing=10):
    # คำนวณ global mean
    global_mean = df[target_col].mean()
    
    # คำนวณ statistics per category
    category_stats = df.groupby(categorical_col)[target_col].agg(['mean', 'count'])
    
    # Apply smoothing
    category_stats['smoothed_mean'] = (
        (category_stats['mean'] * category_stats['count'] + global_mean * smoothing) /
        (category_stats['count'] + smoothing)
    )
    
    return category_stats['smoothed_mean'].to_dict()

# ใช้กับตัวแปรหลัก
target_encoding_maps = {}
for col in ['purpose', 'addr_state', 'home_ownership']:
    target_encoding_maps[col] = target_encode(df, col, 'is_bad_loan')
    df[f'{col}_target_encoded'] = df[col].map(target_encoding_maps[col])
```

#### **2. Frequency Encoding**
```python
# Frequency encoding สำหรับ high cardinality categories
for col in ['emp_title', 'zip_code']:
    freq_map = df[col].value_counts().to_dict()
    df[f'{col}_frequency'] = df[col].map(freq_map)
```

---

## ⚠️ ข้อควรระวังในการใช้งาน

### 🚨 **Data Leakage Issues**

#### **1. Future Information**
```python
# ตัวแปรที่มี future information (ห้ามใช้ในการพยากรณ์)
future_leakage_vars = [
    'total_pymnt', 'total_pymnt_inv', 'total_rec_prncp', 'total_rec_int',
    'total_rec_late_fee', 'recoveries', 'collection_recovery_fee',
    'last_pymnt_d', 'last_pymnt_amnt', 'next_pymnt_d',
    'last_fico_range_low', 'last_fico_range_high'
]

# ตัวแปรที่ปลอดภัยสำหรับการพยากรณ์
safe_vars = [
    'loan_amnt', 'term', 'int_rate', 'installment', 'grade', 'sub_grade',
    'emp_title', 'emp_length', 'home_ownership', 'annual_inc', 'verification_status',
    'purpose', 'dti', 'delinq_2yrs', 'fico_range_low', 'fico_range_high',
    'inq_last_6mths', 'open_acc', 'pub_rec', 'revol_bal', 'revol_util', 'total_acc'
]
```

#### **2. Target Leakage**
```python
# ตัวแปรที่อาจมี target leakage
potential_leakage = [
    'funded_amnt_inv',  # อาจสะท้อนความต้องการของนักลงทุน
    'initial_list_status',  # อาจเกี่ยวข้องกับ loan quality
]

# ตรวจสอบ correlation กับ target
for var in potential_leakage:
    if var in df.columns:
        corr = df[var].corr(df['is_bad_loan'])
        print(f"{var}: correlation = {corr:.3f}")
```

### 📊 **Missing Data Strategies**

#### **1. Missing Data Analysis**
```python
def analyze_missing_data(df):
    missing_info = pd.DataFrame({
        'Variable': df.columns,
        'Missing_Count': df.isnull().sum(),
        'Missing_Percentage': (df.isnull().sum() / len(df)) * 100,
        'Data_Type': df.dtypes
    })
    
    missing_info = missing_info[missing_info['Missing_Count'] > 0].sort_values(
        'Missing_Percentage', ascending=False
    )
    
    return missing_info

missing_analysis = analyze_missing_data(df)
print(missing_analysis.head(10))
```

#### **2. Imputation Strategies**
```python
# กลยุทธ์การ impute ตามประเภทตัวแปร
imputation_strategies = {
    'credit_related': {
        'variables': ['mths_since_last_delinq', 'mths_since_last_record'],
        'method': 'high_value',  # Missing = ไม่เคยมีปัญหา
        'value': 999
    },
    'financial_ratios': {
        'variables': ['revol_util', 'dti'],
        'method': 'median',
        'group_by': 'grade'
    },
    'employment': {
        'variables': ['emp_length'],
        'method': 'mode',
        'group_by': 'emp_title'
    }
}

# ตัวอย่างการ impute
df['revol_util_imputed'] = df.groupby('grade')['revol_util'].transform(
    lambda x: x.fillna(x.median())
)
```

### 🎯 **Model Selection Considerations**

#### **1. Variable Importance**
```python
# กลุ่มตัวแปรตามความสำคัญ
variable_groups = {
    'tier_1_critical': [
        'grade', 'sub_grade', 'fico_score_avg', 'dti', 'annual_inc'
    ],
    'tier_2_important': [
        'revol_util', 'delinq_2yrs', 'inq_last_6mths', 'emp_length_numeric'
    ],
    'tier_3_useful': [
        'home_ownership', 'purpose', 'open_acc', 'total_acc', 'pub_rec'
    ],
    'tier_4_supplementary': [
        'addr_state', 'credit_history_length', 'loan_size_category'
    ]
}
```

#### **2. Multi-collinearity Check**
```python
# ตรวจสอบ correlation matrix
import seaborn as sns
import matplotlib.pyplot as plt

numeric_vars = df.select_dtypes(include=[np.number]).columns
correlation_matrix = df[numeric_vars].corr()

# หา highly correlated pairs
def find_high_correlation(corr_matrix, threshold=0.8):
    high_corr_pairs = []
    for i in range(len(corr_matrix.columns)):
        for j in range(i+1, len(corr_matrix.columns)):
            if abs(corr_matrix.iloc[i, j]) > threshold:
                high_corr_pairs.append((
                    corr_matrix.columns[i], 
                    corr_matrix.columns[j], 
                    corr_matrix.iloc[i, j]
                ))
    return high_corr_pairs

high_corr = find_high_correlation(correlation_matrix)
for var1, var2, corr in high_corr:
    print(f"{var1} - {var2}: {corr:.3f}")
```

---

*📖 หมายเหตุ: การเลือกและการประมวลผลตัวแปรควรคำนึงถึงวัตถุประสงค์ของการวิเคราะห์และข้อจำกัดของข้อมูล โดยเฉพาะเรื่องการป้องกัน data leakage ในการสร้างโมเดลพยากรณ์*

---
[⬅️ กลับไปที่ Dataset Overview](./03_dataset_overview.md) | [➡️ ไปยัง Business Questions](./05_business_questions.md)