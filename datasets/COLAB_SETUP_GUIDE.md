# 🚀 คู่มือการใช้งาน LoanStats_web_14422.csv ใน Google Colab

## 📋 ขั้นตอนการเตรียมความพร้อม

### 1️⃣ การเตรียมข้อมูลใน Google Drive

**ก่อนเริ่ม Lab ใดๆ ให้ทำขั้นตอนนี้ก่อน:**

1. **เข้า Google Drive** 
   - ไปที่ [drive.google.com](https://drive.google.com)
   - ใช้ Google Account เดียวกับที่จะใช้ Colab

2. **สร้างโฟลเดอร์**
   ```
   MyDrive/
   └── NT_Python_for_Data_Analytics/
       └── LoanStats_web_14422.csv  ← อัปโหลดไฟล์นี้
   ```

3. **ทดสอบการเข้าถึงข้อมูล**
   - เปิด Google Colab ใหม่
   - ทดสอบ mount drive และโหลดข้อมูล

### **📚 ในแต่ละ Lab**

1. **เปิด Notebook จาก README.md**
   - คลิก [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)] badge
   - หรือใช้ลิงค์ใน README

2. **รันส่วน Setup ก่อนเสมอ**
   ```python
   # Standard setup code
   from google.colab import drive
   import pandas as pd
   import numpy as np
   import matplotlib.pyplot as plt
   import seaborn as sns
   
   # Mount drive
   drive.mount('/content/drive')
   
   # Load data
   DATA_PATH = '/content/drive/MyDrive/NT_Python_for_Data_Analytics/LoanStats_web_14422.csv'
   df_loan = pd.read_csv(DATA_PATH, low_memory=False)
   
   print(f"✅ Data loaded: {df_loan.shape}")
   ```

3. **ใช้ Template Functions (สำหรับ Module 2)**
   - Copy functions จาก `LAB_TEMPLATES.md`
   - ใช้ `quick_profile(df_loan)` สำหรับ overview
   - ใช้ `analyze_column('column_name')` สำหรับวิเคราะห์เฉพาะคอลัมน์

---

## 🎯 การใช้งานเฉพาะใน Labs แต่ละ Module

### **📁 Module 1: Essential Data Analytics & Basic Python**

**Lab 01a-01e: ใช้ข้อมูลสำหรับตัวอย่าง**
```python
# ตัวอย่างการใช้ข้อมูลเบื้องต้น
print(f"📊 ข้อมูล Lending Club มีทั้งหมด {len(df_loan):,} รายการสินเชื่อ")
print(f"📋 มี {len(df_loan.columns)} คอลัมน์")

# ดูข้อมูลตัวอย่าง
print("\n🔍 ตัวอย่างข้อมูล 5 แถวแรก:")
df_loan.head()

# สถิติเบื้องต้น
if 'loan_amnt' in df_loan.columns:
    print(f"\n💰 จำนวนเงินกู้เฉลี่ย: ${df_loan['loan_amnt'].mean():,.2f}")
    print(f"💰 จำนวนเงินกู้สูงสุด: ${df_loan['loan_amnt'].max():,.2f}")
```

### **📁 Module 2: Data Profiling & Preparation**

**Lab 02a: Pandas DataFrame**
```python
# การสำรวจ DataFrame structure
print("📊 DataFrame Information:")
print(f"Shape: {df_loan.shape}")
print(f"Memory usage: {df_loan.memory_usage(deep=True).sum()/1024**2:.1f} MB")
print(f"Data types: {df_loan.dtypes.value_counts().to_dict()}")

# ดูคอลัมน์ทั้งหมด
print(f"\n📋 All columns ({len(df_loan.columns)}):")
for i, col in enumerate(df_loan.columns, 1):
    print(f"{i:3d}. {col}")
```

