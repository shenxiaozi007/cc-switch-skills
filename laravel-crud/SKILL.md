---
name: laravel-crud
description: 当用户要求“新增接口”“改增删改查”“做列表详情”“调整业务逻辑”，或需要按本仓库 BaseController + Modules Business/Dao 分层模式实现 CRUD 能力时使用本技能。
---

# 本仓库 CRUD 开发规范

## 典型分层路径
- 服务目录先按当前仓库实际存在目录探测，当前重点候选为 `www/service.manage.wg.com`、`www/service.his.wg.com`。
- 如果当前目录同时包含 `wg-manage-service` 和 `wg-his-service`，必须先根据用户给出的入口、路由、模块名或打开文件判断目标项目；无法判断再问。
- 管理端路由：`routes/management/proxy/**`（优先看同模块 route 文件）
- Controller：`app/Http/Controllers/Management/Proxy/**`（常继承 `BaseController`）
- Business：`app/Modules/Management/Business/**`（常继承 `BaseBusiness`）
- Dao：主要在 `app/Modules/Basics/Dao/**`，少量在 `app/Modules/**/Dao/**`
- Model：主要在 `app/Modules/Basics/Model/**`，少量在 `app/Modules/**/Model/**`

## 默认流程
1. 先查同模块路由、Controller、Business、Dao、Model 的现有写法和命名。
2. 开始编码前先列出计划新增/修改的 route 清单，默认权限建议必须来自同模块 `WebRoute::*` 使用习惯；未选择需要权限的 route 默认使用 `WebRoute::AUTH_NEEDLESS`。
3. 如果涉及列表接口（分页或不分页），开始编码前先列出可筛选字段候选，让用户选择哪些字段需要做筛选；只为用户确认的字段补 Model scope/Dao 查询能力。
4. 明确接口输入输出、权限点、中间件、是否写操作、是否涉及表结构。
5. 按薄 Controller、Business 校验与编排、Dao 查询持久化的分层落地。
6. 涉及写接口时评估 `api_mutex`，但按现有模块和风险决定是否使用。
7. 完成后做最小验证，并在交付中说明验证结果。

## 开工前必须询问
- 新增或调整 CRUD route 前，必须先把 route 以 `METHOD path -> Controller@method -> 默认权限建议` 的格式列出来，询问用户哪些 route 需要使用 `WebRoute::*` 权限点。
- 用户确认需要权限的 route 后，再补充或复用对应 `WebRoute` 常量；用户未选择、明确不需要权限或未回答但允许继续时，使用 `WebRoute::AUTH_NEEDLESS`。
- 涉及写接口时，在 route 清单里标注是否建议加 `api_mutex`，并等待用户确认是否使用。
- 涉及列表接口时，必须先列出筛选字段候选，优先从表字段、已有同类列表、常见查询口径中推断，例如：编号、名称、状态、类型、负责人、时间范围、父级/归属关系等。
- 用户选择筛选字段后，才在 Model 增加对应 `scopeXxxQuery`/`scopeXxxLikeQuery`，避免无用筛选条件；未被选择的字段不主动实现筛选。
- 如果用户已经在需求里明确给出了 route 权限和筛选字段，可以复述确认后直接执行；如果信息冲突或缺失，先问再写代码。

## 需要先确认的情况
- 路由归属、权限点、菜单/按钮权限语义无法从同模块推断。
- 每次新增 CRUD route 时，确认哪些 route 需要权限点，哪些 route 使用 `AUTH_NEEDLESS`。
- 每次新增列表接口时，确认需要支持筛选的字段。
- 状态流转、审批规则、数据可见范围、默认筛选条件属于业务口径。
- 同一能力在多个模块已有实现且风格冲突。

## 注意事项 / 禁止项
- 不在 Controller 写复杂业务分支。
- 不绕过 Business 直接在 Controller 调 Dao 或 Model。
- 不为了新接口重构无关旧接口。
- Raw SQL 必须参数绑定，避免字符串拼接。
- 不保留已经确认废弃的兼容 route、权限常量、RBAC 项、空 Business 方法、旧枚举或旧字段；用户明确要求保留的兼容入口除外。
- 不在新增 Model 中重复引入 BaseModel 已包含的 trait，例如 `SoftDeletes`、`ModelTimeTraits`、`ModelMainNoTrait`。
- 不用 `$model->refresh()` 解决普通更新后的取值问题；Eloquent `update()` 后当前实例已同步变更字段，只有确实需要重新加载关系或数据库触发结果时才刷新。

## 完成检查
- 路由、中间件、权限点与同模块风格一致。
- Controller 仅收参、调用 Business、`revert()` 返回。
- Business 完成校验、流程编排、事务边界。
- Dao 封装查询和持久化；涉及结构变更时已补 migration 或说明。
- 提交前针对本次新增/修改模块扫描：`request->all(`、直接 `doQuery(`、`extends Model`、`function mainNo(`、重复 trait、废弃 route/权限/枚举、`refresh()`、空兼容方法、旧字段名。
- 新增或修改文件至少跑 `php -l`；批量新增 Model/Dao/migration 时用 `find ... -name '*.php' -print0 | xargs -0 -n1 php -l`。

