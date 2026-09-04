# 货品资料管理指南 (`prdt`)

本文档涵盖货品主档（`PRDT`）的新增、单笔/批量查询、差异更新与修改操作。

---

## 1. 基础命令速查

- **创建货品（必须预先经用户确认）**：
  ```bash
  ./erp_cli prdt add -no <prd_no> -name <name> -upr <price>
  ```
  如果需要设置更为复杂的自定义属性，使用 JSON 载荷方式：
  ```bash
  ./erp_cli prdt add -file <JSON文件路径>
  ```
  *(模板参考：`<SKILL_DIR>/assets/product_template.json`)*

- **单笔查询货品**：
  ```bash
  ./erp_cli prdt get -no <prd_no>
  ```

- **批量查询货品（带分页）**：
  ```bash
  ./erp_cli prdt list -page <页码> -rows <每页笔数>
  ```

- **差异查询（查询指定时间戳后的变更）**：
  ```bash
  ./erp_cli prdt diff -up-dd <时间戳> -page <页码> -rows <每页笔数>
  ```

- **更新货品资料（必须预先经用户确认）**：
  ```bash
  ./erp_cli prdt update -no <prd_no> -name <新名称>
  ```