**Lab 02d: Data Profiling Workshop**
```python
# ใช้ comprehensive_data_profile function จาก templates
from LAB_TEMPLATES import comprehensive_data_profile

# หรือใช้ function แบบง่าย
def quick_loan_profile(df):
    print("🏦 LENDING CLUB DATA PROFILE")
    print("="*40)
    
    # Basic stats
    print(f"📊 Total loans: {len(df):,}")
    print(f"📊 Features: {len(df.columns)}")
    print(f"📊 Missing values: {df.isnull().sum().sum():,}")
    
    # Key columns analysis
    key_cols = ['loan_amnt', 'int_rate', 'grade', 'loan_status']
    available_cols = [col for col in key_cols if col in df.columns]
    
    print(f"\n🔑 Key columns available: {available_cols}")
    
    for col in available_cols:
        if df[col].dtype == 'object':
            print(f"\n📝 {col}: {df[col].nunique()} unique values")
            print(f"   Top 3: {df[col].value_counts().head(3).to_dict()}")
        else:
            print(f"\n🔢 {col}: ${df[col].mean():,.2f} average")
            print(f"   Range: ${df[col].min():,.2f} - ${df[col].max():,.2f}")

quick_loan_profile(df_loan)
```

**Lab 02h-02i: Correlation Analysis**
```python
# เลือกคอลัมน์สำคัญสำหรับวิเคราะห์
key_numeric = ['loan_amnt', 'int_rate', 'annual_inc', 'dti']
available_numeric = [col for col in key_numeric if col in df_loan.columns]

if len(available_numeric) >= 2:
    # Correlation matrix
    df_analysis = df_loan[available_numeric].copy()
    corr_matrix = df_analysis.corr()
    
    # Visualization
    plt.figure(figsize=(10, 8))
    sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', center=0)
    plt.title('Correlation Matrix - Key Lending Variables')
    plt.show()
    
    # หา correlation สูงสุด
    if 'int_rate' in corr_matrix.columns:
        int_rate_corr = corr_matrix['int_rate'].sort_values(ascending=False)
        print("🔗 Correlation with Interest Rate:")
        print(int_rate_corr)
else:
    print("❌ Not enough numeric columns for correlation analysis")
```

### **📁 Module 3: Machine Learning**

**Lab 03c: Classification (Loan Default Prediction)**
```python
# เตรียมข้อมูลสำหรับ Classification
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report
from sklearn.preprocessing import LabelEncoder

# เลือกฟีเจอร์และ target
features = ['loan_amnt', 'int_rate', 'annual_inc', 'dti']
target = 'loan_status'

# ตรวจสอบว่ามีคอลัมน์ที่ต้องการหรือไม่
available_features = [col for col in features if col in df_loan.columns]
has_target = target in df_loan.columns

print(f"🎯 Available features: {available_features}")
print(f"🎯 Target available: {has_target}")

if has_target and len(available_features) >= 2:
    # สร้าง working dataset
    df_work = df_loan[available_features + [target]].copy()
    
    # ทำความสะอาดข้อมูล - เก็บเฉพาะ loan ที่มีสถานะชัดเจน
    good_loans = ['Fully Paid', 'Current']
    bad_loans = ['Charged Off', 'Default']
    
    df_work = df_work[df_work[target].isin(good_loans + bad_loans)]
    
    # สร้าง binary target (0 = Good, 1 = Bad)
    df_work['default'] = df_work[target].apply(
        lambda x: 0 if x in good_loans else 1
    )
    
    print(f"\n📊 Dataset after cleaning: {df_work.shape}")
    print(f"📊 Default rate: {df_work['default'].mean()*100:.1f}%")
    
    # เตรียมข้อมูลสำหรับ modeling
    X = df_work[available_features].fillna(df_work[available_features].median())
    y = df_work['default']
    
    # แบ่งข้อมูล
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42, stratify=y
    )
    
    print(f"\n✅ Data ready for classification:")
    print(f"   Training set: {X_train.shape}")
    print(f"   Test set: {X_test.shape}")
    print(f"   Features: {list(X.columns)}")
    
    # Quick model training
    rf = RandomForestClassifier(n_estimators=50, random_state=42)
    rf.fit(X_train, y_train)
    
    # Predictions
    y_pred = rf.predict(X_test)
    
    print(f"\n📊 Classification Results:")
    print(classification_report(y_test, y_pred))
    
    # Feature importance
    importance_df = pd.DataFrame({
        'feature': X.columns,
        'importance': rf.feature_importances_
    }).sort_values('importance', ascending=False)
    
    print(f"\n🔝 Feature Importance:")
    print(importance_df)
    
else:
    print("❌ Missing required columns for classification")
    print(f"Available columns: {list(df_loan.columns[:20])}...")
```

