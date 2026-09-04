# 附件库管理指南 (`file`)

本文档介绍单据附件库（`FILE`）的新增上传、查询与删除操作。

---

## 1. 基础命令速查

- **上传附件**（自动读取本地文件并 Base64 编码）：
  ```bash
  ./erp_cli file add -bil-id <单据别> -bil-no <单据号码> -bil-itm <项次, 0=表头> -path <本地文件路径>
  ```
  *(注：`bil-id` 如 `BX` 报销单、`GW` 外出申请单；`bil-itm` 0 代表挂在单据表头)*

- **查询附件**：
  ```bash
  ./erp_cli file get -table <数据表名> -file-id <附件ID>
  ```
  *(注：报销单附件表名为 `TF_BX_FILE1`)*

- **删除附件**：
  ```bash
  ./erp_cli file delete -table <数据表名> -file-id <附件ID>
  ```
