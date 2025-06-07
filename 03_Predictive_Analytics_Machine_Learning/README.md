# Session 3: Predictive Analytics and Machine Learning

## เนื้อหา
### 1. Introduction to Predictive Analytics and Machine Learning
- ความแตกต่างระหว่าง Descriptive และ Predictive Analytics
- ประเภทของ Machine Learning (Supervised, Unsupervised, Reinforcement)
- Machine Learning workflow
- Model evaluation concepts

### 2. Algorithms for Classification
- Logistic Regression
- Decision Trees
- Random Forest
- Model evaluation metrics (Accuracy, Precision, Recall, F1-score)

### 3. LAB for Classification
- Predicting loan default using LoanStats data
- Data preprocessing for ML
- Model training and evaluation
- Cross-validation

### 4. Algorithms for Regression
- Linear Regression
- Polynomial Regression
- Ridge and Lasso Regression
- Random Forest Regression
- Model evaluation metrics (MAE, MSE, RMSE, R²)

### 5. LAB for Regression
- Predicting loan interest rates
- Feature engineering techniques
- Model comparison
- Model interpretation

### 6. Model Evaluation และ Comparison
- Cross-validation best practices
- Hyperparameter tuning
- Model comparison framework

### 7. Best Practices และ Conclusion
- Best practices in ML projects
- Model deployment considerations
- Ethical considerations in ML
- Next steps in ML journey

## ไฟล์ในโฟลเดอร์นี้
- `03a_ML_Introduction_Theory.ipynb` - ML concepts และ theory
- `03b_Classification_Algorithms.ipynb` - Classification methods
- `03c_Classification_LAB.ipynb` - Classification lab
- `03d_Regression_Algorithms.ipynb` - Regression methods
- `03e_Regression_LAB.ipynb` - Regression lab
- `03f_Model_Evaluation_Comparison.ipynb` - Model evaluation
- `03g_ML_Best_Practices_Conclusion.ipynb` - Best practices
- `ml_cheatsheet.md` - ML algorithm cheatsheet

## Learning Objectives
หลังจบ session นี้ ผู้เรียนจะสามารถ:
- อธิบายหลักการของ Machine Learning
- เลือกใช้ algorithm ที่เหมาะสมกับปัญหา
- สร้างและประเมิน Classification models
- สร้างและประเมิน Regression models
- ใช้ scikit-learn ในการสร้าง ML models
- ตีความผลลัพธ์และประเมินประสิทธิภาพของ model

## Key Libraries
- scikit-learn
- pandas
- numpy
- matplotlib
- seaborn
- scipy

## Duration
- ทฤษฎี: 3 ชั่วโมง
- Labs: 3 ชั่วโมง
- รวม: 6 ชั่วโมง

## Business Applications
- Credit risk assessment
- Customer segmentation
- Price prediction
- Fraud detection
- Recommendation systems

## การใช้งานใน Google Colab
```python
# Upload ไฟล์ข้อมูล
from google.colab import files
uploaded = files.upload()

# ติดตั้ง libraries เพิ่มเติม (ถ้าจำเป็น)
!pip install scikit-learn pandas matplotlib seaborn

# Import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn import *
```

## หมายเหตุ
- ไฟล์ทั้งหมดออกแบบให้ใช้งานใน Google Colab ได้
- แต่ละไฟล์เป็นอิสระกัน สามารถเรียนรู้ทีละไฟล์ได้
- ควรเรียนตามลำดับสำหรับผู้เริ่มต้น
- มี cheat sheet สำหรับใช้อ้างอิงอย่างรวดเร็ว