## Controller 约束
- 仅收参、调用 Business、`revert()` 返回。
- 推荐构造器注入：`__construct(protected Request $request, protected XxxBusiness $business)`。
- 不在 Controller 写复杂业务分支。
- Controller 传入 Business 的数组参数必须用 `$this->request->only([...])` 白名单；单字段接口可用 `$this->validate()` 后只传字段值。
- 管理端写操作通常透传 `management_auth_info()`。
- 管理端路由通常配 `auth:jwt-management` + `WebRoute::*` 权限点，保持与 `routes/management/proxy/**` 同模块文件一致。
- 写接口是否加 `api_mutex` 优先参考同模块已有写接口；新增高风险写操作要在 route 清单里标注建议。

## Business 约束
- 承担参数校验、业务编排、事务控制。
- 常用 `validator(...)->validate()` 做规则校验。
- 枚举型参数优先使用 `Rule::in(XXX::all())`。
- 复杂入参场景建议补 `customAttributes` 优化报错提示。
- 跨表写入必须明确事务边界（`DB::transaction` 或 `app('db')->transaction`）。
- 事务范围要尽量小：参数校验、读库查询、存在性校验、日期/状态前置校验、待写入数组组装，默认放事务外；事务内只放必须一起提交/回滚的写操作、日志、快照、同步更新。
- 事务内避免为了刷新统计再读库；能用事务外已读取的数据计算的字段，先组装 `$updateData`，事务内直接 `update()` 并必要时 `$model->fill($updateData)`。
- Business 不拼装底层组合查询参数。列表筛选中从用户字段转换成 `xxx_params`、关联筛选参数等逻辑应放在对应 Dao 的 `beforeBuildFiled()`。
- 写操作要校验业务前置状态，例如已解散/禁用/已完成的实体不可继续编辑、加人、移除或流转。
- 外部 API 调用与事件触发只保留在业务编排层。

## Dao 约束
- 封装查询、分页、落库、局部更新。
- 复用项目基础 Dao 能力（如 `getList/getPageList/findBy...`）。
- 查询条件优先复用 Model 的 `scopeXxxQuery`、`scopeXxxLikeQuery`、时间范围 scope；只为需求确认的筛选字段补 scope。
- 常规列表优先让 Business 组 `$params` 后调用 `getList/getPageList`；不要为简单列表额外写自定义 Dao 方法。
- Dao 中确定参数的查询方法不要走通用 `$params + doQuery()`，应使用 `newBuilder()->select($this->getSelectColumns($columns))->with($relations)->XxxQuery(...)->first/get()`。
- 确定参数的方法签名要直接表达参数含义，例如 `array $employeeNos, string $realNameLike = '', array $columns = []`，不要用泛化 `$params` 兜底。
- `beforeBuildFiled(Builder $query, array $params): array` 用于整理组合筛选参数，例如把 `team_member_name` 转成 `team_bind_user_params`，把员工姓名/工号/是否在职转成 `employee_params`。
- 禁止 SQL 字符串拼接；Raw 语句必须参数绑定。

## Model 约束
- 新增业务 Model 优先继承 `App\Kernel\Base\BaseModel`，并用 `protected string $mainNoColumn = 'xxx_no';` 定义唯一编号，不再覆写 `mainNo()`。
- 继承 `BaseModel` 后不要重复 `use SoftDeletes, ModelTimeTraits, ModelMainNoTrait`。
- 不为了普通日志时间覆盖 `setUpdatedAt()`；如果业务字段如 `action_at` 需要记录操作时间，优先在 Business 写日志数据时显式赋值。
- 查询条件放到 Model scope：精确筛选用 `scopeXxxQuery`，模糊筛选用 `scopeXxxLikeQuery`，排除类可用 `scopeNotInXxxQuery`，排序字段必须补 `scopeSortByXxxQuery`。
- 关联筛选优先用 `scopeXxxParamsQuery(Builder $builder, array $params)` + `$this->loadSubModelQuery('relationName', $builder, $params)`，避免在 Model 中手写多层 `whereHas` 闭包。
- 本表多字段 OR 查询可以保留专门 scope，例如 `scopeNameOrShortNameLikeQuery`；跨关联组合筛选不要放在 Business 中拼闭包。

## 高风险点清单
- 状态流转要校验前置状态，避免越级更新。
- 写接口建议考虑防重复提交（路由 `api_mutex`）。
- 业务异常统一抛项目异常，不直接返回散乱错误结构。

## 实施步骤
1. 先定义接口输入输出与权限边界。
2. 新增/修改 Controller 方法并保持薄。
3. 在 Business 实现校验和主流程。
4. 在 Dao 完成数据读写。
5. 需要常量时补到 `Constant`/Model。
6. 结构变更补 migration。

## 参考模板
- `references/crud-skeleton.md`
- `references/crud-checklist.md`
