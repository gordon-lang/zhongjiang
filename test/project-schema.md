# 项目全生命周期报告 Schema

本文档定义了项目全生命周期情况报告的七大模块及其对应的数据库字段映射关系。

## 报告结构总览

项目全生命周期情况报告严格按照以下七大项输出，不得扩展或增减：

1. 一、项目概况
2. 二、立项、可研与概算情况
3. 三、招标采购
4. 四、工程进度
5. 五、合同执行情况
6. 六、请款支付与资金情况
7. 七、项目风险预警

---

## 一、项目概况

### 1.1 项目基本信息

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 工程名称 | fp_project_basic | project_name | 项目名称 |
| 工程编号 | fp_project_basic | project_code | 项目编码 |
| 工程类别 | fp_project_basic | engineering_type_value | 01=集中项目，02=其他项目 |
| 建设性质 | fp_project_basic | item_name | 建设性质 |
| 所属市级 | fp_city_and_county | district | 通过 fp_project_basic.pro_city_value 关联 fp_city_and_county.district_id |
| 所属县级 | fp_city_and_county | district | 通过 fp_project_basic.pro_county_value 关联 fp_city_and_county.district_id |
| 工程地址 | fp_project_basic | project_address | 工程地址 |
| 前期负责人 | fp_project_basic | pre_project_manager | 前期负责人 |
| 施工负责人 | fp_project_basic | construction_manager | 施工负责人 |
| 重点工程 | fp_project_basic | important_project | 0=是，1=否 |
| 重点工程类别 | fp_project_basic | engineering_type_value | 01=集中建设项目，02=其他项目 |
| 项目类型 | fp_sys_dic_item | item_name | 通过 fp_project_basic.project_type_value 关联 fp_sys_dic_item.item_value |
| 建设规模 | fp_project_basic | construct_scale | 建设规模 |
| 到位资金（万元） | fp_project_basic | place_amount | 到位资金 |
| 工程分类 | fp_sys_dic_item | item_name | 通过 fp_project_basic.engineering_classification_value 关联 fp_sys_dic_item.item_value |

### 关联表说明
- **主表**：fp_project_basic（项目基础信息表）
- **关联表**：
  - fp_city_and_county（市县字典表）
  - fp_sys_dic_item（系统字典表）

---

## 二、立项、可研与概算情况

### 2.1 项目建议书

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 项目代码 | fp_project_basic | project_code | 通过project_code关联 |
| 项目建议书批复总投资（万元） | fp_ct_project_advice | project_advice_approval_total | 项目建议书批复总投资 |
| 资金性质 | fp_ct_project_advice | capital_nature | 资金性质 |
| 资金来源 | fp_ct_project_advice | source_funds | 资金来源 |
| 项目建议书批复建筑总面积 | fp_ct_project_advice | all_area | 建筑总面积 |
| 项目建议书批复建筑地下面积 | fp_ct_project_advice | under_ground_area | 地下面积 |
| 项目建议书批复建筑地上面积 | fp_ct_project_advice | above_ground_area | 地上面积 |
| 使用单位 | fp_ct_project_advice | under_ground_area | 使用单位 |
| 项目建议书批复文号 | fp_ct_project_advice | approval_no | 批复文号 |
| 项目建议书批复日期 | fp_ct_project_advice | approval_date | 批复日期 |

### 2.2 可行性研究

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 可行性研究工程费用（万元） | fp_ct_feasibility_study | project_cost | 工程费用 |
| 可行性研究工程建设其他费用（万元） | fp_ct_feasibility_study | project_building_other_cost | 工程建设其他费用 |
| 可行性研究预备费（万元） | fp_ct_feasibility_study | reserve_funds | 预备费 |
| 可行性研究估算总投资（万元） | fp_ct_feasibility_study | estimate_all_investment | 估算总投资 |
| 可研批复地上建筑面积 | fp_ct_feasibility_study | above_ground_area | 地上建筑面积 |
| 可研批复地下建筑面积 | fp_ct_feasibility_study | under_ground_area | 地下建筑面积 |
| 可研批复总建筑面积 | fp_ct_feasibility_study | all_area | 总建筑面积 |
| 批复文号 | fp_ct_feasibility_study | approval_document_number | 批复文号 |
| 批复日期 | fp_ct_feasibility_study | approval_time | 批复日期 |

