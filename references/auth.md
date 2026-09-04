# 验证、登录与会话管理指南 (`auth`)

本文档涵盖 ERP API 的 OAuth 授权码登录、账号密码登录、Token 在线校验与状态查询。

---

## 1. 登录模式与命令

系统**默认采用 OAuth 授权码登录**（浏览器授权），无需手动提供账号密码。

### 1.1 OAuth 授权码登录（默认推荐）

- **标准命令**：
  ```bash
  ./erp_cli login -url http://<你的站点>/ERPAPI
  ```
  *(无头/远程服务器环境下可加 `-no-browser`，手动在本地浏览器访问终端输出的授权 URL)*

- **登录机制与特性**：
  - **内置凭据**：`ClientId` 与 `ClientSecret` 已固化在程序内部，无需传参；
  - **端点自动推导**：`authorize` / `token` / `userinfo` 端点根据 `-url` 自动推导为 `<url>/oauth/*`；
  - **交互式自适应**：人类在终端直接运行未带 `-url` 的 `login` 会提示输入；在 AI Agent 非 TTY 环境下缺少 `-url` 会快速报错提示，避免阻塞；
  - **真实账套与用户信息回填**：OAuth 回调后自动请求 `userinfo` 获取真实 `COMPNO`（账套）、`USR`（工号）、部门等并写入配置文件。

### 1.2 账号密码登录（传统方式）

仅在用户**特别强调账号密码登录**时使用（显式传 `-legacy` 或同时提供 `-usr` 与 `-pwd`）：
```bash
./erp_cli login -legacy -url http://<你的站点>/ERPAPI -comp <账套> -usr <账号> -pwd "<密码>"
```

---

## 2. 状态查询与 Token 在线校验

```bash
./erp_cli status
```

- **功能**：
  1. 显示当前已绑定的接口 URL、账套、登录用户名、所属部门；
  2. 访问令牌自动脱敏显示（如 `4d4a****`）；
  3. 真实在线请求 `userinfo` 校验 Token 有效性（显示 `token 校验: 有效 (已通过 userinfo 验证)` 或提示过期）。