**Lab 03e: Regression (Interest Rate Prediction)**
```python
# เตรียมข้อมูลสำหรับ Regression
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# เลือกฟีเจอร์และ target สำหรับ regression
features = ['loan_amnt', 'annual_inc', 'dti']
target = 'int_rate'

available_features = [col for col in features if col in df_loan.columns]
has_target = target in df_loan.columns

print(f"📈 Regression setup:")
print(f"   Features: {available_features}")
print(f"   Target: {target} ({'✅' if has_target else '❌'})")

if has_target and len(available_features) >= 2:
    # สร้าง working dataset
    df_reg = df_loan[available_features + [target]].dropna()
    
    print(f"\n📊 Dataset for regression: {df_reg.shape}")
    print(f"📊 Target statistics:")
    print(df_reg[target].describe())
    
    # เตรียมข้อมูล
    X = df_reg[available_features]
    y = df_reg[target]
    
    # แบ่งข้อมูล
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42
    )
    
    # Train model
    lr = LinearRegression()
    lr.fit(X_train, y_train)
    
    # Predictions
    y_pred = lr.predict(X_test)
    
    # Evaluate
    r2 = r2_score(y_test, y_pred)
    rmse = np.sqrt(mean_squared_error(y_test, y_pred))
    
    print(f"\n📊 Regression Results:")
    print(f"   R² Score: {r2:.3f}")
    print(f"   RMSE: {rmse:.3f}")
    
    # Coefficients
    coef_df = pd.DataFrame({
        'feature': X.columns,
        'coefficient': lr.coef_
    }).sort_values('coefficient', key=abs, ascending=False)
    
    print(f"\n🔍 Model Coefficients:")
    print(coef_df)
    
    # Simple visualization
    plt.figure(figsize=(10, 6))
    plt.scatter(y_test, y_pred, alpha=0.5)
    plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--', lw=2)
    plt.xlabel('Actual Interest Rate')
    plt.ylabel('Predicted Interest Rate')
    plt.title(f'Actual vs Predicted (R² = {r2:.3f})')
    plt.show()
    
else:
    print("❌ Missing required columns for regression")
```

---

## 🔧 Troubleshooting ปัญหาที่อาจพบ

### **❌ ปัญหาและวิธีแก้**

1. **"FileNotFoundError: LoanStats_web_14422.csv"**
   ```python
   # ตรวจสอบไฟล์ใน Drive
   import os
   files = os.listdir('/content/drive/MyDrive/NT_Python_for_Data_Analytics/')
   print("Files found:", files)
   ```

2. **"Drive not mounted"**
   ```python
   # Mount ใหม่
   from google.colab import drive
   drive.mount('/content/drive', force_remount=True)
   ```

3. **"Memory Error" หรือ "Out of Memory"**
   ```python
   # ใช้ sample หรือเลือกเฉพาะคอลัมน์ที่ต้องการ
   important_cols = ['loan_amnt', 'int_rate', 'grade', 'loan_status', 'annual_inc']
   df_small = pd.read_csv(DATA_PATH, usecols=important_cols)
   
   # หรือใช้ sample
   df_sample = df_loan.sample(n=5000)  # ใช้ 5000 แถว
   ```

4. **"Column not found"**
   ```python
   # ตรวจสอบชื่อคอลัมน์ที่มีจริง
   print("Available columns:")
   for i, col in enumerate(df_loan.columns):
       print(f"{i+1:3d}. {col}")
   
   # หาคอลัมน์ที่มีคำที่ต้องการ
   search_term = 'loan'
   matching_cols = [col for col in df_loan.columns if search_term.lower() in col.lower()]
   print(f"Columns containing '{search_term}': {matching_cols}")
   ```

5. **"Performance Issues"**
   ```python
   # ใช้ sample สำหรับการทดสอบ
   df_test = df_loan.sample(n=1000)
   
   # ลด memory usage
   df_loan = df_loan.select_dtypes(exclude=['object'])  # เก็บเฉพาะตัวเลข
   
   # Clear memory
   del df_sample
   import gc
   gc.collect()
   ```

