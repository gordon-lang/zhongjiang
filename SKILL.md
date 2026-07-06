---
name: project-lifecycle-report
description: 项目全生命周期情况报告生成技能。用于按项目名称或项目编号查询并生成完整的七大模块报告，覆盖项目概况、立项可研与概算、招标采购、工程进度、合同执行、请款支付与资金、项目风险预警。输出必须严格遵循既定结构、字段顺序、关联关系和取值口径。
---

# 项目全生命周期情况报告

## 技能目标

本技能用于根据用户提供的项目名称或项目编号，生成《项目全生命周期情况报告》完整正文。报告必须严格按照七大模块顺序输出，不得增减模块、不得调整顺序、不得扩展 schema 之外的字段。

## 适用场景

- 查询单个项目的全生命周期情况
- 生成标准化项目情况报告
- 按指定项目名称或项目编号输出完整七大模块信息

## 输入要求

生成报告前，必须先确认项目标识。优先使用以下任一信息：
- 项目名称
- 项目编号

若用户提供的信息不足以唯一识别项目，必须先询问用户，不得自行猜测。

## 工具使用总则

1. 每个章节输出前，必须先调用对应工具完成检索。
2. 结构化字段优先使用 `sqlagent`。
3. 工程月报、项目建设背景相关材料、预警说明等非结构化内容优先使用知识库查询工具。
4. `sqlagent` 必须使用自然语言精确描述查询需求，并将该描述作为参数 `query` 的值。
5. 若存在多条查询需求，应拆分为多次工具调用，不要把多类问题混成一条查询。
6. 若首次检索为空，不要直接写“暂无数据”；必须先核对项目标识、字段口径、关联关系、筛选条件后补查一次。
7. 只有在完成检索且补查后仍无结果时，才允许在对应字段或对应小节写“暂无数据”。
8. 工具检索仅用于内部取数，不能把查询语句、报错信息、SQL 回显、英文说明或工具过程信息带入报告正文。

## 总输出规则

1. 只输出最终报告正文，不输出思考过程、分析过程、检索过程、草稿、备选方案、免责声明、生成说明或工具回显。
2. 全程只使用中文输出和沟通，不输出英文句子、SQL 语句、代码块、调试信息。
3. 使用 Markdown 格式输出，标题、分级标题和表格均按 Markdown 语法组织。
4. 不输出分隔线、括号中的自我说明、内部判断或过程性话术。
5. 报告必须是单篇完整文档，不得夹带“已查询完毕”“现在开始生成”等过程性语句。
6. 不得扩展 Excel 表格中未列出的字段。
7. 必须保持原始字段名称和顺序。
8. 金额类字段保留两位小数，并注明单位；比例类字段以百分比形式展示，保留两位小数。
9. 多条记录按表格逐行展示；涉及多合同时，应按合同逐条列出。
10. 允许归纳的内容仅限以下三类：
   - 1.2 项目建设背景与项目建设目的与意义
   - 四、工程进度
   - 七、项目风险预警中的规则说明
11. 除上述允许归纳的部分外，其余内容只依据已检索到的数据输出，不得补写推断内容。

## 执行流程

### 1. 确定项目标识

优先使用用户给出的项目名称或项目编号作为检索依据。若无法唯一定位，先向用户确认。

### 2. 按七大模块依次检索

必须按以下顺序处理并输出：
1. 一、项目概况
2. 二、立项、可研与概算情况
3. 三、招标采购
4. 四、工程进度
5. 五、合同执行情况
6. 六、请款支付与资金情况
7. 七、项目风险预警

### 3. 生成报告正文

将各模块按固定标题层级和字段顺序组织成一份完整 Markdown 报告。

## 固定报告结构

最终报告必须严格按以下顺序输出：

# 项目全生命周期情况报告

## 一、项目概况
### 1.1 项目基本信息
### 1.2 项目建设背景与项目建设目的与意义

## 二、立项、可研与概算情况
### 2.1 项目建议书
### 2.2 可行性研究
### 2.3 初步设计（报批报审）

## 三、招标采购
### 3.1 招标公告
### 3.2 招标结果

## 四、工程进度
### 当月工作完成情况
### 下月工作安排
### 关键节点信息

## 五、合同执行情况
### 5.1 合同签订
### 5.2 合同执行情况

## 六、请款支付与资金情况
### 6.1 工程请款
### 6.2 监管账号付款
### 6.3 资金到位

## 七、项目风险预警
### 7.1 预警消息
### 7.2 风险预警规则

## 各模块字段、关联关系与输出要求

