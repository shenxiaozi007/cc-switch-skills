---
name: laravel-migrations
description: 当用户要求“新增 migration”“改表结构”“加字段/索引”“写回滚”“执行指定 migration”，或需要本仓库 migration 设计、生成、回滚和执行路径指导时使用本技能。
---

# 本仓库 Migration 开发规范

## 适用范围
- migration 目录必须按当前仓库实际结构探测，不要写死某个项目域名或服务名。
- 优先使用用户明确给出的目录；否则从当前工作目录向下查找 `database/migrations`，常见形态包括：
  - 当前 Laravel 服务根目录下的 `database/migrations/**`
  - 多服务仓库中的 `www/*/database/migrations/**`
  - 其他服务目录中的 `*/database/migrations/**`
- 如果当前仓库能匹配多个 `database/migrations` 目录，必须先确认目标服务或根据用户给出的路径选择，不要默认指定某一个服务。
- 当前仓库的 migration 可能既有根目录平铺文件，也有专题子目录，目录名常见格式为：
  - `YYYYMMDD_业务主题`，如 `20231229_fission_project`、`20241226_patient`
  - 少量历史目录只有日期或带短横线，新增时优先沿用同模块最近目录，不主动制造新风格。
- 新增前必须先在目标 `database/migrations/` 下查找同模块、同表、同字段历史 migration，优先复用现有专题目录；没有合适目录时再按 `YYYYMMDD_topic` 新建专题目录。

## 默认流程
1. 先确认服务目录、专题目录、目标表当前定义、是否已有同表/同字段/同索引 migration。
2. 新增 migration 时，优先在具体 Laravel 服务目录执行 `php artisan make:migration ...` 生成文件；若要放入专题子目录，生成后移动到目标专题目录并保留 Laravel 时间戳文件名前缀。
3. 按现有同模块表结构设计 `up()` / `down()`：字段顺序、默认值、注释、索引命名、时间字段写法优先跟目标表附近历史一致。
4. 默认不执行 `migrate` / `rollback` / `migrate --pretend`；完成后给用户指定服务目录内的 `--path` 命令。
5. 完成后至少执行 `php -l` 检查生成的 migration 文件。

## 需要先确认的情况
- 目标服务目录或专题目录无法从现有结构判断。
- 新表主键要用 `$table->id()`、`$table->increments('id')` 还是 `$table->bigIncrements('id')` 无法从同模块历史判断。
- 时间字段要用 `$table->timestamps()` / `$table->softDeletes()`，还是显式 `timestamp('created_at')`、`timestamp('updated_at')`、`string('deleted_at', 45)` 无法从目标模块历史判断。
- 变更涉及删除字段、改字段类型、大表加索引、唯一索引、数据回填或发布先后顺序。
- DDL 与目标表历史写法冲突，且无法判断应按历史表还是新规范落地。

## 注意事项 / 禁止项
- 不主动执行 `php artisan migrate`、`migrate:rollback`、`migrate --pretend`。
- 不修改用户未要求的历史 migration。
- 不照抄历史 migration 里的明显错误：例如 `down()` 里 `dropIfExists()` 表名和 `up()` 创建表名不一致、空 `down()`、`alert`/`filed` 等历史拼写错误；新增文件要用正确命名。
- 新表默认推荐 `$table->timestamps();` 和 `$table->softDeletes();`，但如果同模块/目标表族历史显式维护 `created_at` / `updated_at` / `deleted_at`，优先跟随局部一致性，并在结果里说明。
- 单字段索引优先链式写在字段定义上；联合索引才后置声明。
- 索引名称统一使用字段名命名，不带表名前缀：单字段唯一索引用 `字段名_unique`，单字段普通索引用 `字段名_index`；联合索引用 `字段1_字段2_unique` / `字段1_字段2_index`。
- 改表 migration 的 `down()` 不允许留空；无法安全回滚时要写明原因并让用户确认，不要沉默生成不可回滚文件。

## 完成检查
- migration 文件由 `make:migration` 生成或已说明无法生成的原因。
- `up()` / `down()` 成对完整，索引名、表名、字段名一致。
- 已通过 `php -l`。
- 已给出用户在目标服务目录手动执行的 `php artisan migrate --path=/database/migrations/...php` 命令；专题目录文件要包含目录，例如 `--path=/database/migrations/20241226_patient/xxx.php`。

## 核心规则（Must）
1. 表结构变更必须通过 migration，禁止线上手工改表。
2. `up()` 与 `down()` 必须成对设计，可回滚。
3. 配置和环境判断走 `config()`，不要在业务代码直接 `env()`。
4. 涉及 SQL 语句时禁止拼接参数。

