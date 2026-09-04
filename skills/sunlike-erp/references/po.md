# 采购单管理指南 (`po`)

本文档涵盖采购单（`DRPPO`）的新增、查询与删除操作。

---

## 1. 基础命令速查

- **创建采购单（必须预先经用户确认）**：
  ```bash
  ./erp_cli po add -ref-no <参考单号> -cus-no <客户/厂商编号> -dep <部门编号>
  ```
  录入包含多项目明细、品号、数量、单价等复杂结构的采购单，使用 JSON 载荷方式（推荐）：
  ```bash
  ./erp_cli po add -file <JSON文件路径>
  ```
  *(模板参考：`<SKILL_DIR>/assets/po_template.json`)*

- **单笔查询采购单**：
  ```bash
  ./erp_cli po get -id <单据识别号> -no <单据号码>
  ```

- **批量查询采购单**：
  ```bash
  ./erp_cli po list -start <开始日期_YYYY-MM-DD> -end <结束日期_YYYY-MM-DD>
  ```

- **删除采购单**：
  ```bash
  ./erp_cli po delete -id <单据识别号> -no <单据号码>
  ```

---

## 2. 预提交确认规范

调用 `po add` 之前，向用户展示拟采购的客户/厂商、部门、采购明细项（品号、数量、单价、金额）并获得明确确认。