## 一、项目概况

### 取数规则
- 主表：`fp_project_basic`
- 关联表：`fp_city_and_county`、`fp_sys_dic_item`
- 关键关联键：
  - `fp_project_basic.pro_city_value -> fp_city_and_county.district_id`
  - `fp_project_basic.pro_county_value -> fp_city_and_county.district_id`
  - `fp_project_basic.project_type_value -> fp_sys_dic_item.item_value`
  - `fp_project_basic.engineering_classification_value -> fp_sys_dic_item.item_value`
- 取数顺序：先取 1.1 结构化基本信息，再结合项目建议书、可行性研究、初步设计等事实补写 1.2。
- 1.2 没有独立数据库字段，必须基于已取到的项目事实做有限概括，不要因为缺少专门背景原文就直接判空。
- 项目概况的结构化字段优先用 `sqlagent`，1.2 优先用知识库查询工具。

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

### 项目概况专项要求
- 本模块只输出第一部分内容。
- 项目基本信息按表格形式输出。
- 项目建设背景与项目建设目的与意义优先基于项目基本信息和前期批复事实进行归纳；如相关事实仍不足以支撑，再写“暂无数据”。
- 不得增加额外模块或分析内容。

## 二、立项、可研与概算情况

### 取数规则
- 主表：`fp_ct_project_advice`、`fp_ct_feasibility_study`、`fp_prospect_begin_design`
- 关联表：`fp_project_basic`
- 关键关联键：三张前期咨询表均通过 `project_code` 关联项目主表。
- 取数顺序：先项目建议书，再可行性研究，最后初步设计；每段单独成表，字段按 schema 顺序输出。
- 结构化批复信息优先用 `sqlagent`。

### 2.1 项目建议书

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 项目代码 | fp_project_basic | project_code | 通过 project_code 关联 |
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

### 本模块专项要求
- 本模块只输出第二部分内容。
- 项目建议书、可行性研究、初步设计内容均按照表格形式输出。
- 若首次检索为空，不要直接写“暂无数据”，先核对项目代码、批复表和字段口径后补查一次。
- 不得增加额外模块或分析内容。

## 三、招标采购

### 取数规则
- 主表：`fp_bg_bidding_notice`、`fp_bg_bidding_result`
- 关联表：`fp_project_basic`、`bg_bidding_document`、`fp_sys_dic_item`
- 关键关联键：
  - `fp_bg_bidding_notice.project_code -> fp_project_basic.project_code`
  - `fp_bg_bidding_notice.document_id -> bg_bidding_document.id`
  - `fp_bg_bidding_result.project_code -> fp_project_basic.project_code`
  - `fp_bg_bidding_result.document_id -> bg_bidding_document.id`
  - `fp_bg_bidding_result.bidding_result_value -> fp_sys_dic_item.item_value`，且 `dictionary_type='tender_status'`
- 取数顺序：先公告后结果，结果必须带中标单位和招标状态字典值。
- 招标公告和招标结果优先用 `sqlagent`。

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

### 本模块专项要求
- 本模块只输出第三部分内容。
- 招标公告、招标结果内容按照表格形式输出。
- 若首次检索为空，不要直接写“暂无数据”，先核对项目代码、公告表、结果表和字典关联后补查一次。
- 不得增加额外模块或分析内容。

## 四、工程进度

### 取数规则
- 数据源：工程月报知识库（泛微 PDF 附件）
- 无结构化数据库主表，按知识库内容抽取。
- 取数重点：当月工作完成情况、下月工作安排、关键节点信息。
- 取数顺序：优先最新且与项目标识匹配的月报；只做归纳，不要逐字搬运。
- 输出前必须先调用知识库查询工具完成检索；工程进度不允许先写空值再补查。

### 知识库抽取字段

| 取数项 | 来源位置 | 说明 |
|---|---|---|
| 当月工作完成情况 | 工程月报正文 | 提炼本月已完成的设计、现场、进度、造价等工作 |
| 下月工作安排 | 工程月报正文 | 提炼下月计划、待办事项和目标节点 |
| 关键节点信息 | 工程月报正文/节点清单 | 如月报中明确出现里程碑、批复、开工、主体等节点，则一并提取 |
| 月报时间范围 | 月报标题/正文 | 以当月月报为准，优先选择与项目标识一致的最新月报 |

### 本模块专项要求
- 本模块只输出第四部分内容。
- 工程进度内容应围绕工程月报知识库提取，不得扩展其他无关内容。
- 工程进度内容应根据知识库材料进行简洁归纳，不要逐字搬运原文。
- 如无最新月报数据，应明确说明。
- 输出内容应保持简洁、客观、可直接用于报告正文。

