---
name: service-antifraud-ops
description: 守护者max / service-antifraud 项目专用运维与项目上下文 skill。适用于介绍项目架构、判断公共服务和反诈服务职责、本地 Laradock/MySQL/R2 测试、生产部署、打 tag 上线、Cloudflare/H5 发布、微信登录和微信支付检查、队列 worker、smoke 测试，以及排查 ant.hxcbox.cn 和 file.hxcbox.cn 线上问题。
---

# 守护者max 项目运维

## 用途

当任务和 `service-antifraud` 项目自身有关时使用本 skill：介绍系统做什么、判断能力归属哪个服务、准备发布、部署生产、线上验收或排查生产故障。

回答默认使用中文，优先给可直接执行的命令。不要暴露或提交 `.env` 密钥。命令优先匹配现有 Laradock 部署方式。

## 项目地图

- 本地仓库：`/Users/hxc/Documents/php/my-project/service-antifraud`
- 生产仓库：`/var/www/service-antifraud`
- 生产 Laradock：`/var/www/laradock`
- 反诈业务 API：`https://ant.hxcbox.cn`
- 公共基础服务 API：`https://file.hxcbox.cn`
- H5 站点可能独立托管，例如：`https://b.hxcbox.cn`
- 公共基础服务代码：`www/service.storage.company.com`
- 反诈业务服务代码：`www/service.antifraud.local.com`
- uni-app H5/多端客户端：`apps/client`
- 原生微信小程序客户端：`apps/mp-wechat`

项目职责、模块边界见 `references/project-overview.md`。
部署命令、验收和排障见 `references/deploy-runbook.md`。

## 操作原则

- 把 `service.storage.company.com` 当成可复用公共服务：文件、用户、token、验证码登录、微信登录、钱包、套餐、微信支付。
- 把 `service.antifraud.local.com` 当成反诈业务服务：文件绑定、分析记录、OCR/ASR/LLM Agent、报告、管理复核/重试。
- 点数余额放公共服务钱包，用 `project_code=antifraud` 隔离项目。
- 生产 R2 文件访问要走公共服务下载 URL 流程；R2 原始公网 URL 可能返回 `400`。
- 本地和生产测试都用 MySQL，不要为了省事切到 SQLite。
- 生产发布使用 tag，部署说明里写清楚 tag 和 commit。
- 用户要求发布 H5 时，如果 Cloudflare Pages 生产分支是 `master`，推送 `master` 即可触发构建。

## 发布流程

1. 用 `git status --short` 检查改动，避免提交无关文件。
2. 提交前按改动范围跑检查：
   - H5 客户端：`cd apps/client && npm run typecheck`
   - 小程序 JS：`cd apps/mp-wechat && node --check pages/<page>/index.js`
   - PHP 文件：`php -l <changed.php>`
   - 后端相关 PHPUnit：在 `www/service.antifraud.local.com` 或 `www/service.storage.company.com` 下跑相关 `--filter`
3. 使用简短英文 commit message。
4. 创建 `release-YYYYMMDD-HHMM` 格式 tag。
5. 推送 `master` 和 tag。
6. 给出 `references/deploy-runbook.md` 中的生产部署命令。

## 生产排查入口

线上请求失败时，先拿到或检查：

- 精确 URL、HTTP 状态码、返回 body 和时间点。
- 对应服务的 Laravel/Lumen 日志。
- 请求打到 `ant.hxcbox.cn` 还是 `file.hxcbox.cn`。
- 异步分析时 worker 是否运行。
- 生产 `.env` 是否关闭 mock：
  - `WECHAT_LOGIN_MOCK=false`
  - `WECHAT_PAY_MOCK=false`
  - `APP_DEBUG=false`

常见原因：

- `参数错误：生产环境不允许直传 openid`：前端或接口仍在生产使用 mock 微信登录参数。
- `公共服务请求失败`：反诈服务访问不了 `file.hxcbox.cn`、服务密钥不一致，或公共服务自身 500。
- `Could not resolve host: 你的短信或邮件验证码发送服务地址`：`.env` 里还保留验证码 webhook 中文占位；清空它或配置 SMTP。
- R2 原始 URL 返回 `400`：使用公共服务签名 `download-url`，不要直接把 R2 原始 URL 给第三方。
- 分析一直 `pending`：Redis 队列 worker 没跑，或存在 failed job。

## 回复风格

- 默认用简洁中文回答。
- 命令用可复制的代码块。
- 明确命令是在本地 Mac 还是生产服务器执行。
- 绝不把用户消息里的真实密钥写进文档、提交或 skill。
