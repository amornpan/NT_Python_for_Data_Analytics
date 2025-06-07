# Session 2: Data Profiling and Preparation

## เนื้อหา
### 1. Understanding of Pandas DataFrame
- DataFrame structure และ operations
- การอ่านข้อมูลจากไฟล์ต่างๆ (CSV, Excel, JSON)
- DataFrame indexing และ selection
- Data manipulation techniques

### 2. Data Profiling with Univariate/Multivariate Analysis
- Descriptive statistics
- Univariate analysis (single variable)
- Multivariate analysis (multiple variables)
- Correlation analysis
- Data quality assessment

### 3. Data Distribution - Shapes of Data with Implications
- Normal distribution
- Skewed distributions
- Outlier detection
- Distribution visualization
- Statistical implications

### 4. Workshop/LAB for Data Profiling
- Profiling LoanStats_web_14422.csv
- Summary statistics generation
- Missing value analysis
- Data type assessment
- Distribution analysis

### 5. Workshop/LAB for Data Preparation
- Handling missing values
- Data type conversion
- Outlier treatment
- Feature engineering
- Data transformation

## ไฟล์ในโฟลเดอร์นี้
### หมวดที่ 1: Understanding of Pandas DataFrame
- `02a_Pandas_DataFrame_ความรู้เบื้องต้น.ipynb` - Pandas DataFrame พื้นฐาน
- `02b_การอ่านข้อมูลจากไฟล์.ipynb` - การนำเข้าข้อมูลจากไฟล์ต่างๆ
- `02c_การจัดการและเลือกข้อมูล.ipynb` - DataFrame selection และ manipulation

### หมวดที่ 2: Data Profiling พื้นฐาน
- `02d_Data_Profiling_ความรู้เบื้องต้น.ipynb` - ทำความรู้จัก Data Profiling
- `02e_การตรวจสอบคุณภาพข้อมูล.ipynb` - Data Quality Assessment

### หมวดที่ 3: Univariate Analysis
- `02f_Univariate_Analysis_เชิงสถิติ.ipynb` - การวิเคราะห์ตัวแปรเดียว
- `02g_Univariate_Visualization.ipynb` - การแสดงผลข้อมูลตัวแปรเดียว

### หมวดที่ 4: Multivariate Analysis
- `02h_Multivariate_Analysis_เชิงสถิติ.ipynb` - การวิเคราะห์หลายตัวแปร
- `02i_Correlation_Analysis.ipynb` - การวิเคราะห์ความสัมพันธ์

### หมวดที่ 5: Data Distribution Analysis
- `02j_การวิเคราะห์การกระจายข้อมูล.ipynb` - Distribution shapes และ implications
- `02k_Outlier_Detection.ipynb` - การหา Outliers

### หมวดที่ 6: Workshop & Labs
- `02l_Workshop_Lending_Club_Profiling.ipynb` - Workshop: Data Profiling ข้อมูล Lending Club
- `02m_Workshop_Data_Preparation.ipynb` - Workshop: การเตรียมข้อมูล
- `02n_Lab_Complete_Pipeline.ipynb` - Lab: ท่อส่งข้อมูลแบบครบวงจร

### หมวดที่ 7: เอกสารเสริม
- `data_profiling_checklist.md` - Data profiling checklist
- `common_data_issues.md` - ปัญหาข้อมูลที่พบบ่อย

## Learning Objectives
หลังจบ session นี้ ผู้เรียนจะสามารถ:
- ใช้ Pandas ในการจัดการข้อมูล
- ทำ Data profiling และประเมินคุณภาพข้อมูล
- วิเคราะห์การกระจายของข้อมูล
- เตรียมข้อมูลสำหรับการวิเคราะห์
- ใช้เทคนิคการทำความสะอาดข้อมูล

## Key Libraries
- pandas
- numpy
- matplotlib
- seaborn
- scipy.stats

## Duration
- ทฤษฎี: 3 ชั่วโมง
- Workshop/Lab: 5 ชั่วโมง
- รวม: 8 ชั่วโมง

## การใช้งาน
1. เริ่มจากไฟล์ `02a` ตามลำดับ
2. ทำ Workshop และ Lab หลังเรียนทฤษฎี
3. ใช้ checklist ตรวจสอบความเข้าใจ
4. อ้างอิง common issues เมื่อพบปัญหา