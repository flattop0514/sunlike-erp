# 未/忘打卡补卡管理指南 (`card`)

本文档涵盖考勤异常（忘打卡 C11、未打卡 C12）的补卡单新增、查询与删除操作。

---

## 1. 基础命令速查

- **考勤代号说明**：
  - **`C11`**：**忘打卡**
  - **`C12`**：**未打卡**

- **新增未/忘打卡（必须预先经用户确认）**：
  ```bash
  # 方式 A：使用工号
  ./erp_cli card add -ref-no <参考单号> -yg-no <员工工号> -sz-no <C11|C12> -trs-dd <日期时间> -sz-ym <补卡年月>

  # 方式 B：使用姓名自动解析（推荐）
  ./erp_cli card add -name <员工姓名> -sz-no <C11|C12> -trs-dd <补卡日期时间>
  ```
  如果需要批量导入或更复杂的属性，使用 JSON 载荷方式：
  ```bash
  ./erp_cli card add -file <JSON文件路径>
  ```
  *(模板参考：`<SKILL_DIR>/assets/card_template.json`)*

- **单笔查询未/忘打卡**：
  ```bash
  ./erp_cli card get -yg-no <员工工号> -sz-no <C11|C12> -trs-dd <日期时间>
  ```

- **批量查询未/忘打卡**：
  ```bash
  ./erp_cli card list -start <开始日期_YYYY-MM-DD> -end <结束日期_YYYY-MM-DD> -sz-no <C11|C12> [-dep <部门代号>]
  ```

- **删除未/忘打卡**：
  ```bash
  ./erp_cli card delete -yg-no <员工工号> -sz-no <C11|C12> -trs-dd <日期时间>
  ```

---

## 2. 预提交确认规范

在调用 `card add` 之前，必须向用户展示以下信息并获得确认：
- 补卡员工（姓名/工号/部门）
- 补卡性质（`C11 忘打卡` 或 `C12 未打卡`）
- 补卡时间点（`TRS_DD`，如 `2026-08-21 08:30:00`）
- 补卡年月（`SZ_YM`）
