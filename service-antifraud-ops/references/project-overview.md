# service-antifraud 项目概览

## 产品定位

`service-antifraud` 是“守护者max”反诈 MVP。核心链路是：登录/注册 -> 买点数 -> 上传图片或音频 -> 创建分析任务 -> OCR/ASR/LLM Agent 分析 -> 生成风险报告 -> 成功扣点，失败释放冻结点数。

## 服务边界

### 公共基础服务

路径：`www/service.storage.company.com`

线上域名：`https://file.hxcbox.cn`

职责：

- 文件上传、文件详情、下载 URL、Cloudflare R2 对接
- 用户、身份、验证码登录、微信登录
- 公共 Bearer token 与内部 `auth/introspect`
- 项目钱包：`project_code=antifraud`
- 点数套餐、支付订单、微信支付 V3、支付回调、幂等到账

关键 API 前缀：

- `/service/api/v1/auth/*`
- `/service/api/v1/file/*`
- `/service/api/v1/wallet/*`
- `/service/api/v1/payment/*`

### 反诈业务服务

路径：`www/service.antifraud.local.com`

线上域名：`https://ant.hxcbox.cn`

职责：

- 当前用户项目映射与 `/api/v1/me`
- 公共文件注册到 `file_assets`
- 图片/音频分析任务
- OCR/ASR/LLM Agent
- 风险规则 fallback
- 报告查询、记录列表、管理端重试

关键 API：

- `GET /api/v1/me`
- `POST /api/v1/files/register`
- `POST /api/v1/analysis/image`
- `POST /api/v1/analysis/audio`
- `GET /api/v1/analysis/{recordId}`
- `GET /api/v1/analysis-records`
- `POST /management/proxy/analysis/{recordId}/retry`

## 前端

### H5 / uni-app

路径：`apps/client`

生产环境变量：

```env
VITE_API_BASE_URL=https://ant.hxcbox.cn
VITE_FILE_BASE_URL=https://file.hxcbox.cn
```

常用命令：

```bash
npm run typecheck
npm run build:h5
npm run build:mp-weixin
```

### 原生微信小程序

路径：`apps/mp-wechat`

微信公众平台合法域名：

- request: `https://ant.hxcbox.cn`
- request: `https://file.hxcbox.cn`
- uploadFile: `https://file.hxcbox.cn`
- downloadFile: `https://file.hxcbox.cn`

## Agent 与 LLM

第三方 LLM 使用 OpenAI-compatible 配置：

```env
LLM_BASE_URL=
LLM_API_KEY=
LLM_MODEL=
LLM_VISION_MODEL=
LLM_AUDIO_MODEL=
LLM_TIMEOUT=60
OCR_PROVIDER=llm
ASR_PROVIDER=llm
```

图片分析优先使用视觉模型；音频优先 ASR。LLM 不可用时，风险规则作为 fallback，仍应给基础报告。

## 数据与运行时

- MySQL：公共服务和反诈服务分库。
- Redis：缓存和队列。
- R2：线上文件存储，公共服务负责上传和签名下载。
- Supervisor 或 Laradock worker：消费反诈分析队列。

## 重要约束

- 生产环境不要使用 mock 微信登录或 mock 支付。
- 生产环境不要保留 `.env` placeholder，例如 `change-me` 或中文占位 URL。
- 不要把 R2 密钥、微信支付密钥、LLM key 写入代码、文档或 skill。
- 点数余额以公共服务钱包为准，反诈服务不要自建真实余额。
