# 员工资料管理指南 (`yg`)

本文档涵盖员工资料（`MF_YG`）的单笔/批量查询、按姓名搜索、录入新员工与更新操作。

---

## 1. 基础命令速查

- **单笔查询员工资料**：
  ```bash
  ./erp_cli yg get -no <员工工号>
  ```

- **批量查询员工资料（带分页）**：
  ```bash
  ./erp_cli yg list -page <页码> -rows <每页笔数>
  ```

- **按姓名搜索员工（获取工号与部门）**：
  ```bash
  ./erp_cli yg find -name <姓名>
  ```

- **创建/录入新员工（必须预先经用户确认）**：
  ```bash
  ./erp_cli yg add -no <员工工号> -name <姓名> -dep <部门编号>
  ```
  录入完整人事属性（邮箱、银行账号、职位等级等），使用 JSON 载荷方式：
  ```bash
  ./erp_cli yg add -file <JSON文件路径>
  ```
  *(模板参考：`<SKILL_DIR>/assets/employee_template.json`)*

- **更新员工资料（必须预先经用户确认）**：
  ```bash
  ./erp_cli yg update -no <员工工号> -name <新姓名> -dep <新部门> -rem <备注> -pos <职位> -email <邮箱> -tel <手机> -pass-need <T/F> -up <上级工号>
  ```
  或者通过 JSON 文件批量更新：
  ```bash
  ./erp_cli yg update -file <JSON文件路径>
  ```
