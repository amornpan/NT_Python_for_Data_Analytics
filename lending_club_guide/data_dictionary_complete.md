# 📋 Lending Club Data Dictionary - Complete Reference

## 📊 ข้อมูลจาก Official Data Dictionary

### 📝 **ตารางข้อมูลครบถ้วนทุกตัวแปร (143 ตัวแปร)**

| Variable Name | Description |
|---------------|-------------|
| **acc_now_delinq** | The number of accounts on which the borrower is now delinquent. |
| **acc_open_past_24mths** | Number of trades opened in past 24 months. |
| **addr_state** | The state provided by the borrower in the loan application |
| **all_util** | Balance to credit limit on all trades |
| **annual_inc** | The self-reported annual income provided by the borrower during registration. |
| **annual_inc_joint** | The combined self-reported annual income provided by the co-borrowers during registration |
| **application_type** | Indicates whether the loan is an individual application or a joint application with two co-borrowers |
| **avg_cur_bal** | Average current balance of all accounts |
| **bc_open_to_buy** | Total open to buy on revolving bankcards. |
| **bc_util** | Ratio of total current balance to high credit/credit limit for all bankcard accounts. |
| **chargeoff_within_12_mths** | Number of charge-offs within 12 months |
| **collection_recovery_fee** | post charge off collection fee |
| **collections_12_mths_ex_med** | Number of collections in 12 months excluding medical collections |
| **delinq_2yrs** | The number of 30+ days past-due incidences of delinquency in the borrower's credit file for the past 2 years |
| **delinq_amnt** | The past-due amount owed for the accounts on which the borrower is now delinquent. |
| **desc** | Loan description provided by the borrower |
| **dti** | A ratio calculated using the borrower's total monthly debt payments on the total debt obligations, excluding mortgage and the requested LC loan, divided by the borrower's self-reported monthly income. |
| **dti_joint** | A ratio calculated using the co-borrowers' total monthly payments on the total debt obligations, excluding mortgages and the requested LC loan, divided by the co-borrowers' combined self-reported monthly income |
| **earliest_cr_line** | The month the borrower's earliest reported credit line was opened |
| **emp_length** | Employment length in years. Possible values are between 0 and 10 where 0 means less than one year and 10 means ten or more years. |
| **emp_title** | The job title supplied by the Borrower when applying for the loan.* |
| **fico_range_high** | The upper boundary range the borrower's FICO at loan origination belongs to. |
| **fico_range_low** | The lower boundary range the borrower's FICO at loan origination belongs to. |
| **funded_amnt** | The total amount committed to that loan at that point in time. |
| **funded_amnt_inv** | The total amount committed by investors for that loan at that point in time. |
| **grade** | LC assigned loan grade |
| **home_ownership** | The home ownership status provided by the borrower during registration or obtained from the credit report. Our values are: RENT, OWN, MORTGAGE, OTHER |
| **id** | A unique LC assigned ID for the loan listing. |
| **il_util** | Ratio of total current balance to high credit/credit limit on all install acct |
| **initial_list_status** | The initial listing status of the loan. Possible values are – W, F |
| **inq_fi** | Number of personal finance inquiries |
| **inq_last_12m** | Number of credit inquiries in past 12 months |
| **inq_last_6mths** | The number of inquiries in past 6 months (excluding auto and mortgage inquiries) |
| **installment** | The monthly payment owed by the borrower if the loan originates. |
| **int_rate** | Interest Rate on the loan |
| **issue_d** | The month which the loan was funded |
| **last_credit_pull_d** | The most recent month LC pulled credit for this loan |
| **last_fico_range_high** | The upper boundary range the borrower's last FICO pulled belongs to. |
| **last_fico_range_low** | The lower boundary range the borrower's last FICO pulled belongs to. |
| **last_pymnt_amnt** | Last total payment amount received |
| **last_pymnt_d** | Last month payment was received |
| **loan_amnt** | The listed amount of the loan applied for by the borrower. If at some point in time, the credit department reduces the loan amount, then it will be reflected in this value. |
| **loan_status** | Current status of the loan |
| **max_bal_bc** | Maximum current balance owed on all revolving accounts |
| **member_id** | A unique LC assigned Id for the borrower member. |
| **mo_sin_old_il_acct** | Months since oldest bank installment account opened |
| **mo_sin_old_rev_tl_op** | Months since oldest revolving account opened |
| **mo_sin_rcnt_rev_tl_op** | Months since most recent revolving account opened |
| **mo_sin_rcnt_tl** | Months since most recent account opened |
| **mort_acc** | Number of mortgage accounts. |
| **mths_since_last_delinq** | The number of months since the borrower's last delinquency. |
| **mths_since_last_major_derog** | Months since most recent 90-day or worse rating |
| **mths_since_last_record** | The number of months since the last public record. |
| **mths_since_rcnt_il** | Months since most recent installment accounts opened |
| **mths_since_recent_bc** | Months since most recent bankcard account opened. |
| **mths_since_recent_bc_dlq** | Months since most recent bankcard delinquency |
| **mths_since_recent_inq** | Months since most recent inquiry. |
| **mths_since_recent_revol_delinq** | Months since most recent revolving delinquency. |
| **next_pymnt_d** | Next scheduled payment date |
| **num_accts_ever_120_pd** | Number of accounts ever 120 or more days past due |
| **num_actv_bc_tl** | Number of currently active bankcard accounts |
| **num_actv_rev_tl** | Number of currently active revolving trades |
| **num_bc_sats** | Number of satisfactory bankcard accounts |
| **num_bc_tl** | Number of bankcard accounts |
| **num_il_tl** | Number of installment accounts |
| **num_op_rev_tl** | Number of open revolving accounts |
| **num_rev_accts** | Number of revolving accounts |
| **num_rev_tl_bal_gt_0** | Number of revolving trades with balance >0 |
| **num_sats** | Number of satisfactory accounts |
| **num_tl_120dpd_2m** | Number of accounts currently 120 days past due (updated in past 2 months) |
| **num_tl_30dpd** | Number of accounts currently 30 days past due (updated in past 2 months) |
| **num_tl_90g_dpd_24m** | Number of accounts 90 or more days past due in last 24 months |
| **num_tl_op_past_12m** | Number of accounts opened in past 12 months |
| **open_acc** | The number of open credit lines in the borrower's credit file. |
| **open_acc_6m** | Number of open trades in last 6 months |
| **open_il_12m** | Number of installment accounts opened in past 12 months |
| **open_il_24m** | Number of installment accounts opened in past 24 months |
| **open_act_il** | Number of currently active installment trades |
| **open_rv_12m** | Number of revolving trades opened in past 12 months |
| **open_rv_24m** | Number of revolving trades opened in past 24 months |
| **out_prncp** | Remaining outstanding principal for total amount funded |
| **out_prncp_inv** | Remaining outstanding principal for portion of total amount funded by investors |
| **pct_tl_nvr_dlq** | Percent of trades never delinquent |
| **percent_bc_gt_75** | Percentage of all bankcard accounts > 75% of limit. |
| **policy_code** | publicly available policy_code=1 new products not publicly available policy_code=2 |
| **pub_rec** | Number of derogatory public records |
| **pub_rec_bankruptcies** | Number of public record bankruptcies |
| **purpose** | A category provided by the borrower for the loan request. |
| **pymnt_plan** | Indicates if a payment plan has been put in place for the loan |
| **recoveries** | post charge off gross recovery |
| **revol_bal** | Total credit revolving balance |
| **revol_util** | Revolving line utilization rate, or the amount of credit the borrower is using relative to all available revolving credit. |
| **sub_grade** | LC assigned loan subgrade |
| **tax_liens** | Number of tax liens |
| **term** | The number of payments on the loan. Values are in months and can be either 36 or 60. |
| **title** | The loan title provided by the borrower |
| **tot_coll_amt** | Total collection amounts ever owed |
| **tot_cur_bal** | Total current balance of all accounts |
| **tot_hi_cred_lim** | Total high credit/credit limit |
| **total_acc** | The total number of credit lines currently in the borrower's credit file |
| **total_bal_ex_mort** | Total credit balance excluding mortgage |
| **total_bal_il** | Total current balance of all installment accounts |
| **total_bc_limit** | Total bankcard high credit/credit limit |
| **total_cu_tl** | Number of finance trades |
| **total_il_high_credit_limit** | Total installment high credit/credit limit |
| **total_pymnt** | Payments received to date for total amount funded |
| **total_pymnt_inv** | Payments received to date for portion of total amount funded by investors |
| **total_rec_int** | Interest received to date |
| **total_rec_late_fee** | Late fees received to date |
| **total_rec_prncp** | Principal received to date |
| **total_rev_hi_lim** | Total revolving high credit/credit limit |
| **verification_status** | Indicates if income was verified by LC, not verified, or if the income source was verified |
| **verification_status_joint** | Indicates if the co-borrowers' joint income was verified by LC, not verified, or if the income source was verified |
| **zip_code** | The first 3 numbers of the zip code provided by the borrower in the loan application. |