## 五、合同执行情况

### 取数规则
- 主表：`fp_contract`
- 子表：`fp_contract_info_ex`
- 关联表：`fp_project_basic`、`fp_project_business_unit`、`fp_sys_dic_item`、`fp_contract_modify`、`fp_contract_complete`、`fp_finance_payment`、`fp_contract_settlement`、`fp_finance_bill`
- 关键关联键：
  - `fp_contract.project_code -> fp_project_basic.project_code`
  - `fp_contract.second_unit -> fp_project_business_unit.id`
  - `fp_contract.contract_type_value -> fp_sys_dic_item.item_value`，且 `dictionary_type='contract_type'`
  - 其余执行类表均通过 `contract_code` 关联合同主表
- 计算口径：变更、完成量、付款、结算、收票均按审核通过或已收状态汇总后再计算比例。
- 合同签订和执行数据优先用 `sqlagent`。

### 5.1 合同签订

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 工程名称 | fp_project_basic | project_name | 项目名称 |
| 合同编码/费用编号 | fp_contract | contract_code | 合同编码 |
| 合同名称/费用名称 | fp_contract | contract_name | 合同名称 |
| 合同类型 | fp_sys_dic_item | item_name | 通过 fp_contract.contract_type_value 关联 fp_sys_dic_item.item_value，且 dictionary_type='contract_type' |
| 甲方单位 | - | - | 常量值：江苏省公建中心 |
| 乙方单位 | fp_project_business_unit | name | 通过 fp_contract.second_unit 关联 fp_project_business_unit 主键 id |
| 签订时间 | fp_contract | signing_time | 签订时间 |
| 合同金额/费用金额(元) | fp_contract | contract_amount | 合同金额 |

### 5.2 合同执行情况

| 字段名称 | 计算方式 | 说明 |
|---------|---------|------|
| 合同累计变更金额(元) | SUM(fp_contract_modify.change_amount) | 条件：verify_status_value='03'，deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 变更后金额(元) | 合同金额 + 合同累计变更金额 | 计算得出 |
| 累计合同完成量(元) | SUM(fp_contract_complete.contract_total_complete) | 条件：verify_status_value='03'，deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 剩余完成量(元) | 变更后金额 - 累计合同完成量 | 计算得出 |
| 累计完成比例 | 累计合同完成量 / 变更后金额 | 计算得出 |
| 累计付款(元) | SUM(fp_finance_payment.payment_amount) | 条件：verify_status_value='03'，deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 合同结算金额(元) | SUM(fp_contract_settlement.authorize_amount) | 条件：verify_status_value='03'，deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |
| 累计付款比例(合同额) | 累计付款 / 变更后金额 | 计算得出 |
| 累计付款比例(合同完成量) | 累计付款 / 累计合同完成量 | 计算得出 |
| 累计收票(元) | SUM(fp_finance_bill.face_amount) | 条件：collection_status_value='02'，deleted=0；通过 contract_code 关联；四舍五入保留两位小数 |

### 本模块专项要求
- 本模块只输出第五部分内容。
- 合同执行情况可能包含多个合同，应按列表形式展示。
- 合同签订、执行情况内容按照表格形式输出。
- 金额类字段保留两位小数，注明单位（元）。
- 比例类字段以百分比形式展示，保留两位小数。
- 如某字段无数据，可标注“暂无数据”。

## 六、请款支付与资金情况

### 取数规则
- 主表：`fp_contract_request`、`fp_regulatory_payment`、`fp_regulatory_payment_ex`、`fp_finance_funds`
- 关联表：`fp_contract`、`fp_project_basic`
- 关键关联键：
  - `fp_contract_request.contract_code -> fp_contract.contract_code`
  - `fp_regulatory_payment.contract_code -> fp_contract.contract_code`
  - `fp_regulatory_payment_ex.rid -> fp_regulatory_payment.id`
  - `fp_finance_funds.project_code -> fp_project_basic.project_code`
- 取数顺序：先工程请款，再监管账号付款，最后资金到位；金额类字段统一按元输出。
- 请款、监管账号付款、资金到位优先用 `sqlagent`。

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
| 监管账号付款-累计付款 | fp_regulatory_payment_ex | accumulated_payment_amount | 累计求和；通过 fp_regulatory_payment 主键 id 关联 fp_regulatory_payment_ex.rid |
| 监管账号付款-分包合同余额 | fp_regulatory_payment_ex | subcontract_balance | 分包合同余额；通过 fp_regulatory_payment 主键 id 关联 fp_regulatory_payment_ex.rid |

