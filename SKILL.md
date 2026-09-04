---
name: sunlike-erp
description: 运行 ERP 命令行工具（`erp_cli`）在天心天思数字化 API 平台上管理货品（新增、单笔查询、批量查询、更新、差异查询）、采购单（新增、单笔查询、批量查询、删除）、员工资料（单笔查询、批量查询）、请假单（新增、单笔查询、批量查询、删除）、报销单（新增、查询、批量查询、修改、删除、费用设定查询）、外出单（新增、查询、批量查询、删除）、附件（生成、查询、删除）、未/忘打卡（新增、查询、批量查询、删除）和外出申请单（新增、查询、批量查询、删除）。内置"费用报销"工作流：附件检查、发票识别（文本/扫描件转图/数电票内嵌XML交叉核验）、费用代号分组与表身汇总（乘车日期、同代号合并、发票号码库 TF_BX_INVNO 与发票登记主档 INV_NO）、附件入库、表头生成、预提交确认、删单重建与核验。在用户要求与天心天思 ERP 交互、管理货品目录、查询员工信息、生成/查询/删除采购单、增删查请假单、报销单、外出单、附件、未/忘打卡、外出申请单，以及提出"费用报销/报销"时使用本 Skill。
---

# Sunlike ERP

本 Skill 内置了适用于多平台的二进制可执行文件（存放于 `<SKILL_DIR>/bin/` 子目录下）。

## 1. 调用路径规范（必读）

AI Agent 应将路径解析为**本 Skill 所在目录（即 `SKILL.md` 的父目录 `<SKILL_DIR>`）下的路径**，根据当前操作系统与 CPU 架构选用对应的二进制（必要时先执行 `chmod +x` 赋予可执行权限）：

- **macOS (Apple Silicon M1/M2/M3/M4)**: `<SKILL_DIR>/bin/erp_cli_darwin_arm64`
- **macOS (Intel)**: `<SKILL_DIR>/bin/erp_cli_darwin_amd64`
- **Linux (amd64 / x86_64)**: `<SKILL_DIR>/bin/erp_cli_linux_amd64`
- **Linux (arm64)**: `<SKILL_DIR>/bin/erp_cli_linux_arm64`
- **Windows (amd64)**: `<SKILL_DIR>\bin\erp_cli_windows_amd64.exe`

*(以下示例指南统一以 `./erp_cli` 简写代指上述解析后的实际可执行文件路径)*

---

## 2. 核心安全红线（所有单据写操作必读）

> **⚠️ 必须获得用户明确预确认**：
> **所有单据与基础资料的新增、修改、删除（如 `prdt add/update`、`po add/delete`、`yg add/update`、`leave add/delete`、`bx add/update/delete`、`goout add/delete`、`card add/delete`、`gw add/delete`）在正式调用 CLI 写入 ERP 之前，必须先将拟提交的关键信息以清晰结构化的形式向用户展示，获得用户明确确认（例如用户回复“确认”、“提交”、“同意”等）后，方可执行实际的写命令！严禁未经确认静默提交！**

---

## 3. 快速登录与状态核验

- **检查当前登录状态与 Token 有效性**：
  ```bash
  ./erp_cli status
  ```
- **OAuth 授权码登录（默认方式）**：
  ```bash
  ./erp_cli login -url http://<你的站点>/ERPAPI
  ```
  *注：Agent 调用必须显式带 `-url`；未提供时助手应在对话中主动向用户询问站点地址。详细机制见 [references/auth.md](references/auth.md)*。

---

## 4. 业务模块文档索引（操作前按需查阅对应文档）

为降低单次上下文开销并确保业务规则准确，执行具体业务单据操作时，**请使用 `read` 工具查阅 `<SKILL_DIR>/references/<模块>.md` 获取完整指令参数与避坑经验**：

| 业务领域 | 子命令 | 关键特性与避坑提示 | 详细手册 |
| :--- | :--- | :--- | :--- |
| **费用报销** | `bx` | 发票识别、数电票 XML 提取、费用代号分组、发票主档 INV_NO 写入、删单重建 | [`references/expense.md`](references/expense.md) |
| **请假单** | `leave` | `VOC_CNT` 单位取决于假别 `UT_DAY`（天/小时/次）、年假映射 C06 特休 | [`references/leave.md`](references/leave.md) |
| **外出申请单** | `gw` | 缺省地址时必须显式赋 `'0'`（`STA_COUN_ID='0'`、`END_COUN_ID='0'`） | [`references/gw.md`](references/gw.md) |
| **未/忘打卡** | `card` | `C11 忘打卡` 与 `C12 未打卡` 考勤补卡操作 | [`references/card.md`](references/card.md) |
| **假别代号** | `sz` | 绝不硬编码假别代号，通过 `sz list` 实时获取当前账套假别列表及计量单位 | [`references/sz.md`](references/sz.md) |
| **外出单** | `goout` | 外出事由、工时与假别记录 | [`references/goout.md`](references/goout.md) |
| **采购单** | `po` | 采购单新增、明细录入与查询 | [`references/po.md`](references/po.md) |
| **货品资料** | `prdt` | 货品目录维护、单笔/批量/差异查询与更新 | [`references/prdt.md`](references/prdt.md) |
| **员工资料** | `yg` | 人事资料查询、按姓名解析工号与部门、人事更新 | [`references/yg.md`](references/yg.md) |
| **附件管理** | `file` | 单据表头/表身附件上传与查询删除 | [`references/file.md`](references/file.md) |
| **认证登录** | `login`/`status` | OAuth 授权码登录、用户信息回填、Token 在线校验 | [`references/auth.md`](references/auth.md) |

---

## 5. 参考模板

模板文件存放于 `<SKILL_DIR>/assets/` 目录下（使用 `-file` 载荷参数引用时解析为 `<SKILL_DIR>/assets/...`）：

- 报销单模板：`<SKILL_DIR>/assets/bx_template.json`
- 请假单模板：`<SKILL_DIR>/assets/leave_template.json`
- 外出申请单模板：`<SKILL_DIR>/assets/gw_template.json`
- 未/忘打卡模板：`<SKILL_DIR>/assets/card_template.json`
- 外出单模板：`<SKILL_DIR>/assets/goout_template.json`
- 采购单模板：`<SKILL_DIR>/assets/po_template.json`
- 货品资料模板：`<SKILL_DIR>/assets/product_template.json`
- 员工资料模板：`<SKILL_DIR>/assets/employee_template.json`
- 附件模板：`<SKILL_DIR>/assets/file_template.json`