## 命名与创建建议
- 新建表：`create_xxx_table`
- 改表字段/索引：`alter_xxx_add_xxx` / `alter_xxx_drop_xxx`
- 历史文件里存在 `alert_*`、`filed`、`tabel` 等拼写错误，新增文件统一使用 `alter`、`field`、`table`。
- 新增 migration 时优先先在 Laravel 服务目录执行 `php artisan make:migration ...` 生成文件，再修改生成出来的 migration 文件。
- 命令示例：
  - `php artisan make:migration create_xxx_table --create=xxx`
  - `php artisan make:migration alter_xxx_add_yyy --table=xxx`

## 执行建议
- 默认不要替用户执行 `php artisan migrate` / `migrate:rollback` / `migrate --pretend` 等迁移相关命令。
- 完成 migration 文件修改后，把建议执行的指定文件命令提供给用户，并提醒由用户确认环境后手动执行。
- 优先给出指定文件执行命令，避免误跑全部：
  - `php artisan migrate --path=/database/migrations/完整文件名.php`
- 执行前确认环境与分支对应：
  - `local` 开发环境
  - `tests` 对应 `beta`
  - `production` 对应 `master`

## 设计要点
1. 先评估变更类型：新表、加字段、改类型、加索引、删字段。
2. 先考虑兼容与回滚：
   - 新字段优先可空或给默认值
   - 高风险删除操作优先分阶段（先停写/迁移数据，再删除）
3. 索引命名清晰且与查询场景匹配。
4. 字段习惯按当前仓库历史收敛：
   - 业务编号、单号、外部关联编号通常为 `string(..., 64)->default('')->comment(...)`
   - 状态/类型字段历史多用 `string(..., 32)` 表达枚举别名；只有明确数值枚举/开关才用 `unsignedTinyInteger`
   - 业务时间、日期、月份通常用 `unsignedInteger`，注释写清 `时间戳`、`Ymd` 或 `Ym`
   - 金额常见 `decimal/unsignedDecimal(16, 2)` 或更高精度，必须按业务精度确认
   - JSON 字段使用 `json(...)->nullable()->comment(...)`，不要设置字符串默认值
   - 操作人字段常见 `add_adm_no/add_adm_name/edit_adm_no/edit_adm_name`
5. 新建表时，单字段索引优先写在字段定义链路上，不要集中放到表定义末尾：
   - 唯一索引：`$table->string('operation_log_no', 64)->default('')->comment('日志编号')->unique('operation_log_no_unique');`
   - 普通索引：`$table->string('ip', 128)->default('')->comment('登录ip')->index('ip_index');`
   - 多字段联合索引或必须后置声明的索引，才使用 `$table->index([...], '字段1_字段2_index')` / `$table->unique([...], '字段1_字段2_unique')`。
6. 新建表优先使用基础模板，但要先对齐同模块历史：
   - 表注释优先写在 `Schema::create()` 内：`$table->comment('模块 - 表含义');`；历史也存在 `set_table_comment('table', '表含义')`，只有维护旧模块时才沿用。
   - 主键默认可使用 `$table->id();`；如果同模块普遍使用 `$table->increments('id')` 或 `$table->bigIncrements('id')`，跟随同模块。
   - 业务编号字段紧跟主键：`$table->string('简化的表名_no', 64)->default('')->comment('xx编号')->unique('简化的表名_no_unique');`
   - `add_time` / `last_update_time` 放在业务字段后：`$table->unsignedInteger('add_time')->default(0)->comment('添加时间');`、`$table->unsignedInteger('last_update_time')->default(0)->comment('最后更新时间');`
   - 新表推荐 `$table->timestamps();` 和 `$table->softDeletes();`；维护显式时间字段的历史表族时保持局部一致。
   - 需要完整模板时读取 `references/migration-skeleton.md`。
7. 大表变更优先拆步，减少锁表与长事务风险。

## 回滚要点
- `down()` 只回滚当前 migration 对应变更，避免误伤其他结构。
- 删除索引、字段、表时名称要和 `up()` 严格对应。
- 新增字段并带索引时，`down()` 先 `dropIndex()` / `dropUnique()` 再 `dropColumn()`。
- 单字段索引如果 `up()` 未显式命名，Laravel 默认名通常是 `{table}_{column}_index`；优先在 `up()` 显式命名，避免回滚猜名。
- 回滚前确认是否已有依赖新字段/新表的代码发布。

## 提交前检查
- [ ] `up()` / `down()` 对称完整
- [ ] 索引/字段命名清晰
- [ ] 指定执行路径可单独运行
- [ ] 变更对线上数据兼容性已评估
- [ ] 与当前分支发布路径一致（beta/master）

## 参考模板
- `references/migration-skeleton.md`
- `references/migration-checklist.md`
