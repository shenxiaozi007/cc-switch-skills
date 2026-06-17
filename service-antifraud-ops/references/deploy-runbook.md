# service-antifraud 部署与排障 Runbook

## 本地验证

本地 Laradock 常见路径：

```bash
cd /Users/hxc/Documents/php/laradock
docker compose up -d nginx php-fpm workspace mysql redis
```

本地项目路径：

```bash
cd /Users/hxc/Documents/php/my-project/service-antifraud
```

本地前端环境：

```env
VITE_API_BASE_URL=http://service.antifraud.local.hxc
VITE_FILE_BASE_URL=http://service.storage.company.hxc
```

本地后端用 MySQL，不要为了测试改 SQLite。文件存储可以直接用线上 R2 配置，但不要提交 `.env`。

## 发版前检查

```bash
cd /Users/hxc/Documents/php/my-project/service-antifraud
git status --short
git diff --check
```

前端：

```bash
cd /Users/hxc/Documents/php/my-project/service-antifraud/apps/client
npm run typecheck
npm run build:h5
```

小程序 JS：

```bash
cd /Users/hxc/Documents/php/my-project/service-antifraud/apps/mp-wechat
node --check pages/audio/index.js
```

PHP：

```bash
cd /Users/hxc/Documents/php/my-project/service-antifraud/www/service.antifraud.local.com
php -l app/Modules/Service/AnalysisBusiness.php
./vendor/bin/phpunit --filter 'ContentExtractionServiceTest|RiskAnalysisBusinessTest'
```

## 打 tag 并推送

```bash
cd /Users/hxc/Documents/php/my-project/service-antifraud
git add <files>
git commit -m "Fix or feature summary"
git tag release-YYYYMMDD-HHMM
git push origin master
git push origin release-YYYYMMDD-HHMM
```

## 生产部署

在生产服务器执行：

```bash
cd /var/www/service-antifraud
git fetch --all --tags
git checkout release-YYYYMMDD-HHMM
```

安装依赖：

```bash
cd /var/www/laradock
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.storage.company.com && composer install --no-dev --optimize-autoloader'
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.antifraud.local.com && composer install --no-dev --optimize-autoloader'
```

执行迁移：

```bash
cd /var/www/laradock
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.storage.company.com && php artisan migrate --force'
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.antifraud.local.com && php artisan migrate --force'
```

重启运行时：

```bash
cd /var/www/laradock
docker compose restart php-fpm
docker compose exec nginx nginx -t
docker compose exec nginx nginx -s reload
```

队列 worker：

```bash
supervisorctl status
supervisorctl restart antifraud-worker:*
```

如果服务器使用 Laradock 内部 supervisor 目录，配置来源通常是：

```bash
/var/www/service-antifraud/docs/laradock/supervisor/antifraud-worker.conf
```

## 生产 .env 核心项

公共服务 `www/service.storage.company.com/.env`：

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://file.hxcbox.cn
CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
OBJECT_STORAGE_DEFAULT_DISK=cloudflare_r2
WECHAT_LOGIN_MOCK=false
WECHAT_PAY_MOCK=false
SERVICE_APP_ID=antifraud
SERVICE_SECRET=<same-as-antifraud>
```

反诈服务 `www/service.antifraud.local.com/.env`：

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://ant.hxcbox.cn
CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
COMMON_SERVICE_BASE_URL=https://file.hxcbox.cn/service/api/v1
COMMON_SERVICE_PROJECT_CODE=antifraud
COMMON_SERVICE_APP_ID=antifraud
COMMON_SERVICE_SECRET=<same-as-common-service>
```

如果容器内访问公网域名异常，可使用 Nginx 内网访问：

```env
COMMON_SERVICE_BASE_URL=http://nginx/service/api/v1
COMMON_SERVICE_HOST=file.hxcbox.cn
```

邮箱验证码若使用企业微信邮箱：