### 🎯 **การจัดกลุ่มตัวแปรตามประเภท**

#### **📊 ข้อมูลสินเชื่อพื้นฐาน (Loan Basics)**
- `id`, `member_id`, `loan_amnt`, `funded_amnt`, `funded_amnt_inv`
- `term`, `int_rate`, `installment`, `grade`, `sub_grade`
- `issue_d`, `loan_status`, `pymnt_plan`, `purpose`, `title`, `desc`

#### **👤 ข้อมูลผู้กู้ (Borrower Information)**
- `emp_title`, `emp_length`, `home_ownership`, `annual_inc`
- `verification_status`, `zip_code`, `addr_state`
- `dti`, `application_type`

#### **💳 ประวัติเครดิต (Credit History)**
- `fico_range_low`, `fico_range_high`, `last_fico_range_low`, `last_fico_range_high`
- `earliest_cr_line`, `delinq_2yrs`, `delinq_amnt`
- `inq_last_6mths`, `inq_last_12m`, `inq_fi`
- `open_acc`, `pub_rec`, `pub_rec_bankruptcies`
- `revol_bal`, `revol_util`, `total_acc`

#### **💰 ข้อมูลการชำระ (Payment Information)**
- `out_prncp`, `out_prncp_inv`, `total_pymnt`, `total_pymnt_inv`
- `total_rec_prncp`, `total_rec_int`, `total_rec_late_fee`
- `recoveries`, `collection_recovery_fee`
- `last_pymnt_d`, `last_pymnt_amnt`, `next_pymnt_d`