---

## 📝 สรุปและเคล็ดลับสำคัญ

### **✅ ข้อควรจำ**
1. **เสมอ mount Google Drive ก่อน** ในทุก notebook
2. **ใช้ path ที่สม่ำเสมอ**: `/content/drive/MyDrive/NT_Python_for_Data_Analytics/`
3. **ตรวจสอบข้อมูลก่อนใช้**: `.shape`, `.info()`, `.head()`, `.columns`
4. **ใช้ sample ขณะพัฒนา**: `df.sample(n=1000)` เพื่อความเร็ว
5. **จัดการ missing data อย่างเหมาะสม**: `.dropna()`, `.fillna()`
6. **บันทึกผลงาน**: save กลับไปยัง Google Drive
7. **ล้าง memory เมื่อจำเป็น**: `del variable; gc.collect()`

### **🎯 Best Practices**
- เริ่มด้วย sample เล็กๆ เสมอ
- ตรวจสอบคุณภาพข้อมูลก่อนวิเคราะห์
- ใช้ visualization เพื่อเข้าใจข้อมูล
- เขียน code ที่อ่านง่าย มี comment
- เก็บ notebook ที่สำคัญไว้ใน Drive

### **🚀 Next Steps**
1. ทดสอบการ setup กับข้อมูลจริง
2. เตรียม environment สำหรับนักเรียน
3. สร้าง backup plans สำหรับปัญหาที่อาจเกิดขึ้น
4. เตรียมข้อมูลเสริมหากต้องการ

---

**🎓 พร้อมสำหรับการเรียนการสอน Course 250FDEV01C00 แล้ว!**

*หากมีคำถามเพิ่มเติม สามารถอ้างอิงจาก COLAB_SETUP_GUIDE.md และ LAB_TEMPLATES.md ที่เตรียมไว้*rive/
   └── NT_Python_for_Data_Analytics/
       └── LoanStats_web_14422.csv  ← อัปโหลดไฟล์นี้
   ```

3. **อัปโหลดไฟล์**
   - ลากไฟล์ `LoanStats_web_14422.csv` ไปใน Google Drive
   - หรือใช้ปุ่ม "New" > "File upload"

### 2️⃣ การใช้งานใน Google Colab

#### 🔗 Template Code (ใส่ในทุก Lab)

```python
# 🚀 Standard Setup สำหรับทุก Lab
from google.colab import drive
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Mount Google Drive
drive.mount('/content/drive')

# โหลดข้อมูล
DATA_PATH = '/content/drive/MyDrive/NT_Python_for_Data_Analytics/LoanStats_web_14422.csv'
df_loan = pd.read_csv(DATA_PATH, low_memory=False)

print(f"✅ Data loaded: {df_loan.shape[0]:,} rows × {df_loan.shape[1]:,} columns")
```

---

## 📚 การใช้งานใน Labs แต่ละ Module

### 📁 Module 1: Essential Data Analytics & Basic Python

**Lab 01a-01e: ความรู้เบื้องต้น**
```python
# ตัวอย่างการใช้ข้อมูลเบื้องต้น
print("📊 ข้อมูล Lending Club")
print(f"จำนวนสินเชื่อ: {len(df_loan):,} รายการ")
print(f"คอลัมน์ทั้งหมด: {len(df_loan.columns)} คอลัมน์")

# ดูข้อมูลตัวอย่าง
df_loan.head()
```

### 📁 Module 2: Data Profiling & Preparation

#### **Lab 02a: Pandas DataFrame**
```python
# การสำรวจ DataFrame
print("📊 DataFrame Info:")
print(f"Shape: {df_loan.shape}")
print(f"Memory usage: {df_loan.memory_usage(deep=True).sum()/1024**2:.1f} MB")

