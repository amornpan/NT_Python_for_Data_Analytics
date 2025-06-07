# ⚠️ ปัญหาข้อมูลที่พบบ่อย

## 🕳️ ปัญหาค่าว่าง (Missing Values)

### 🔍 ประเภทของค่าว่าง
1. **Missing Completely at Random (MCAR)**
   - ค่าว่างเกิดขึ้นแบบสุ่มสมบูรณ์
   - ไม่เกี่ยวข้องกับตัวแปรอื่น
   - **ตัวอย่าง**: เซ็นเซอร์เสียทำให้ไม่มีข้อมูลบางวัน

2. **Missing at Random (MAR)**
   - ค่าว่างสัมพันธ์กับตัวแปรอื่นที่มีในข้อมูล
   - **ตัวอย่าง**: คนรุ่นเก่ามักไม่กรอกข้อมูลอีเมล

3. **Missing Not at Random (MNAR)**
   - ค่าว่างเกี่ยวข้องกับค่าที่หายไปเอง
   - **ตัวอย่าง**: คนที่มีรายได้สูงไม่เปิดเผยรายได้

### 💡 วิธีแก้ไขค่าว่าง
```python
# ลบข้อมูลที่มีค่าว่าง
df.dropna()

# เติมด้วยค่าเฉลี่ย
df['column'].fillna(df['column'].mean())

# เติมด้วย mode
df['column'].fillna(df['column'].mode()[0])

# Forward fill / Backward fill
df['column'].fillna(method='ffill')
```

## 🔄 ปัญหาข้อมูลซ้ำ (Duplicates)

### 🔍 ประเภทของข้อมูลซ้ำ
1. **Exact Duplicates**: แถวที่เหมือนกันทุกคอลัมน์
2. **Partial Duplicates**: บาง key fields เหมือนกัน
3. **Near Duplicates**: เหมือนกันแต่มีความผิดพลาดเล็กน้อย

### 💡 วิธีจัดการข้อมูลซ้ำ
```python
# ตรวจหาและลบ exact duplicates
df.drop_duplicates()

# ตรวจหาตาม key columns
df.drop_duplicates(subset=['loan_id'])

# เก็บแถวแรกหรือแถวสุดท้าย
df.drop_duplicates(keep='first')  # หรือ 'last'
```

## ❌ ปัญหาข้อมูลไม่ถูกต้อง (Invalid Data)

### 🔍 ปัญหาที่พบบ่อยใน Lending Club
1. **อัตราดอกเบี้ยติดลบ**: interest_rate < 0
2. **จำนวนเงินกู้เป็นศูนย์**: loan_amount = 0
3. **รายได้ติดลบ**: annual_income < 0
4. **FICO score นอกช่วง**: fico_score < 300 หรือ > 850
5. **DTI เกิน 100%**: dti > 100

### 💡 วิธีตรวจสอบและแก้ไข
```python
# ตรวจสอบข้อมูลไม่ถูกต้อง
invalid_interest = df[df['interest_rate'] < 0]
invalid_amount = df[df['loan_amount'] <= 0]

# แทนที่ด้วยค่าที่สมเหตุสมผล
df.loc[df['interest_rate'] < 0, 'interest_rate'] = df['interest_rate'].median()

# ลบข้อมูลที่ไม่ถูกต้อง
df = df[df['loan_amount'] > 0]
```

## 🔀 ปัญหาความไม่สอดคล้อง (Inconsistency)

### 🔍 ตัวอย่างความไม่สอดคล้อง
1. **funded_amount > loan_amount**: เงินที่ได้รับมากกว่าที่ขอกู้
2. **Grade ไม่สอดคล้องกับ interest_rate**: Grade A แต่ดอกเบี้ยสูง
3. **รูปแบบข้อมูลไม่เหมือนกัน**: "3 years" vs "3Y" vs "36 months"

### 💡 วิธีแก้ไข
```python
# ตรวจสอบความสอดคล้อง
inconsistent = df[df['funded_amount'] > df['loan_amount']]

# แก้ไขข้อมูลไม่สอดคล้อง
df.loc[df['funded_amount'] > df['loan_amount'], 'funded_amount'] = df['loan_amount']

# ปรับให้เป็นรูปแบบเดียวกัน
df['emp_length'] = df['emp_length'].str.replace('Y', ' years')
```

## 📊 ปัญหา Outliers

