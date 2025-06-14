# 🔄 โมเดลธุรกิจ P2P Lending

## 📋 สารบัญ
- [2.1 P2P Lending คืออะไร](#21-p2p-lending-คืออะไร)
- [2.2 วิธีการทำงานของ Lending Club](#22-วิธีการทำงานของ-lending-club)
- [2.3 แหล่งรายได้และต้นทุน](#23-แหล่งรายได้และต้นทุน)
- [2.4 การบริหารความเสี่ยง](#24-การบริหารความเสี่ยง)
- [2.5 กลุ่มลูกค้าเป้าหมาย](#25-กลุ่มลูกค้าเป้าหมาย)
- [2.6 การประเมินเครดิตและการกำหนดราคา](#26-การประเมินเครดิตและการกำหนดราคา)
- [2.7 KPIs และตัววัดผลสำคัญ](#27-kpis-และตัววัดผลสำคัญ)
- [2.8 ความท้าทายและโอกาส](#28-ความท้าทายและโอกาส)

---

## 2.1 P2P Lending คืออะไร

### 💡 **แนวคิดพื้นฐาน**

**Peer-to-Peer (P2P) Lending** หรือ **Marketplace Lending** เป็นรูปแบบการให้สินเชื่อที่เชื่อมต่อ:

- **👨‍💼 ผู้กู้** (Borrowers) ที่ต้องการเงินกู้
- **💰 นักลงทุน** (Investors) ที่ต้องการผลตอบแทนจากการลงทุน
- **🏢 แพลตฟอร์ม** ที่ทำหน้าที่เป็นตัวกลาง

โดยตัดบทบาทของธนาคารดั้งเดิมออก (**Disintermediation**)

### 🔄 **การเปรียบเทียบกับแบบดั้งเดิม**

#### **🏦 แบบดั้งเดิม (Traditional Banking)**
```
ผู้กู้ ↔️ ธนาคาร ↔️ ผู้ฝากเงิน
      (รับเงินฝาก → ให้สินเชื่อ)
```

#### **🌐 แบบ P2P Lending**
```
ผู้กู้ ↔️ แพลตฟอร์ม ↔️ นักลงทุน
      (เชื่อมต่อโดยตรง)
```

### 💎 **ข้อดีเชิงโครงสร้าง**

| ประเด็น | ธนาคารดั้งเดิม | P2P Lending |
|---------|----------------|-------------|
| **🏢 โครงสร้างต้นทุน** | สาขา + พนักงานมาก | แพลตฟอร์มออนไลน์ |
| **💰 แหล่งเงินทุน** | เงินฝาก + เงินกู้ | นักลงทุนโดยตรง |
| **⚡ ความเร็ว** | 1-4 สัปดาห์ | 3-7 วัน |
| **📊 ความโปร่งใส** | จำกัด | สูง |
| **🎯 การกำหนดราคา** | มาตรฐาน | ปรับตามความเสี่ยง |

### 🎯 **ประโยชน์สำหรับแต่ละฝ่าย**

#### **สำหรับผู้กู้ 👨‍💼**
- ✅ **อัตราดอกเบี้ยต่ำกว่า**: เฉลี่ยต่ำกว่าบัตรเครดิต 3-5%
- ✅ **กระบวนการเร็ว**: อนุมัติภายใน 1 สัปดาห์
- ✅ **ไม่มีค่าธรรมเนียมแอบแฝง**: ชัดเจนตั้งแต่เริ่มต้น
- ✅ **ยืดหยุ่น**: สามารถชำระก่อนกำหนดได้

#### **สำหรับนักลงทุน 💰**
- ✅ **ผลตอบแทนสูง**: 6-12% ต่อปี (มากกว่าเงินฝาก)
- ✅ **การกระจายความเสี่ยง**: ลงทุนในหลายสินเชื่อ
- ✅ **ความโปร่งใส**: เห็นข้อมูลผู้กู้ละเอียด
- ✅ **ควบคุมได้**: เลือกสินเชื่อที่ต้องการลงทุน

---

## 2.2 วิธีการทำงานของ Lending Club

### 📋 **กระบวนการสำหรับผู้กู้**

#### **ขั้นตอนที่ 1: การสมัคร**
```python
# ข้อมูลที่ผู้กู้ต้องให้
borrower_application = {
    'personal_info': {
        'name': 'ชื่อ-นามสกุล',
        'address': 'ที่อยู่',
        'social_security': 'เลขประจำตัวประชาชน'
    },
    'financial_info': {
        'annual_income': 'รายได้ต่อปี',
        'employment_length': 'ระยะเวลาการทำงาน',
        'home_ownership': 'สถานะที่อยู่อาศัย'
    },
    'loan_details': {
        'loan_amount': 'จำนวนเงินที่ต้องการ',
        'loan_purpose': 'วัตถุประสงค์การกู้',
        'loan_term': 'ระยะเวลาผ่อนชำระ'
    }
}
```

#### **ขั้นตอนที่ 2: การประเมินเครดิต**
```python
# กระบวนการประเมิน
credit_evaluation = {
    'credit_score': 'FICO Score 660-850',
    'credit_history': 'ประวัติการใช้เครดิต',
    'debt_to_income': 'อัตราส่วนหนี้ต่อรายได้ < 40%',
    'employment_verification': 'การยืนยันการทำงาน',
    'income_verification': 'การยืนยันรายได้'
}
```

#### **ขั้นตอนที่ 3: การกำหนดเกรดและอัตราดอกเบี้ย**
| เกรด | FICO Score | อัตราดอกเบี้ย | คุณสมบัติหลัก |
|------|------------|---------------|---------------|
| **A** | 740-850 | 5.3-9.3% | เครดิตดีเยี่ยม, รายได้มั่นคง |
| **B** | 680-739 | 9.3-13.6% | เครดิตดี, ประวัติดี |
| **C** | 660-679 | 13.6-18.1% | เครดิตปานกลาง |
| **D** | 640-659 | 18.1-22.9% | เครดิตพอใช้ |
| **E** | 620-639 | 22.9-26.3% | เครดิตต่ำ |
| **F** | 600-619 | 26.3-28.9% | เครดิตต่ำมาก |
| **G** | 580-599 | 28.9%+ | เครดิตแย่ |

### 💰 **กระบวนการสำหรับนักลงทุน**

#### **ขั้นตอนที่ 1: การลงทะเบียน**
```python
# ประเภทนักลงทุน
investor_types = {
    'individual': {
        'min_income': 70000,  # รายได้ขั้นต่ำ $70k
        'min_net_worth': 70000,  # มูลค่าสุทธิ $70k
        'max_investment': '10% ของมูลค่าสุทธิ'
    },
    'institutional': {
        'min_investment': 100000,  # ลงทุนขั้นต่ำ $100k
        'accredited_investor': True,
        'professional_management': True
    }
}
```

#### **ขั้นตอนที่ 2: การเลือกสินเชื่อ**
```python
# ข้อมูลที่นักลงทุนใช้ตัดสินใจ
loan_information_for_investors = {
    'loan_basics': {
        'loan_amount': 'จำนวนเงินกู้',
        'interest_rate': 'อัตราดอกเบี้ย',
        'term': 'ระยะเวลา',
        'grade': 'เกรดความเสี่ยง'
    },
    'borrower_info': {
        'annual_income': 'รายได้ต่อปี',
        'employment_length': 'ระยะเวลาการทำงาน',
        'home_ownership': 'สถานะที่อยู่อาศัย',
        'purpose': 'วัตถุประสงค์การกู้'
    },
    'credit_metrics': {
        'credit_score_range': 'ช่วง FICO Score',
        'delinquencies': 'ประวัติผิดนัดชำระ',
        'public_records': 'บันทึกสาธารณะ',
        'inquiries': 'การสอบถามเครดิต'
    }
}
```

#### **ขั้นตอนที่ 3: การกระจายการลงทุน**
```python
# กลยุทธ์การลงทุน
investment_strategies = {
    'conservative': {
        'grades': ['A', 'B'],
        'expected_return': '6-9%',
        'default_rate': '2-4%'
    },
    'balanced': {
        'grades': ['B', 'C', 'D'],
        'expected_return': '8-12%',
        'default_rate': '4-8%'
    },
    'aggressive': {
        'grades': ['D', 'E', 'F', 'G'],
        'expected_return': '12-18%',
        'default_rate': '8-15%'
    }
}
```

### 🔄 **การจับคู่และการจัดสรรเงิน**

#### **การจับคู่อัตโนมัติ**
```python
# ระบบจับคู่
matching_system = {
    'borrower_posts_loan': 'ผู้กู้โพสต์ความต้องการ',
    'investors_browse': 'นักลงทุนเลือกสินเชื่อ',
    'automated_matching': 'ระบบจับคู่อัตโนมัติ',
    'funding_period': '14 วัน (สำหรับการระดมทุน)',
    'minimum_funding': '85% ของจำนวนที่ขอ'
}
```

#### **การจัดสรรเงิน**
```python
# ตัวอย่างการจัดสรร
loan_allocation_example = {
    'loan_amount': 25000,
    'investors': [
        {'investor_1': 500, 'percentage': 2.0},
        {'investor_2': 1000, 'percentage': 4.0},
        {'investor_3': 2500, 'percentage': 10.0},
        # ... นักลงทุนอื่นๆ
        {'investor_100': 250, 'percentage': 1.0}
    ],
    'total_funded': 25000,
    'funding_status': 'Fully Funded'
}
```

---

*📖 หมายเหตุ: โมเดลธุรกิจนี้อ้างอิงจากการดำเนินงานของ Lending Club ในช่วงปี 2007-2014 ซึ่งเป็นช่วงเวลาที่ข้อมูลในหลักสูตรครอบคลุม และอาจมีการเปลี่ยนแปลงในปัจจุบัน*

---
[⬅️ กลับไปที่ Lending Club Overview](./01_lending_club_overview.md) | [➡️ ไปยัง Dataset Overview](./03_dataset_overview.md)
