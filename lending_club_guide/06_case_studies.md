# 💡 Case Studies และการประยุกต์ใช้

## 📋 สารบัญ
- [🎯 Case Study 1: Credit Risk Assessment](#-case-study-1-credit-risk-assessment)
- [📊 Case Study 2: Portfolio Optimization](#-case-study-2-portfolio-optimization)
- [🔍 Case Study 3: Customer Segmentation](#-case-study-3-customer-segmentation)
- [📈 Case Study 4: Pricing Strategy](#-case-study-4-pricing-strategy)
- [⚠️ Case Study 5: Early Warning System](#️-case-study-5-early-warning-system)

---

## 🎯 Case Study 1: Credit Risk Assessment
*การประเมินความเสี่ยงเครดิตสำหรับผู้สมัครใหม่*

### 📋 **Background & Business Problem**

**สถานการณ์**: Lending Club ต้องการปรับปรุงกระบวนการประเมินความเสี่ยงเครดิตเพื่อลดอัตราการผิดนัดชำระจาก 12% เป็น 10%

**Business Questions**:
1. ปัจจัยไหนที่ส่งผลต่อการผิดนัดชำระมากที่สุด?
2. เราสามารถสร้างโมเดลที่ทำนายความเสี่ยงได้แม่นยำแค่ไหน?
3. การปรับเกณฑ์การอนุมัติจะส่งผลกระทบต่อธุรกิจอย่างไร?

### 💡 **Key Recommendations**
1. **Model Implementation**: ใช้ threshold 0.25 เพื่อบรรลุเป้าหมาย default rate 10%
2. **Focus Areas**: เพิ่มความสำคัญกับ FICO score และ DTI ratio 
3. **Data Quality**: ปรับปรุงการ verify รายได้และประวัติการทำงาน
4. **Monitoring**: ติดตาม model performance และปรับปรุงทุก 3 เดือน

---

## 📊 Case Study 2: Portfolio Optimization
*การหาส่วนผสมที่เหมาะสมของ portfolio สินเชื่อ*

### 🎯 **Business Problem**
Lending Club ต้องการออกแบบ portfolio ใหม่เพื่อเพิ่ม risk-adjusted returns โดยคงเป้าหมาย ROI 15% และ default rate ไม่เกิน 8%

### 🎯 **Recommendations**
1. **Optimal Mix**: เน้น Grade B (40%), Grade C (35%), Grade A (20%), และ Grade D (5%)
2. **Risk Management**: หลีกเลี่ยง Grade F และ G เนื่องจาก risk-return profile ไม่ดี
3. **Diversification**: กระจายการลงทุนใน 3-4 grades หลักเพื่อลดความเสี่ยง
4. **Rebalancing**: ทบทวนและปรับ portfolio ทุก 6 เดือน

---

## 🔍 Case Study 3: Customer Segmentation
*การแบ่งกลุ่มลูกค้าเพื่อ targeted marketing*

### 🎯 **Business Problem**
Lending Club ต้องการเพิ่มประสิทธิภาพการตลาดโดยการทำความเข้าใจลูกค้าแต่ละกลุ่มและสร้าง campaigns ที่ตรงเป้า

### 💡 **Marketing Strategies**

**Segment 1 - Premium Customers**:
- Approach: Competitive rates, high loan amounts, premium service
- Channels: Direct mail, email, phone
- Products: Large loans, home improvement, debt consolidation

**Segment 2 - Core Profitable**:
- Approach: Standard products, volume focus, loyalty programs
- Channels: Email, social media, referral programs
- Products: Standard loans, repeat customer offers

**Segment 3 - Growth Potential**:
- Approach: Educational content, gradual credit building
- Channels: Social media, content marketing, mobile app
- Products: Small loans, credit education, financial tools

**Segment 4 - High Risk**:
- Approach: Careful screening, higher rates, smaller amounts
- Channels: Targeted ads, call center
- Products: Small secured loans, financial counseling

---

## 📈 Case Study 4: Pricing Strategy
*การปรับกลยุทธ์การกำหนดราคาเพื่อเพิ่มผลกำไร*

### 🎯 **Business Problem**
Lending Club ต้องการปรับปรุงกลยุทธ์การกำหนดอัตราดอกเบี้ยเพื่อเพิ่มความสามารถในการแข่งขันและเพิ่มผลกำไร

### 💡 **Pricing Recommendations**
1. **Grade A-B**: ลดอัตราดอกเบี้ย 0.5-1% เพื่อเพิ่มความสามารถในการแข่งขัน
2. **Grade C-D**: เพิ่มอัตราดอกเบี้ย 0.5% เนื่องจาก pricing ต่ำเกินไป
3. **Grade E-G**: ปรับปรุงกระบวนการ underwriting แทนการเพิ่มราคา
4. **Dynamic Pricing**: ใช้ระบบปรับราคาแบบ real-time ตาม market conditions

---

## ⚠️ Case Study 5: Early Warning System
*ระบบเตือนภัยสำหรับสินเชื่อที่มีความเสี่ยงสูง*

### 🎯 **Business Problem**
Lending Club ต้องการระบบที่สามารถระบุสินเชื่อที่มีแนวโน้มจะเป็นปัญหาล่วงหน้า เพื่อดำเนินการป้องกันหรือลดขนาดของปัญหา

### 💡 **System Implementation**
1. **Real-time Monitoring**: ติดตั้งระบบที่อัปเดต risk scores แบบ real-time
2. **Automated Alerts**: ส่งการแจ้งเตือนอัตโนมัติเมื่อ risk score เปลี่ยนแปลง
3. **Performance Tracking**: ติดตามประสิทธิภาพของระบบและปรับปรุงอย่างต่อเนื่อง
4. **Integration**: เชื่อมต่อกับระบบ CRM และ customer service

### 📊 **Risk Levels & Actions**

**Red Alert (Risk Score ≥ 70)**:
- Actions: ติดต่อลูกค้าทันที, พิจารณาการปรับโครงสร้างหนี้, หยุดการให้สินเชื่อเพิ่ม
- Timeline: ภายใน 1 สัปดาห์
- Owner: Risk Management Team

**Orange Alert (Risk Score 50-69)**:
- Actions: ส่งการแจ้งเตือนเชิงป้องกัน, เสนอโปรแกรม financial counseling
- Timeline: ภายใน 2 สัปดาห์
- Owner: Customer Success Team

**Yellow Alert (Risk Score 30-49)**:
- Actions: ส่งข้อมูลการศึกษาทางการเงิน, เสนอเครื่องมือบริหารการเงิน
- Timeline: ภายใน 1 เดือน
- Owner: Customer Care Team

---

## 🎓 การประยุกต์ใช้ในหลักสูตร

### 📚 **Day 1: Descriptive Analytics**
- Case Study 1: Risk factor analysis
- Case Study 2: Performance analysis by grade
- Focus: การใช้ pandas, basic statistics, data visualization

### 📊 **Day 2: Advanced Analytics**
- Case Study 3: Customer segmentation with clustering
- Case Study 4: Pricing analysis และ competitive benchmarking
- Focus: Machine learning, statistical testing, business insights

### 🤖 **Day 3: Predictive Modeling**
- Case Study 1: Credit risk prediction model
- Case Study 5: Early warning system development
- Focus: Predictive modeling, business application, implementation

### 💡 **Learning Outcomes**

หลังจากศึกษา case studies เหล่านี้ ผู้เรียนจะสามารถ:

✅ **วิเคราะห์ความเสี่ยง**: ประเมินและจัดการความเสี่ยงเครดิต  
✅ **ปรับปรุง portfolio**: หาส่วนผสมที่เหมาะสมของสินทรัพย์  
✅ **เข้าใจลูกค้า**: แบ่งกลุ่มและสร้างกลยุทธ์ตามกลุ่ม  
✅ **กำหนดราคา**: สร้างกลยุทธ์ pricing ที่เหมาะสม  
✅ **ป้องกันปัญหา**: สร้างระบบเตือนภัยล่วงหน้า  

### 🔧 **Technical Skills Developed**

- **Python**: pandas, numpy, scikit-learn, matplotlib, seaborn
- **Statistics**: hypothesis testing, correlation analysis, statistical significance
- **Machine Learning**: classification, clustering, optimization, model evaluation
- **Business Analysis**: KPI development, ROI calculation, risk assessment
- **Data Visualization**: creating meaningful charts และ dashboards

### 🌟 **Real-world Applications**

**ในอุตสาหกรรมการเงิน**:
- Credit scoring models
- Portfolio risk management  
- Customer lifecycle management
- Fraud detection systems
- Regulatory compliance

**ในธุรกิจอื่นๆ**:
- Customer segmentation สำหรับ e-commerce
- Predictive maintenance ในอุตสาหกรรม
- Demand forecasting ในค้าปลีก
- Risk assessment ในประกันภัย
- Performance optimization ในการตลาด

---

*📖 หมายเหตุ: Case studies เหล่านี้ใช้ข้อมูลจริงจาก Lending Club เพื่อให้ผู้เรียนได้ประสบการณ์การแก้ปัญหาธุรกิจจริงและเรียนรู้การประยุกต์ใช้ data analytics ในบริบทของธุรกิจการเงิน*

---
[⬅️ กลับไปที่ Business Questions](./05_business_questions.md) | [➡️ ไปยัง Course Applications](./07_course_applications.md)