#### **📈 ข้อมูลเครดิตรายละเอียด (Detailed Credit Info)**
- `mths_since_last_delinq`, `mths_since_last_major_derog`, `mths_since_last_record`
- `acc_now_delinq`, `acc_open_past_24mths`, `chargeoff_within_12_mths`
- `collections_12_mths_ex_med`, `mort_acc`, `tax_liens`

#### **🏦 ข้อมูลบัญชีเครดิต (Credit Account Details)**
- `bc_open_to_buy`, `bc_util`, `percent_bc_gt_75`
- `num_actv_bc_tl`, `num_bc_sats`, `num_bc_tl`
- `num_actv_rev_tl`, `num_op_rev_tl`, `num_rev_accts`, `num_rev_tl_bal_gt_0`
- `num_il_tl`, `open_act_il`, `il_util`

#### **📊 ตัวชี้วัดเพิ่มเติม (Additional Metrics)**
- `avg_cur_bal`, `max_bal_bc`, `tot_cur_bal`, `tot_hi_cred_lim`
- `total_bal_ex_mort`, `total_bal_il`, `total_bc_limit`
- `total_il_high_credit_limit`, `total_rev_hi_lim`
- `pct_tl_nvr_dlq`, `all_util`

### 🚨 **ตัวแปรที่ต้องระวัง Data Leakage**

#### **🔮 Future Information (ห้ามใช้ในการพยากรณ์)**
```python
future_leakage_vars = [
    'total_pymnt', 'total_pymnt_inv', 'total_rec_prncp', 'total_rec_int',
    'total_rec_late_fee', 'recoveries', 'collection_recovery_fee',
    'last_pymnt_d', 'last_pymnt_amnt', 'next_pymnt_d',
    'last_fico_range_low', 'last_fico_range_high', 'last_credit_pull_d',
    'out_prncp', 'out_prncp_inv'
]
```

#### **⚠️ Potential Target Leakage**
```python
potential_leakage_vars = [
    'policy_code',  # อาจเกี่ยวข้องกับ loan approval
    'initial_list_status',  # สถานะเริ่มต้นอาจสะท้อน quality
    'funded_amnt_inv',  # ความต้องการของนักลงทุนอาจสะท้อน risk
]
```

