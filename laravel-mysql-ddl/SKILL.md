---
name: laravel-mysql-ddl
description: 当用户要求在创建 Laravel migration 前，根据业务需求生成、设计、草拟、评审或确认 MySQL SQL/DDL 时使用本技能，尤其适用于“生成mysql语句”“先生成SQL”“建表语句”“改表语句”“字段设计”“索引设计”“表结构草案”，或需要在 laravel-migrations 前增加 SQL 确认步骤。
---

# 需求生成 MySQL DDL

## 定位
这个 skill 负责把业务需求整理成 MySQL DDL 草案，作为 `laravel-migrations` 的前置步骤。默认只输出 SQL 草案、设计说明、风险点和待确认项，不创建 migration 文件，不执行 SQL。

## 默认流程
1. 先判断变更类型：新建表、已有表加字段、修改字段、加索引、删字段、拆表、数据回填。
2. 先按当前仓库实际结构定位目标 `database/migrations/`：优先用用户给出的路径，否则从当前工作目录查找；如果存在多个服务目录，不要默认指定某个服务，要先确认或根据上下文选择。
3. 查看目标 migration 目录下同模块、同表、同前缀表的历史 migration，提取局部字段风格、主键类型、时间字段和索引命名习惯。
4. 从需求中提取表名、字段、类型、默认值、是否可空、注释、唯一性、索引、查询场景、数据量和兼容性要求。
5. 信息不足时先给“保守草案 + 待确认项”；涉及高风险操作时必须明确要求用户确认。
6. 按本仓库迁移规范生成 MySQL DDL；新表优先包含 `id`、业务编号、`add_time`、`last_update_time`、`deleted_at`、`created_at`、`updated_at`，但时间字段类型要说明将由后续 migration 跟随同模块历史落地。
7. 输出 DDL 后停下，等待用户确认。
8. 用户确认 SQL 后，再建议使用 `laravel-migrations` 按该 SQL 生成 migration 文件。

## 重要边界
- 不直接执行 SQL。
- 不直接创建 migration 文件，除非用户明确要求切到 `laravel-migrations`。
- 不把需求里的业务描述直接变成大段宽表；先识别核心实体和关系。
- 不为没有查询场景的字段主动加索引；枚举字段、文件 ID 字段也不要因为“可能会用”就加索引。
- 删除字段、改字段类型、大表加索引、非空无默认值、唯一约束、数据回填都要标为高风险。

## DDL 规则
需要字段类型、索引和模板细则时读取 `references/mysql-ddl-guidelines.md`。

核心约定：
- 表名和字段名使用小写蛇形命名。
- 新表默认 `ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci`。
- 新表主键默认 `BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY`；但生成 migration 时要根据同模块历史选择 `$table->id()`、`increments('id')` 或 `bigIncrements('id')`。
- 业务编号字段紧跟主键：`{short_table}_no varchar(64) NOT NULL DEFAULT '' COMMENT 'xx编号'`，通常加唯一索引。
- 时间字段按现有项目习惯保留 `add_time int unsigned NOT NULL DEFAULT 0`、`last_update_time int unsigned NOT NULL DEFAULT 0`。
- 同时保留 Laravel 时间字段：`created_at timestamp NULL DEFAULT NULL`、`updated_at timestamp NULL DEFAULT NULL`、`deleted_at timestamp NULL DEFAULT NULL`；历史 migration 里存在 `deleted_at varchar(45)` 或 `$table->softDeletes()` 等写法，DDL 草案先表达结构意图，后续 migration 按同模块历史选择具体 Laravel 写法。
- 字段必须写 `COMMENT`，索引名必须稳定清晰。
- 状态/类型字段优先根据现有代码含义选择：
  - 只有 0/1、数值枚举、数量级很小且不需要展示别名时，用 `tinyint unsigned NOT NULL DEFAULT 0`
  - 项目历史大量使用字符串枚举别名时，用 `varchar(32) NOT NULL DEFAULT ''`
- 编号、单号、外部关联 no 默认 `varchar(64) NOT NULL DEFAULT ''`；文件 ID 默认 `varchar(32) NOT NULL DEFAULT ''`；名称默认 `varchar(64/128)`；备注默认 `varchar(256/512)`，长正文才用 `text`。
- 业务时间命名按精度表达：
  - 精确到时分秒的业务时间使用 `_at` 后缀，存 Unix timestamp，例如 `sale_start_at`。
  - 只按天统计、筛选或展示日期使用 `_date` 后缀，存 `Ymd`，例如 `effective_date`。
  - 月维度使用 `_month` 后缀，存 `Ym`，例如 `record_pay_month`。
  - 记录创建/更新时间只使用 `add_time` / `last_update_time`，不要再新造普通 `_time` 字段。
- 金额/单价/成本默认先按 `decimal(16,2)` 草拟；库存、均价、分摊等高精度场景确认是否用 `decimal(16,4)` 或更高。
- JSON 字段使用 `json DEFAULT NULL COMMENT '...'` 或 `json NULL COMMENT '...'`，不要给空字符串默认值。
- 操作人字段按仓库习惯优先使用 `add_adm_no/add_adm_name/edit_adm_no/edit_adm_name`。

## 仓库真实习惯摘要
- migration 目录既有根目录平铺，也有大量 `YYYYMMDD_topic` 专题目录；SQL 草案只管 DDL，但输出下一步时要提醒后续 migration 放到同模块最近专题目录。
- 新表常见表注释为 `COMMENT='模块 - 表含义'`，后续 migration 可能落成 `$table->comment(...)`；旧表也有 `set_table_comment()`。
- 单字段索引历史常用字段名作为索引名，例如 `KEY clinic_no (clinic_no)`；新草案可用 `idx_` 前缀，但如果是本仓库高频 no 字段，优先用稳定短名并在索引说明中解释。
- 历史改表 migration 存在空 `down()` 和拼写错误文件名，生成草案时要明确回滚 SQL 或说明无法安全回滚的原因，不延续这些问题。

## 输出格式
每次输出按这个顺序：
1. 需求理解。
2. MySQL DDL 草案。
3. 字段说明。
4. 索引说明。
5. 风险与兼容性。
6. 待确认项。
7. 下一步：确认后使用 `$laravel-migrations` 根据 SQL 生成 migration。

## 需要先确认的情况
- 表名、模块名、业务编号命名无法推断。
- 字段类型会影响金额、精度、时间、枚举或状态流转。
- 主键是否必须保持同模块 `int unsigned` 还是可用 `bigint unsigned`。
- Laravel 时间字段要按 `timestamp NULL` 表达，还是兼容历史 `deleted_at varchar(45)`。
- 是否唯一、是否允许为空、默认值、数据量级、是否大表不明确。
- 业务时间字段应该是 `_at`、`_date` 还是 `_month` 不明确。
- 枚举字段、文件 ID 字段是否真的存在筛选、关联或排序场景不明确。
- 变更会影响已有线上数据或需要数据回填。
- 需求可能拆成多张表，但用户只要求“一张表”。

## 完成检查
- SQL 可直接表达结构意图，但明确未执行。
- 字段名、类型、默认值、注释齐全。
- 索引和唯一约束有查询或业务理由。
- 高风险点已标出。
- 已提醒用户确认 SQL 后再调用 `laravel-migrations`。