```env
VERIFICATION_CODE_WEBHOOK_URL=
VERIFICATION_CODE_WEBHOOK_TOKEN=
VERIFICATION_CODE_MAIL_ENABLED=true
VERIFICATION_CODE_MAIL_HOST=smtp.exmail.qq.com
VERIFICATION_CODE_MAIL_PORT=465
VERIFICATION_CODE_MAIL_ENCRYPTION=ssl
```

## Smoke 测试

健康检查：

```bash
curl -i https://ant.hxcbox.cn/api/v1/system/health
curl -i https://file.hxcbox.cn/service/api/v1/file/disks
```

基础 smoke：

```bash
cd /var/www/service-antifraud
SMOKE_ACCOUNT=<test-email-or-phone> \
SMOKE_CODE=<real-code> \
ANT_BASE_URL=https://ant.hxcbox.cn \
FILE_BASE_URL=https://file.hxcbox.cn \
bash docs/scripts/smoke-mvp.sh
```

完整分析 smoke：

```bash
cd /var/www/service-antifraud
SMOKE_ACCOUNT=<test-email-or-phone> \
SMOKE_CODE=<real-code> \
ANT_BASE_URL=https://ant.hxcbox.cn \
FILE_BASE_URL=https://file.hxcbox.cn \
SMOKE_REQUIRE_ANALYSIS=true \
bash docs/scripts/smoke-e2e-analysis.sh
```

## 常见问题

### 生产不允许直传 openid

含义：生产环境仍然在走 mock 微信登录入参。检查前端和公共服务：

```env
WECHAT_LOGIN_MOCK=false
```

### 公共服务请求失败

先直接请求公共服务：

```bash
curl -i "https://file.hxcbox.cn/service/api/v1/payment/packages?project_code=antifraud"
```

再查公共服务日志。常见原因是公共服务 500、服务签名不一致、`COMMON_SERVICE_BASE_URL` 不可达。

### 验证码接口 cURL error 6

如果错误里出现中文占位 URL，说明 `.env` 还保留：

```env
VERIFICATION_CODE_WEBHOOK_URL=你的短信或邮件验证码发送服务地址
```

改为空并启用 SMTP，或配置真实 webhook。

### RedisStore 类型错误

Lumen/Illuminate 版本要求 redis 绑定为 `Illuminate\Contracts\Redis\Factory`。确认已安装 `illuminate/redis` 并注册 Redis service provider，不要直接绑定 `Redis` 扩展实例给 cache。

### artisan key:generate 不存在

部分 Lumen 项目没有启用该命令。可以手动设置 `.env` 的 `APP_KEY` 为 32 位以上随机字符串。

### R2 文件 URL 400

Cloudflare R2 原始 endpoint URL 不等于公开可下载 URL。分析图片/音频前应通过公共服务获取签名下载 URL，或由服务器下载后转给 LLM。

### 音频 ASR 404

第三方 OpenAI-compatible 服务可能不支持 `/audio/transcriptions`。如果用户提交了文本，反诈服务应使用用户文本继续风险分析；如果没有文本，任务失败并释放冻结点数。

### 分析一直 pending

检查 worker 和失败队列：

```bash
cd /var/www/laradock
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.antifraud.local.com && php artisan queue:failed'
tail -f /var/www/service-antifraud/www/service.antifraud.local.com/storage/logs/worker.log
```

## 回滚

```bash
cd /var/www/service-antifraud
git fetch --all --tags
git checkout <previous-stable-tag>

cd /var/www/laradock
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.storage.company.com && composer install --no-dev --optimize-autoloader'
docker compose exec workspace bash -lc 'cd /var/www/service-antifraud/www/service.antifraud.local.com && composer install --no-dev --optimize-autoloader'
docker compose restart php-fpm
docker compose exec nginx nginx -s reload
supervisorctl restart antifraud-worker:*
```

不要自动回滚 migration，除非确认新字段/新表没有被线上数据使用。