### 🔍 ประเภท Outliers
1. **Statistical Outliers**: ค่าที่ผิดปกติทางสถิติ
2. **Business Outliers**: ค่าที่ไม่สมเหตุสมผลทางธุรกิจ
3. **Data Entry Errors**: ข้อผิดพลาดจากการบันทึกข้อมูล

### 💡 วิธีตรวจหา Outliers
```python
# IQR Method
Q1 = df['loan_amount'].quantile(0.25)
Q3 = df['loan_amount'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
outliers = df[(df['loan_amount'] < lower_bound) | (df['loan_amount'] > upper_bound)]

# Z-Score Method
from scipy import stats
z_scores = np.abs(stats.zscore(df['loan_amount']))
outliers = df[z_scores > 3]
```

## 🎯 ปัญหาเฉพาะ Lending Club Data

### ⚠️ ปัญหาที่ต้องระวัง
1. **Grade และ Interest Rate ไม่สอดคล้อง**
   ```python
   # Grade A ควรมีดอกเบี้ยต่ำ
   grade_a_high_rate = df[(df['grade'] == 'A') & (df['interest_rate'] > 15)]
   ```

2. **Employment Length ในรูปแบบต่างกัน**
   ```python
   # มีทั้ง "10+ years", "10Y", "10 yrs"
   df['emp_length'].value_counts()
   ```

3. **Purpose ที่ไม่ชัดเจน**
   ```python
   # มี "other", "Other", "OTHER"
   df['purpose'] = df['purpose'].str.lower()
   ```

4. **Loan Status ที่ซับซ้อน**
   ```python
   # มีหลายระดับของ Late
   late_statuses = ['Late (16-30 days)', 'Late (31-120 days)']
   ```

## 🛠️ เครื่องมือช่วยตรวจสอบ

### 📊 Libraries ที่มีประโยชน์
```python
# สำหรับ Data Profiling
import pandas_profiling
profile = pandas_profiling.ProfileReport(df)

# สำหรับ Data Quality
import great_expectations as ge
df_ge = ge.from_pandas(df)

# สำหรับ Missing Data Pattern
import missingno as msno
msno.matrix(df)
```

## 💡 Best Practices

### ✅ สิ่งที่ควรทำ
- **เข้าใจ Business Context**: รู้ว่าข้อมูลมาจากไหนและหมายถึงอะไร
- **สำรองข้อมูลต้นฉบับ**: เก็บข้อมูลดิบไว้เสมอ
- **จัดทำ Data Dictionary**: ระบุความหมายของแต่ละคอลัมน์
- **บันทึกขั้นตอนการแก้ไข**: เพื่อสามารถ reproduce ได้

### ❌ สิ่งที่ไม่ควรทำ
- **ลบ Outliers โดยไม่ตรวจสอบ**: อาจเป็นข้อมูลสำคัญ
- **เติมค่าว่างแบบไม่คิด**: ใช้ mean เสมออาจผิด
- **ละเลยการตรวจสอบความสอดคล้อง**: ข้อมูลอาจขัดแย้งกัน
- **รีบไปขั้นตอนถัดไปเร็วเกินไป**: Data Profiling ต้องทำให้ดี

## 📋 Checklist การแก้ไขปัญหา

### 🔍 ก่อนแก้ไข
- [ ] ระบุปัญหาทั้งหมดที่พบ
- [ ] จัดลำดับความสำคัญของปัญหา
- [ ] ประเมินผลกระทบของแต่ละปัญหา
- [ ] วางแผนการแก้ไขอย่างเป็นระบบ

### 🛠️ ระหว่างแก้ไข
- [ ] แก้ไขทีละปัญหา
- [ ] ตรวจสอบผลลัพธ์หลังแก้ไขทุกครั้ง
- [ ] บันทึกขั้นตอนการแก้ไข
- [ ] เก็บ backup ก่อนทำการเปลี่ยนแปลงใหญ่

### ✅ หลังแก้ไข
- [ ] ตรวจสอบคุณภาพข้อมูลใหม่
- [ ] ยืนยันว่าไม่มีปัญหาใหม่เกิดขึ้น
- [ ] จัดทำรายงานสรุปการแก้ไข
- [ ] วางแผนการป้องกันปัญหาในอนาคต

---
*⚠️ จำไว้: การแก้ไขปัญหาข้อมูลต้องทำอย่างระมัดระวังและเป็นระบบ*