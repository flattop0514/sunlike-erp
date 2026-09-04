# 外出单管理指南 (`goout`)

本文档涵盖外出单（`GOOUT`）的新增、查询与删除操作。

---

## 1. 基础命令速查

- **新增外出单（必须先经用户确认）**：
  ```bash
  # 方式 A：使用工号
  ./erp_cli goout add -ref-no <参考单号> -yg-no <员工工号> -sz-no <假别代号> -rem <外出事由>

  # 方式 B：使用姓名自动解析（推荐）
  ./erp_cli goout add -name <员工姓名> -sz-no <假别代号> -rem <外出事由>
  ```
  可额外指定起止时间及工时：
  ```bash
  ./erp_cli goout add -name <员工姓名> -sz-no <假别代号> -start <开始日期_YYYY-MM-DD> -start-time <HH:MM> -end <截止日期_YYYY-MM-DD> -end-time <HH:MM> -voc-cnt <工时或天数> -rem <外出事由>
  ```
  复杂结构使用 JSON 载荷方式：
  ```bash
  ./erp_cli goout add -file <JSON文件路径>
  ```
  *(模板参考：`<SKILL_DIR>/assets/goout_template.json`)*

- **单笔查询外出单**：
  ```bash
  ./erp_cli goout get -eg-dd <单据日期时间> -yg-no <员工工号> -sz-no <假别代号>
  ```

- **批量查询外出单**：
  ```bash
  ./erp_cli goout list -start <开始日期_YYYY-MM-DD> -end <结束日期_YYYY-MM-DD> -dep <部门代号>
  ```

- **删除外出单**：
  ```bash
  ./erp_cli goout delete -eg-dd <单据日期时间> -yg-no <员工工号> -sz-no <假别代号>
  ```

---

## 2. 预提交确认规范

在调用 `goout add` 写入系统之前，向用户展示并确认：外出人、外出假别、外出起止时间、外出事由。