### 2.3 初步设计（报批报审）

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 初步设计工程费用（万元） | fp_prospect_begin_design | project_fee | 工程费用 |
| 初步设计工程建设其他费用（万元） | fp_prospect_begin_design | project_build_other_fee | 工程建设其他费用 |
| 初步设计预备费（万元） | fp_prospect_begin_design | project_prepare_fee | 预备费 |
| 初步设计概算总投资（万元） | fp_prospect_begin_design | budgetary_total_invest | 概算总投资 |
| 初步设计建筑面积地上 | fp_prospect_begin_design | begin_overground_area | 地上建筑面积 |
| 初步设计建筑面积地下 | fp_prospect_begin_design | begin_underground_area | 地下建筑面积 |
| 初步设计批复文号 | fp_prospect_begin_design | approval_document_number | 批复文号 |
| 初步设计批复日期 | fp_prospect_begin_design | approval_time | 批复日期 |

### 关联表说明
- **主表**：
  - fp_ct_project_advice（项目建议书表）
  - fp_ct_feasibility_study（可行性研究表）
  - fp_prospect_begin_design（初步设计表）
- **关联方式**：通过 project_code 与 fp_project_basic 关联

---

## 三、招标采购

### 3.1 招标公告

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 招标项目名称 | fp_bg_bidding_notice | bidding_project_name | 招标项目名称 |
| 招标项目编号 | fp_bg_bidding_notice | bidding_project_code | 招标项目编号 |

### 3.2 招标结果

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 招标结果 | fp_sys_dic_item | item_name | 通过 fp_bg_bidding_result.bidding_result_value 关联 fp_sys_dic_item.item_value，且 dictionary_type='tender_status' |
| 中标单位 | fp_bg_bidding_result | win_unit | 中标单位 |

### 关联表说明
- **主表**：
  - fp_bg_bidding_notice（招标公告表）
  - fp_bg_bidding_result（招标结果表）
- **关联表**：
  - bg_bidding_document（招标文件表）- 通过 document_id 关联
  - fp_sys_dic_item（系统字典表）- 招标状态字典
- **关联方式**：通过 project_code 与 fp_project_basic 关联

---

## 四、工程进度

### 4.1 数据来源

- **来源**：泛微PDF附件（工程月报知识库）
- **取值方式**：取知识库中工程月报的当月工作完成情况，以及下月工作计划
- **关键节点**：如有关键节点信息，需提取关键节点信息

### 4.2 输出内容

- 当月工作完成情况
- 下月工作安排
- 关键节点信息（如有）

---

## 五、合同执行情况

### 5.1 合同签订

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 工程名称 | fp_project_basic | project_name | 项目名称 |
| 合同编码/费用编号 | fp_contract | contract_code | 合同编码 |
| 合同名称/费用名称 | fp_contract | contract_name | 合同名称 |
| 合同类型 | fp_sys_dic_item | item_name | 通过 fp_contract.contract_type_value 关联 fp_sys_dic_item.item_value，且 dictionary_type='contract_type' |
| 甲方单位 | - | - | 常量值：江苏省公建中心 |
| 乙方单位 | fp_project_business_unit | name | 通过 fp_contract.second_unit 关联 fp_project_business_unit 主键id |
| 签订时间 | fp_contract | signing_time | 签订时间 |
| 合同金额/费用金额(元) | fp_contract | contract_amount | 合同金额 |

### 5.2 合同执行情况

| 字段名称 | 计算方式 | 说明 |
|---------|---------|------|
| 合同累计变更金额(元) | SUM(fp_contract_modify.change_amount) | 条件：verify_status_value='03'（审核通过），deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 变更后金额(元) | 合同金额 + 合同累计变更金额 | 计算得出 |
| 累计合同完成量(元) | SUM(fp_contract_complete.contract_total_complete) | 条件：verify_status_value='03'（审核通过），deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 剩余完成量(元) | 变更后金额 - 累计合同完成量 | 计算得出 |
| 累计完成比例 | 累计合同完成量 / 变更后金额 | 计算得出 |
| 累计付款(元) | SUM(fp_finance_payment.payment_amount) | 条件：verify_status_value='03'（审核通过），deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 合同结算金额(元) | SUM(fp_contract_settlement.authorize_amount) | 条件：verify_status_value='03'（审核通过），deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 累计付款比例(合同额) | 累计付款 / 变更后金额 | 计算得出 |
| 累计付款比例(合同完成量) | 累计付款 / 累计合同完成量 | 计算得出 |
| 累计收票(元) | SUM(fp_finance_bill.face_amount) | 条件：collection_status_value='02'，deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |

