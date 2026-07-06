---
name: project-lifecycle-report-part1
description: 项目全生命周期情况报告第一板块测试版技能。仅用于按项目名称或项目编号查询并生成第一部分“项目概况”，便于先验证项目基础信息、关联关系、背景与意义归纳是否准确。
---

# 项目全生命周期情况报告（第一板块测试版）

## 技能目标

本技能仅用于生成《项目全生命周期情况报告》的第一部分：项目概况。

本测试版的目标是优先验证以下内容是否准确：
- 项目标识是否能被正确识别
- 项目基本信息字段是否能被准确查询
- 市县、字典等关联关系是否正确
- 项目建设背景与项目建设目的与意义的归纳是否符合预期
- 缺失数据时的补查逻辑是否稳定

## 适用场景

- 仅测试第一板块数据是否准确
- 按项目名称或项目编号生成项目概况
- 验证数据库字段映射、关联关系和背景归纳口径

## 输入要求

生成内容前，必须先确认项目标识。优先使用以下任一信息：
- 项目名称
- 项目编号

若用户提供的信息不足以唯一识别项目，必须先询问用户，不得自行猜测。

## 工具使用规则

1. 输出前必须先调用对应工具完成检索。
2. 结构化字段优先使用 `sqlagent`。
3. 项目建设背景与项目建设目的与意义优先使用知识库查询工具；如知识库材料不足，可结合已检索到的项目基本信息、项目建议书、可行性研究、初步设计等事实做有限归纳。
4. `sqlagent` 必须使用自然语言精确描述查询需求，并将该描述作为参数 `query` 的值。
5. 如果存在多条查询需求，应拆分为多次工具调用，不要把多类问题混成一条查询。
6. 若首次检索为空，不要直接写“暂无数据”；必须先核对项目标识、字段口径、关联关系和筛选条件后补查一次。
7. 只有在完成检索且补查后仍无结果时，才允许在对应字段或对应小节写“暂无数据”。
8. 工具检索仅用于内部取数，不能把查询语句、报错信息、SQL 回显、英文说明或工具过程信息带入报告正文。

## 总输出规则

1. 只输出最终报告正文，不输出思考过程、分析过程、检索过程、草稿、备选方案、免责声明、生成说明或工具回显。
2. 全程只使用中文输出和沟通，不输出英文句子、SQL 语句、代码块、调试信息。
3. 使用 Markdown 格式输出，标题、分级标题和表格均按 Markdown 语法组织。
4. 不输出分隔线、括号中的自我说明、内部判断或过程性话术。
5. 只输出第一部分内容，不得提前输出第二至第七部分。
6. 不得扩展 schema 中未列出的字段。
7. 必须保持原始字段名称和顺序。
8. 只依据已检索到的数据输出；项目建设背景与项目建设目的与意义允许基于已检索事实做有限概括，但不得常识性补写。

## 固定输出结构

最终输出必须严格按以下顺序组织：

# 项目全生命周期情况报告

## 一、项目概况
### 1.1 项目基本信息
### 1.2 项目建设背景与项目建设目的与意义

## 第一板块取数规则

### 数据模型概览
- 主表：`fp_project_basic`
- 关联表：`fp_city_and_county`、`fp_sys_dic_item`
- 关键关联键：
  - `fp_project_basic.pro_city_value -> fp_city_and_county.district_id`
  - `fp_project_basic.pro_county_value -> fp_city_and_county.district_id`
  - `fp_project_basic.project_type_value -> fp_sys_dic_item.item_value`
  - `fp_project_basic.engineering_classification_value -> fp_sys_dic_item.item_value`
- 取数顺序：先取 1.1 结构化基本信息，再结合项目建议书、可行性研究、初步设计等事实补写 1.2。
- 1.2 没有独立数据库字段，必须基于已取到的项目事实做有限概括，不要因为缺少专门背景原文就直接判空。

### 1.1 项目基本信息字段

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

## 第一板块输出要求

### 1.1 项目基本信息
- 必须按表格形式输出。
- 字段名称和顺序必须严格与上表一致。
- 不得增加任何未列出字段。

### 1.2 项目建设背景与项目建设目的与意义
- 优先基于项目基本信息、项目建议书、可行性研究、初步设计等已检索事实进行简要归纳。
- 不要求必须存在独立的背景原文。
- 禁止常识性补写、模板化空话或脱离事实的扩展描述。
- 如相关事实仍不足以支撑，方可写“暂无数据”。

## 执行步骤建议

1. 先确认项目名称或项目编号。
2. 使用 `sqlagent` 查询 1.1 所需结构化字段。
3. 若结果缺失，核对项目标识、字段名、字典关联、市县关联后补查一次。
4. 再使用知识库查询工具检索与项目背景、建设目的、前期批复相关材料。
5. 如知识库材料不足，可结合已取到的项目基本信息和前期批复事实，对 1.2 做有限归纳。
6. 按固定结构输出第一部分正文。

## 最终提醒

- 本测试版只处理第一板块，不输出其他章节。
- 在没有检索结果支撑的情况下，不得补写事实性内容。
- 若某字段或 1.2 小节最终确无数据，可在对应位置写“暂无数据”，但必须先完成检索与补查。