### 🎯 **ตัวแปรหลักสำหรับ Machine Learning**

#### **⭐ Tier 1: Critical Features**
```python
critical_features = [
    'grade', 'sub_grade',           # LC's own risk assessment
    'fico_range_low', 'fico_range_high',  # Credit score
    'dti',                          # Debt-to-income ratio
    'annual_inc',                   # Income capacity
    'loan_amnt',                    # Loan size
    'int_rate',                     # Risk pricing
    'term'                          # Loan duration
]
```

#### **⭐ Tier 2: Important Features**
```python
important_features = [
    'revol_util',                   # Credit utilization
    'delinq_2yrs',                  # Recent delinquencies
    'inq_last_6mths',               # Recent credit inquiries
    'emp_length',                   # Employment stability
    'home_ownership',               # Housing stability
    'purpose',                      # Loan purpose
    'open_acc',                     # Number of open accounts
    'pub_rec'                       # Public records
]
```

#### **⭐ Tier 3: Supplementary Features**
```python
supplementary_features = [
    'earliest_cr_line',             # Credit history length
    'total_acc',                    # Total credit accounts
    'mort_acc',                     # Mortgage accounts
    'verification_status',          # Income verification
    'addr_state',                   # Geographic location
    'installment',                  # Monthly payment
    'mths_since_last_delinq'        # Time since last delinquency
]
```

### 💡 **เคล็ดลับการใช้งาน Data Dictionary**

#### **1. การตรวจสอบความสมบูรณ์ของข้อมูล**
```python
def check_data_completeness(df, data_dict_vars):
    """
    ตรวจสอบว่าข้อมูลมีตัวแปรครบตาม Data Dictionary หรือไม่
    """
    missing_vars = []
    extra_vars = []
    
    # ตัวแปรที่หายไป
    for var in data_dict_vars:
        if var not in df.columns:
            missing_vars.append(var)
    
    # ตัวแปรเพิ่มเติม
    for var in df.columns:
        if var not in data_dict_vars:
            extra_vars.append(var)
    
    print(f"Missing variables: {len(missing_vars)}")
    print(f"Extra variables: {len(extra_vars)}")
    
    return missing_vars, extra_vars
```

#### **2. การสร้าง Feature Groups**
```python
def create_feature_groups():
    """
    สร้างกลุ่มตัวแปรตาม Data Dictionary
    """
    feature_groups = {
        'loan_basics': [
            'loan_amnt', 'funded_amnt', 'term', 'int_rate', 
            'installment', 'grade', 'sub_grade'
        ],
        'borrower_profile': [
            'annual_inc', 'emp_length', 'home_ownership', 
            'verification_status', 'dti', 'purpose'
        ],
        'credit_score': [
            'fico_range_low', 'fico_range_high'
        ],
        'credit_history': [
            'earliest_cr_line', 'delinq_2yrs', 'inq_last_6mths',
            'open_acc', 'pub_rec', 'revol_bal', 'revol_util', 'total_acc'
        ],
        'geographic': [
            'addr_state', 'zip_code'
        ]
    }
    
    return feature_groups
```

#### **3. การวิเคราะห์ Missing Values ตาม Data Dictionary**
```python
def analyze_missing_by_category(df):
    """
    วิเคราะห์ Missing Values ตามหมวดหมู่ของตัวแปร
    """
    feature_groups = create_feature_groups()
    
    missing_analysis = {}
    
    for group_name, features in feature_groups.items():
        available_features = [f for f in features if f in df.columns]
        
        if available_features:
            missing_pct = (df[available_features].isnull().sum() / len(df)) * 100
            missing_analysis[group_name] = missing_pct.sort_values(ascending=False)
    
    return missing_analysis
```

---

*📖 หมายเหตุ: Data Dictionary นี้มาจากข้อมูลอย่างเป็นทางการของ Lending Club และครอบคลุมตัวแปรทั้งหมด 143 ตัว ที่อาจจะพบในไฟล์ข้อมูลจริง ตัวแปรบางตัวอาจไม่มีในไฟล์ข้อมูลที่ใช้ในหลักสูตรเนื่องจากการ preprocessing หรือ privacy considerations*

---
