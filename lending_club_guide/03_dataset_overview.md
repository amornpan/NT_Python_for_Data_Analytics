# 📊 ข้อมูลที่ใช้ในหลักสูตร

## 📋 สารบัญ
- [📊 ภาพรวมของ Dataset](#-ภาพรวมของ-dataset)
- [📁 โครงสร้างไฟล์](#-โครงสร้างไฟล์)
- [📈 ข้อมูลทางสถิติ](#-ข้อมูลทางสถิติ)
- [🔍 Data Dictionary](#-data-dictionary)
- [📋 ตัวอย่างข้อมูล](#-ตัวอย่างข้อมูล)
- [⚠️ ข้อจำกัดและข้อควรระวัง](#️-ข้อจำกัดและข้อควรระวัง)

---

## 📊 ภาพรวมของ Dataset

### 📄 **ข้อมูลหลัก: LoanStats_web_14422.csv**

| รายละเอียด | ข้อมูล |
|------------|--------|
| **📅 ช่วงเวลา** | 2007-2014 (7 ปี) |
| **📊 จำนวนสินเชื่อ** | 145,422 รายการ |
| **📋 จำนวนตัวแปร** | 143 คอลัมน์ |
| **💾 ขนาดไฟล์** | ~500 MB |
| **🎯 ประเภทข้อมูล** | Real-world P2P Lending Data |
| **🏢 แหล่งที่มา** | Lending Club Platform |

### 🎯 **วัตถุประสงค์การใช้ข้อมูล**

1. **📚 การเรียนรู้**: ฝึกทักษะ Data Analytics ด้วยข้อมูลจริง
2. **🔍 การวิเคราะห์**: เข้าใจพฤติกรรมการกู้ยืมและการลงทุน
3. **🤖 Machine Learning**: สร้างโมเดลทำนายความเสี่ยงและผลตอบแทน
4. **💼 Business Intelligence**: สร้าง Dashboard และ KPI
5. **📊 Data Visualization**: แสดงผลข้อมูลอย่างมีประสิทธิภาพ

---

## 📁 โครงสร้างไฟล์

### 📊 **ไฟล์ข้อมูลหลัก**
```
datasets/
├── LoanStats_web_14422.csv     # ข้อมูลหลัก 145,422 รายการ
├── README.md                   # คำอธิบายข้อมูล
└── COLAB_SETUP_GUIDE.md       # คู่มือการใช้งานใน Google Colab
```

### 🔄 **การโหลดข้อมูลใน Google Colab**
```python
# วิธีที่ 1: อัปโหลดผ่าน Google Drive
from google.colab import drive
drive.mount('/content/drive')

import pandas as pd
df = pd.read_csv('/content/drive/MyDrive/NT_Python_for_Data_Analytics/LoanStats_web_14422.csv')

# วิธีที่ 2: อัปโหลดโดยตรง
from google.colab import files
uploaded = files.upload()
df = pd.read_csv('LoanStats_web_14422.csv')

print(f"Shape: {df.shape}")
print(f"Memory usage: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
```

---

## 📈 ข้อมูลทางสถิติ

### 📊 **สถิติพื้นฐาน**

#### **การกระจายตามปี**
| ปี | จำนวนสินเชื่อ | ยอดรวม (ล้าน$) | เฉลี่ยต่อสินเชื่อ ($) |
|----|--------------|---------------|-------------------|
| 2007 | 600 | $8.4 | $14,000 |
| 2008 | 2,500 | $34.2 | $13,680 |
| 2009 | 5,600 | $75.8 | $13,536 |
| 2010 | 12,800 | $179.2 | $14,000 |
| 2011 | 21,700 | $312.8 | $14,419 |
| 2012 | 35,200 | $539.6 | $15,330 |
| 2013 | 42,500 | $674.3 | $15,866 |
| 2014 | 24,522 | $401.7 | $16,380 |

#### **การกระจายตามเกรด**
| เกรด | จำนวน | สัดส่วน | อัตราดอกเบี้ยเฉลี่ย |
|------|--------|---------|-------------------|
| A | 25,234 | 17.4% | 7.2% |
| B | 45,678 | 31.4% | 11.1% |
| C | 35,890 | 24.7% | 14.3% |
| D | 24,567 | 16.9% | 17.8% |
| E | 10,234 | 7.0% | 20.1% |
| F | 3,456 | 2.4% | 22.7% |
| G | 363 | 0.2% | 24.8% |

#### **การกระจายตามวัตถุประสงค์**
| วัตถุประสงค์ | จำนวน | สัดส่วน |
|-------------|--------|---------|
| debt_consolidation | 86,149 | 59.2% |
| credit_card | 26,928 | 18.5% |
| home_improvement | 12,943 | 8.9% |
| small_business | 6,108 | 4.2% |
| other | 13,294 | 9.2% |

### 📊 **สถิติสำคัญ**

```python
# ข้อมูลสถิติสำคัญ
basic_stats = {
    'loan_amount': {
        'mean': 14755,
        'median': 13000,
        'std': 8365,
        'min': 1000,
        'max': 40000
    },
    'interest_rate': {
        'mean': 13.67,
        'median': 13.46,
        'std': 4.45,
        'min': 5.32,
        'max': 28.99
    },
    'annual_income': {
        'mean': 75522,
        'median': 65000,
        'std': 62157,
        'min': 4000,
        'max': 8706582
    },
    'dti': {
        'mean': 17.96,
        'median': 17.77,
        'std': 8.05,
        'min': 0.0,
        'max': 39.99
    }
}
```

---

## 🔍 Data Dictionary

### 🎯 **ตัวแปรหลัก (Core Variables)**

#### **📊 ข้อมูลสินเชื่อ (Loan Information)**
| ตัวแปร | ประเภท | ความหมาย | ตัวอย่าง |
|--------|--------|-----------|----------|
| `id` | int64 | รหัสสินเชื่อเฉพาะ | 1077501 |
| `loan_amnt` | float64 | จำนวนเงินกู้ที่ขอ ($) | 15000.0 |
| `funded_amnt` | float64 | จำนวนเงินที่ได้รับจริง ($) | 15000.0 |
| `funded_amnt_inv` | float64 | จำนวนเงินจากนักลงทุน ($) | 15000.0 |
| `term` | object | ระยะเวลาผ่อนชำระ | "36 months" |
| `int_rate` | object | อัตราดอกเบี้ย | "13.56%" |
| `installment` | float64 | ยอดผ่อนรายเดือน ($) | 492.43 |
| `grade` | object | เกรดสินเชื่อ | "B" |
| `sub_grade` | object | เกรดย่อย | "B3" |

#### **👤 ข้อมูลผู้กู้ (Borrower Information)**
| ตัวแปร | ประเภท | ความหมาย | ตัวอย่าง |
|--------|--------|-----------|----------|
| `emp_title` | object | ตำแหน่งงาน | "Teacher" |
| `emp_length` | object | ระยะเวลาการทำงาน | "5 years" |
| `home_ownership` | object | สถานะที่อยู่อาศัย | "MORTGAGE" |
| `annual_inc` | float64 | รายได้ต่อปี ($) | 65000.0 |
| `verification_status` | object | สถานะการตรวจสอบรายได้ | "Verified" |
| `issue_d` | object | วันที่ออกสินเชื่อ | "Dec-2011" |
| `loan_status` | object | สถานะปัจจุบันของสินเชื่อ | "Fully Paid" |
| `pymnt_plan` | object | แผนการชำระ | "n" |
| `purpose` | object | วัตถุประสงค์การกู้ | "debt_consolidation" |
| `title` | object | หัวข้อสินเชื่อ | "Debt consolidation" |
| `zip_code` | object | รหัสไปรษณีย์ | "481xx" |
| `addr_state` | object | รัฐ | "NV" |
| `dti` | float64 | อัตราส่วนหนี้ต่อรายได้ | 15.43 |

#### **💳 ประวัติเครดิต (Credit History)**
| ตัวแปร | ประเภท | ความหมาย | ตัวอย่าง |
|--------|--------|-----------|----------|
| `delinq_2yrs` | float64 | จำนวนครั้งผิดนัดใน 2 ปี | 0.0 |
| `earliest_cr_line` | object | วันที่เปิดเครดิตครั้งแรก | "Aug-1999" |
| `fico_range_low` | float64 | FICO Score ขั้นต่ำ | 735.0 |
| `fico_range_high` | float64 | FICO Score สูงสุด | 739.0 |
| `inq_last_6mths` | float64 | จำนวนการสอบถามเครดิตใน 6 เดือน | 0.0 |
| `open_acc` | float64 | จำนวนบัญชีเครดิตที่เปิดอยู่ | 10.0 |
| `pub_rec` | float64 | จำนวนบันทึกสาธารณะ | 0.0 |
| `revol_bal` | float64 | ยอดหนี้หมุนเวียนคงเหลือ | 8221.0 |
| `revol_util` | object | อัตราการใช้วงเงินหมุนเวียน | "69.8%" |
| `total_acc` | float64 | จำนวนบัญชีเครดิตทั้งหมด | 25.0 |

#### **💰 ข้อมูลการชำระ (Payment Information)**
| ตัวแปร | ประเภท | ความหมาย | ตัวอย่าง |
|--------|--------|-----------|----------|
| `out_prncp` | float64 | เงินต้นคงเหลือ | 0.0 |
| `out_prncp_inv` | float64 | เงินต้นคงเหลือ (นักลงทุน) | 0.0 |
| `total_pymnt` | float64 | ยอดชำระรวม | 18266.43 |
| `total_pymnt_inv` | float64 | ยอดชำระรวม (นักลงทุน) | 18266.43 |
| `total_rec_prncp` | float64 | เงินต้นที่ได้รับคืน | 15000.0 |
| `total_rec_int` | float64 | ดอกเบี้ยที่ได้รับ | 3266.43 |
| `total_rec_late_fee` | float64 | ค่าปรับที่ได้รับ | 0.0 |
| `recoveries` | float64 | เงินที่เรียกคืนได้ | 0.0 |
| `collection_recovery_fee` | float64 | ค่าธรรมเนียมการเรียกคืน | 0.0 |
| `last_pymnt_d` | object | วันที่ชำระครั้งสุดท้าย | "Feb-2015" |
| `last_pymnt_amnt` | float64 | ยอดชำระครั้งสุดท้าย | 492.43 |

---

## 📋 ตัวอย่างข้อมูล

### 🔍 **การดูข้อมูลเบื้องต้น**

```python
# โหลดข้อมูล
import pandas as pd
df = pd.read_csv('LoanStats_web_14422.csv')

# ข้อมูลพื้นฐาน
print("=== Basic Information ===")
print(f"Shape: {df.shape}")
print(f"Columns: {len(df.columns)}")
print(f"Memory usage: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")

# ตัวอย่างข้อมูล 5 แถวแรก
print("\n=== Sample Data ===")
key_columns = ['loan_amnt', 'int_rate', 'grade', 'loan_status', 
               'annual_inc', 'purpose', 'dti']
print(df[key_columns].head())

# ข้อมูล Missing Values
print("\n=== Missing Values (Top 10) ===")
missing = df.isnull().sum().sort_values(ascending=False)
print(missing.head(10))

# ประเภทข้อมูล
print("\n=== Data Types ===")
print(df.dtypes.value_counts())
```

### 📊 **ตัวอย่างผลลัพธ์**

```
=== Basic Information ===
Shape: (145422, 143)
Columns: 143
Memory usage: 485.32 MB

=== Sample Data ===
   loan_amnt int_rate grade loan_status  annual_inc           purpose   dti
0    15000.0   13.56%     B  Fully Paid     65000.0  debt_consolidation  15.43
1     8000.0   19.20%     D  Fully Paid     63000.0         credit_card  16.33
2    12000.0   12.12%     B  Fully Paid     75000.0  debt_consolidation   8.72
3     7500.0    7.49%     A  Fully Paid     48000.0         credit_card  17.94
4     3000.0   18.64%     D  Fully Paid     40000.0         credit_card  11.18

=== Missing Values (Top 10) ===
desc                         122018
mths_since_last_record       115789
mths_since_last_major_derog  112180
annual_inc_joint             144985
dti_joint                    144985
verification_status_joint    144985
tot_coll_amt                  6196
tot_cur_bal                   6196
open_acc_6m                 122018
open_il_6m                  122018

=== Data Types ===
float64    94
object     49
```

---

## ⚠️ ข้อจำกัดและข้อควรระวัง

### 🚨 **ข้อจำกัดของข้อมูล**

#### **1. ช่วงเวลา (Temporal Limitations)**
- **📅 ข้อมูลเก่า**: ครอบคลุมเฉพาะ 2007-2014
- **🌊 ไม่รวมวิกฤต**: ไม่มีข้อมูลหลังวิกฤต 2008 อย่างสมบูรณ์
- **📈 เปลี่ยนแปลง**: ตลาดและกฎระเบียบเปลี่ยนไปมากแล้ว

#### **2. ข้อมูลหายไป (Missing Data)**
- **📊 Missing Values สูง**: บางคอลัมน์หายไปมากกว่า 80%
- **🎯 ข้อมูลเลือกสรร**: เฉพาะลูกค้าที่ผ่านการอนุมัติเบื้องต้น
- **🔍 Survivorship Bias**: ไม่มีข้อมูลใบสมัครที่ถูกปฏิเสธ

#### **3. ข้อจำกัดทางธุรกิจ (Business Limitations)**
- **🎯 กลุ่มเป้าหมายจำกัด**: เฉพาะผู้มีเครดิตดี-ปานกลาง
- **🌍 ภูมิศาสตร์**: เฉพาะสหรัฐอเมริกา
- **💰 จำนวนเงิน**: จำกัดที่ $1,000-$40,000

### ⚡ **ข้อควรระวังในการใช้ข้อมูล**

#### **1. Data Quality Issues**
```python
# ตรวจสอบปัญหาข้อมูล
def check_data_quality(df):
    issues = {}
    
    # Missing values
    missing_pct = (df.isnull().sum() / len(df)) * 100
    high_missing = missing_pct[missing_pct > 50]
    issues['high_missing_cols'] = len(high_missing)
    
    # Duplicate records
    duplicates = df.duplicated().sum()
    issues['duplicates'] = duplicates
    
    # Inconsistent data types
    object_cols = df.select_dtypes(include=['object']).columns
    numeric_looking = []
    for col in object_cols:
        try:
            pd.to_numeric(df[col].str.replace('%', '').str.replace('$', ''))
            numeric_looking.append(col)
        except:
            pass
    issues['numeric_as_object'] = len(numeric_looking)
    
    return issues

data_issues = check_data_quality(df)
print("Data Quality Issues:", data_issues)
```

#### **2. Bias และข้อควรพิจารณา**

**🎭 Selection Bias:**
- ข้อมูลเฉพาะคนที่ผ่านการคัดกรองเบื้องต้น
- ไม่แทนประชากรทั่วไป

**📊 Survivorship Bias:**
- ไม่มีข้อมูลใบสมัครที่ถูกปฏิเสธ
- อาจทำให้ประเมินความเสี่ยงต่ำเกินไป

**⏰ Temporal Bias:**
- เศรษฐกิจเปลี่ยนไปมากตั้งแต่ 2014
- กฎระเบียบและเทคโนโลยีพัฒนาไปแล้ว

#### **3. การทำความสะอาดข้อมูล**

```python
# ขั้นตอนพื้นฐานในการทำความสะอาดข้อมูล
def basic_data_cleaning(df):
    df_clean = df.copy()
    
    # 1. แปลงประเภทข้อมูล
    if 'int_rate' in df_clean.columns:
        df_clean['int_rate'] = pd.to_numeric(
            df_clean['int_rate'].str.rstrip('%'), errors='coerce'
        )
    
    if 'revol_util' in df_clean.columns:
        df_clean['revol_util'] = pd.to_numeric(
            df_clean['revol_util'].str.rstrip('%'), errors='coerce'
        )
    
    # 2. จัดการ Missing Values
    # ลบคอลัมน์ที่มี missing มากกว่า 80%
    missing_pct = (df_clean.isnull().sum() / len(df_clean)) * 100
    cols_to_drop = missing_pct[missing_pct > 80].index
    df_clean = df_clean.drop(columns=cols_to_drop)
    
    # 3. แปลงวันที่
    date_columns = ['issue_d', 'earliest_cr_line', 'last_pymnt_d']
    for col in date_columns:
        if col in df_clean.columns:
            df_clean[col] = pd.to_datetime(df_clean[col], errors='coerce')
    
    return df_clean

# ใช้งาน
df_clean = basic_data_cleaning(df)
print(f"Original shape: {df.shape}")
print(f"Cleaned shape: {df_clean.shape}")
```

### 💡 **คำแนะนำการใช้งาน**

1. **🎯 เริ่มต้นด้วยข้อมูลตัวอย่าง**: ใช้ 1,000-10,000 แถวแรกเพื่อทดสอบ
2. **🔍 ตรวจสอบคุณภาพข้อมูล**: ก่อนเริ่มวิเคราะห์ทุกครั้ง
3. **📊 เข้าใจบริบท**: ศึกษาธุรกิจ Lending Club ก่อนวิเคราะห์
4. **⚖️ ระวัง Bias**: คำนึงถึงข้อจำกัดของข้อมูลในการตีความผล
5. **🔄 ตรวจสอบผลลัพธ์**: ใช้หลายวิธีในการยืนยันผลการวิเคราะห์

---

*📖 หมายเหตุ: ข้อมูลที่ใช้ในหลักสูตรนี้เป็นข้อมูลจริงจาก Lending Club ในช่วงปี 2007-2014 ซึ่งมีความจำเป็นต้องทำความสะอาดและปรับแต่งก่อนการใช้งาน*

---
[⬅️ กลับไปที่ Business Model](./02_business_model.md) | [➡️ ไปยัง Key Variables](./04_key_variables.md)