# ดูประเภทข้อมูล
df_loan.dtypes.value_counts()
```

#### **Lab 02d: Data Profiling**
```python
# Data Profiling เบื้องต้น
def quick_profile(df):
    print("🔍 Data Profile Summary")
    print("="*40)
    print(f"Rows: {df.shape[0]:,}")
    print(f"Columns: {df.shape[1]:,}")
    print(f"Missing values: {df.isnull().sum().sum():,}")
    print(f"Duplicates: {df.duplicated().sum():,}")
    
    # ข้อมูลตัวเลข vs ข้อมูลข้อความ
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    text_cols = df.select_dtypes(include=['object']).columns
    
    print(f"Numeric columns: {len(numeric_cols)}")
    print(f"Text columns: {len(text_cols)}")
    
    return df.describe()

# ใช้งาน
stats = quick_profile(df_loan)
```

---

## 🔧 Troubleshooting

### ❌ ปัญหาที่พบบ่อย

1. **"No such file or directory"**
   ```python
   # ตรวจสอบ path
   import os
   path = '/content/drive/MyDrive/NT_Python_for_Data_Analytics/'
   print("Files in directory:")
   print(os.listdir(path))
   ```

2. **"Drive not mounted"**
   ```python
   # Mount ใหม่
   from google.colab import drive
   drive.mount('/content/drive', force_remount=True)
   ```

3. **"Memory error"**
   ```python
   # โหลดเฉพาะบางคอลัมน์
   important_cols = ['loan_amnt', 'int_rate', 'grade', 'loan_status']
   df_small = pd.read_csv(DATA_PATH, usecols=important_cols)
   ```

### ✅ เคล็ดลับการใช้งาน

1. **ตรวจสอบข้อมูลก่อนใช้งาน**
   ```python
   print(f"Shape: {df_loan.shape}")
   print(f"Columns: {list(df_loan.columns[:10])}")  # แสดง 10 คอลัมน์แรก
   ```

2. **ใช้ sample ขณะทดสอบ**
   ```python
   df_sample = df_loan.sample(n=1000)  # ใช้ sample ขณะ develop
   ```

3. **จัดการ memory**
   ```python
   # ลบตัวแปรที่ไม่ใช้
   del df_sample
   import gc
   gc.collect()
   ```

4. **เซฟผลงาน**
   ```python
   # บันทึกผลลัพธ์กลับไปยัง Drive
   df_result.to_csv('/content/drive/MyDrive/NT_Python_for_Data_Analytics/results.csv')
   ```

---

## 📱 Quick Reference Commands

### 🔄 Essential Commands
```python
# Mount Drive
from google.colab import drive
drive.mount('/content/drive')

# Load Data
df = pd.read_csv('/content/drive/MyDrive/NT_Python_for_Data_Analytics/LoanStats_web_14422.csv')

# Quick Info
print(f"Shape: {df.shape}")
print(f"Columns: {df.columns.tolist()[:10]}")
print(f"Missing: {df.isnull().sum().sum()}")

# Sample Data
df_sample = df.sample(1000)
```

### 📊 Quick Analysis
```python
# Descriptive Stats
df.describe()

# Missing Data
df.isnull().sum().sort_values(ascending=False).head(10)

# Correlation
df.corr()['target_column'].sort_values(ascending=False)

# Value Counts
df['categorical_column'].value_counts()
```

### 🎨 Quick Plots
```python
# Distribution
df['column'].hist(bins=50)
plt.show()

# Correlation Heatmap
sns.heatmap(df.corr(), annot=True)
plt.show()

# Box Plot
df.boxplot(column='numeric_column', by='categorical_column')
plt.show()
```

---

## 🎯 สรุป Best Practices

1. **เสมอ mount Drive ก่อน** ในทุก notebook
2. **ใช้ path ที่สม่ำเสมอ** `/content/drive/MyDrive/NT_Python_for_Data_Analytics/`
3. **ตรวจสอบข้อมูลก่อนใช้** ด้วย `.shape`, `.info()`, `.head()`
4. **ใช้ sample ขณะพัฒนา** เพื่อความเร็ว
5. **จัดการ missing data** อย่างเหมาะสม
6. **บันทึกผลงาน** กลับไปยัง Drive
7. **ล้าง memory** เมื่อเสร็จแต่ละ task

🎓 **Happy Learning with Course 250FDEV01C00!**

---

*สำหรับคำถามเพิ่มเติม สามารถติดต่อผู้สอนหรือดูใน course materials*