### 关联表说明
- **主表**：fp_contract（合同表）
- **子表**：fp_contract_info_ex（合同子表）
- **关联表**：
  - fp_project_basic（项目表）- 通过 project_code 关联
  - fp_project_business_unit（业务单位表）- 乙方单位
  - fp_sys_dic_item（系统字典表）- 合同类型字典
  - fp_contract_modify（合同变更表）
  - fp_contract_complete（合同完成量表）
  - fp_finance_payment（财务付款表）
  - fp_contract_settlement（合同结算表）
  - fp_finance_bill（财务票据表）

---

## 六、请款支付与资金情况

### 6.1 工程请款

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 请款编号 | fp_contract_request | code | 请款编号 |
| 请款金额(元) | fp_contract_request | request_amount | 请款金额 |
| 审核金额(元) | fp_contract_request | ask_account | 审核金额 |
| 请款日期 | fp_contract_request | request_date | 请款日期 |
| 已付款金额 | fp_contract_request | paid_amount | 已付款金额 |

### 6.2 监管账号付款

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 监管账号付款-累计付款 | fp_regulatory_payment_ex | accumulated_payment_amount | 累计求和；通过 fp_regulatory_payment 主键id 关联 fp_regulatory_payment_ex.rid |
| 监管账号付款-分包合同余额 | fp_regulatory_payment_ex | subcontract_balance | 分包合同余额；通过 fp_regulatory_payment 主键id 关联 fp_regulatory_payment_ex.rid |

### 6.3 资金到位

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 资金到位金额 | fp_finance_funds | place_amount | 累计求和；条件：verify_status_value='03'（审核通过），deleted=0；通过 project_code 关联 |

### 关联表说明
- **主表**：
  - fp_contract_request（工程请款表）
  - fp_regulatory_payment（监管账号付款表）
  - fp_regulatory_payment_ex（监管账户付款子业务表）
  - fp_finance_funds（资金到位表）
- **关联方式**：
  - 请款表通过 contract_code 与 fp_contract 关联
  - 监管账号付款通过 contract_code 与 fp_contract 关联
  - 资金到位表通过 project_code 与 fp_project_basic 关联

---

## 七、项目风险预警

### 7.1 预警消息

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 预警消息 | fp_warning_project_info | title_content | 预警消息标题内容；通过 project_code 与 fp_project_basic 关联 |

### 7.2 风险预警规则

#### 支付风险
1. **规则一**：累计【资金到位金额】 <= 该项目的累计合同完成量，产生预警消息
2. **规则二**：累计【资金到位金额】 < 该项目已支付金额 + 申请的金额，产生预警消息

#### 工程进度风险
- 数据来源：fp_warning_project_info 表
- 关联方式：通过 project_code 与 fp_project_basic 关联

### 关联表说明
- **主表**：fp_warning_project_info（预警消息表）
- **关联方式**：通过 project_code 与 fp_project_basic 关联

---

## 核心关联表汇总

| 表名 | 表说明 | 主键/关联字段 |
|-----|--------|--------------|
| fp_project_basic | 项目基础信息表 | project_code |
| fp_sys_dic_item | 系统字典表 | item_value, dictionary_type |
| fp_city_and_county | 市县字典表 | district_id |
| fp_ct_project_advice | 项目建议书表 | project_code |
| fp_ct_feasibility_study | 可行性研究表 | project_code |
| fp_prospect_begin_design | 初步设计表 | project_code |
| fp_bg_bidding_notice | 招标公告表 | project_code, document_id |
| fp_bg_bidding_result | 招标结果表 | project_code, document_id |
| fp_contract | 合同表 | contract_code, project_code |
| fp_contract_modify | 合同变更表 | contract_code |
| fp_contract_complete | 合同完成量表 | contract_code |
| fp_finance_payment | 财务付款表 | contract_code |
| fp_contract_settlement | 合同结算表 | contract_code |
| fp_finance_bill | 财务票据表 | contract_code |
| fp_contract_request | 工程请款表 | contract_code |
| fp_regulatory_payment | 监管账号付款表 | contract_code, id |
| fp_regulatory_payment_ex | 监管账户付款子表 | rid |
| fp_finance_funds | 资金到位表 | project_code |
| fp_warning_project_info | 预警消息表 | project_code |
| fp_project_business_unit | 业务单位表 | id |
| bg_bidding_document | 招标文件表 | id |