### 6.3 资金到位

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 资金到位金额 | fp_finance_funds | place_amount | 累计求和；条件：verify_status_value='03'，deleted=0；通过 project_code 关联 |

### 本模块专项要求
- 本模块只输出第六部分内容。
- 请款支付与资金情况内容按照表格形式输出。
- 金额类字段保留两位小数，注明单位（元）。
- 如某字段无数据，可标注“暂无数据”。
- 不得增加额外模块或分析内容。

## 七、项目风险预警

### 取数规则
- 主表：`fp_warning_project_info`
- 关联表：`fp_project_basic`
- 关键关联键：`fp_warning_project_info.project_code -> fp_project_basic.project_code`
- 取数顺序：先预警消息，再风险预警规则；规则以材料已有逻辑为准，不新增判断口径。
- 预警消息和风险规则优先用知识库查询工具，必要时可结合 `sqlagent` 复核关联字段。

### 7.1 预警消息

| 字段名称 | 数据库表 | 字段名 | 说明 |
|---------|---------|--------|------|
| 预警消息 | fp_warning_project_info | title_content | 预警消息标题内容；通过 project_code 与 fp_project_basic 关联 |

### 7.2 风险预警规则

#### 支付风险
1. 累计【资金到位金额】小于等于该项目的累计合同完成量，产生预警消息。
2. 累计【资金到位金额】小于该项目已支付金额与申请金额之和，产生预警消息。

#### 工程进度风险
- 数据来源：`fp_warning_project_info` 表
- 关联方式：通过 `project_code` 与 `fp_project_basic` 关联

### 本模块专项要求
- 本模块只输出第七部分内容。
- 预警消息内容和风险规则需分别展示。
- 如首次检索为空，不要直接写“暂无数据”，先核对预警表、项目标识和规则范围后补查一次。
- 风险规则仅输出 schema 或材料中已有逻辑，不新增规则、不扩展解释。

## 关键关联关系速查

- `fp_project_basic.pro_city_value -> fp_city_and_county.district_id`
- `fp_project_basic.pro_county_value -> fp_city_and_county.district_id`
- `fp_project_basic.project_type_value -> fp_sys_dic_item.item_value`
- `fp_project_basic.engineering_classification_value -> fp_sys_dic_item.item_value`
- `fp_ct_project_advice.project_code -> fp_project_basic.project_code`
- `fp_ct_feasibility_study.project_code -> fp_project_basic.project_code`
- `fp_prospect_begin_design.project_code -> fp_project_basic.project_code`
- `fp_bg_bidding_notice.project_code -> fp_project_basic.project_code`
- `fp_bg_bidding_notice.document_id -> bg_bidding_document.id`
- `fp_bg_bidding_result.project_code -> fp_project_basic.project_code`
- `fp_bg_bidding_result.document_id -> bg_bidding_document.id`
- `fp_bg_bidding_result.bidding_result_value -> fp_sys_dic_item.item_value`，且 `dictionary_type='tender_status'`
- `fp_contract.project_code -> fp_project_basic.project_code`
- `fp_contract.second_unit -> fp_project_business_unit.id`
- `fp_contract.contract_type_value -> fp_sys_dic_item.item_value`，且 `dictionary_type='contract_type'`
- `fp_contract_modify.contract_code -> fp_contract.contract_code`
- `fp_contract_complete.contract_code -> fp_contract.contract_code`
- `fp_finance_payment.contract_code -> fp_contract.contract_code`
- `fp_contract_settlement.contract_code -> fp_contract.contract_code`
- `fp_finance_bill.contract_code -> fp_contract.contract_code`
- `fp_contract_request.contract_code -> fp_contract.contract_code`
- `fp_regulatory_payment.contract_code -> fp_contract.contract_code`
- `fp_regulatory_payment_ex.rid -> fp_regulatory_payment.id`
- `fp_finance_funds.project_code -> fp_project_basic.project_code`
- `fp_warning_project_info.project_code -> fp_project_basic.project_code`

## 最终提醒

- 报告必须严格按七大模块输出，不得增减、不得改顺序。
- 字段名称、字段顺序、关联关系、计算口径必须与本技能文档保持一致。
- 在没有检索结果支撑的情况下，不得补写事实性内容。
- 若某字段或某小节最终确无数据，可在对应位置写“暂无数据”，但必须先完成检索与补查。