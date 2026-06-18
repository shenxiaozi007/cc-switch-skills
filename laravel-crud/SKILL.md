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
6. 如果项目存在 `config/rbac/**` 权限配置，并且本次 route 使用了 `WebRoute::*` 权限点，同步补对应权限组和权限项配置。
7. 完成后做最小验证，并在交付中说明验证结果。

## 开工前必须询问
- 新增或调整 CRUD route 前，必须先把 route 以 `METHOD path -> Controller@method -> 默认权限建议` 的格式列出来，询问用户哪些 route 需要使用 `WebRoute::*` 权限点。
- 用户确认需要权限的 route 后，再补充或复用对应 `WebRoute` 常量；用户未选择、明确不需要权限或未回答但允许继续时，使用 `WebRoute::AUTH_NEEDLESS`。
- 如果同模块已有 `config/rbac/management/module/**` 或类似权限配置，route 权限确认时一并说明将新增/复用哪个 `permission_group`、`group_alias`、`alias_name`。
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
- 新增或修改方法必须补充 PHPDoc 方法注释，注释必须包含方法说明、`@param` 入参说明和 `@return` 返回说明；没有入参的方法仍需写 `@return`。
- Controller、Business、Dao、Model 的 public/protected 方法必须写清楚参数业务含义和返回结构；私有方法如果承载校验、格式化、事务组装或复杂逻辑，也必须补 `@param`/`@return`。
- 新增接口字段必须以 migration/Model 实际字段为准，页面原型中存在但表结构不存在的字段不要主动写入。
- Raw SQL 必须参数绑定，避免字符串拼接。
- 涉及日期、月份、时间范围、自然月起止、时间戳格式化等时间处理时，优先使用 `Carbon`，不要默认使用 `DateTime` / `DateTimeImmutable` 混搭，除非现有模块已有明确固定写法且本次只做局部维护。
- 从 `Ymd`、时间戳等日期值推导月份、日期或时间段时，使用 `Carbon::createFromFormat()`、`date()` 等时间函数，不用 `substr((string)$date, 0, 6)` 这类字符串切割。
- 不保留已经确认废弃的兼容 route、权限常量、RBAC 项、空 Business 方法、旧枚举或旧字段；用户明确要求保留的兼容入口除外。
- 不在新增 Model 中重复引入 BaseModel 已包含的 trait，例如 `SoftDeletes`、`ModelTimeTraits`、`ModelMainNoTrait`。
- 不用 `$model->refresh()` 解决普通更新后的取值问题；Eloquent `update()` 后当前实例已同步变更字段，只有确实需要重新加载关系或数据库触发结果时才刷新。

## 关联查询规范
- 列表/详情需要返回关联表展示字段时，优先在 Business 定义 `$relations` 并传给 Dao，由 Dao 使用 `with($relations)` 预加载；关联字段必须只选择必要列，并包含关系匹配键，例如 `planner:employee_no,real_name`、`team:team_no,name`。
- 关联展示字段不要默认落到主表字段，也不要为了单个列表展示新增 `getXxxAttribute()` 把关联字段伪装成主表属性；只有通用、稳定、属于该 Model 语义的派生字段才适合做 accessor。
- 接口契约需要扁平返回少量关联展示字段时，Business 可读取已加载关系后用 `$model->setAttribute('planner_name', $model->planner->real_name ?? '')` 这类方式补响应字段；不要把这类响应字段写入数据库字段数组。
- 关联筛选条件优先封装到 Model scope 或 Dao `beforeBuildFiled()`：Model 用 `scopeXxxParamsQuery()` / `loadSubModelQuery()` 表达关联条件，Dao 的 `beforeBuildFiled()` 负责把用户入参转换成对应关联查询参数；Business 不拼装底层关联查询闭包。
- 如果需要按关联表字段排序、聚合统计或必须依赖数据库层筛选结果，再在 Dao 中使用 join；普通展示型关联查询默认使用 `with()`，避免无谓 join 扩大查询复杂度。
- 关联返回字段同样遵循“需要什么给什么”：主表 `$columns` 和关联 `$relations` 都必须显式收敛字段，不因方便返回整表字段。

## 完成检查
- 路由、中间件、权限点与同模块风格一致。
- 存在 `config/rbac/**` 权限配置时，已同步补权限组和权限项，并确认 `client_route_name`、`proxy_route_name` 指向对应 `WebRoute`。
- 新增或修改的方法已补 PHPDoc，且包含方法说明、`@param` 和 `@return`；无入参方法至少保留 `@return`。
- Controller 仅收参、调用 Business、`revert()` 返回。
- Business 完成校验、流程编排、事务边界。
- Dao 封装查询和持久化；涉及结构变更时已补 migration 或说明。
- 提交前针对本次新增/修改模块扫描：`request->all(`、直接 `doQuery(`、`extends Model`、`function mainNo(`、重复 trait、废弃 route/权限/枚举、`refresh()`、空兼容方法、旧字段名；其中 `request->all(` 不是一律禁止，必须确认对应 Business 入口使用 `$params = validator($params, [...])->validate()` 过滤字段。
- 新增或修改文件至少跑 `php -l`；批量新增 Model/Dao/migration 时用 `find ... -name '*.php' -print0 | xargs -0 -n1 php -l`。

## Controller 约束
- 仅收参、调用 Business、`revert()` 返回。
- 推荐构造器注入：`__construct(protected Request $request, protected XxxBusiness $business)`。
- Controller 方法必须补 PHPDoc，写明路由参数和 `JsonResponse` 返回；构造器注释写明注入的 Request 和 Business。
- 不在 Controller 写复杂业务分支。
- Controller 传入 Business 的数组参数：如果 Business 方法入口第一步会用 `$params = validator($params, [...])->validate()` 定义并过滤允许字段，优先直接传 `$this->request->all()`，由 Business validator 控制最终字段；如果 Business 不做入口过滤，Controller 必须用 `$this->request->only([...])` 白名单。单字段接口可用 `$this->validate()` 后只传字段值，但若 Business 已统一校验数组入参，也应保持 `all()` + Business validator 的模式。
- 新增接口方法优先命名为 `store`，编辑/保存修改接口优先命名为 `update`；route path、Controller、Business、`WebRoute` 常量命名保持一致。
- 管理端写操作通常透传 `management_auth_info()`。
- 管理端路由通常配 `auth:jwt-management` + `WebRoute::*` 权限点，保持与 `routes/management/proxy/**` 同模块文件一致。
- 权限控制通常依赖 route 的 `as => WebRoute::*` 和 `config/rbac/**` 初始化配置；不要误把“补权限配置”等同于新增路由中间件，除非同模块明确这么做。

## Business 约束
- 承担参数校验、业务编排、事务控制。
- Business 方法必须补 PHPDoc，写明已校验前/后的业务参数含义和返回数组结构；校验、格式化、事务组装等私有方法也必须写 `@param`/`@return`。
- 常用 `validator(...)->validate()` 做规则校验；当 Controller 传入 `$this->request->all()` 时，Business 必须写成 `$params = validator($params, [...])->validate()`，并只把过滤后的 `$params` 继续传给 Dao/后续逻辑，不能忽略 validate 返回值。
- 枚举型参数优先使用 `Rule::in(XXX::all())`。
- 普通关联编号默认只做必填/格式校验，例如 `xxx_no => required|string`；不要主动查库校验存在性，除非需求明确指定。确需存在性校验时放在 `validator` 中，用 `Rule::exists($this->xxxDao->getTableName(), 'xxx_no')`，表名从 Dao 的 `getTableName()` 获取，不要写死；软删表追加 `whereNull('deleted_at')`。
- 列表/详情/子资源接口的查询字段和关联在方法内先单独赋值为 `$columns`、`$relations` 后再传入 Dao，即使字段较少也不要把 columns/relations 数组直接内联在 Dao 调用中；不要仅为了复用少量字段/关联而抽 `getColumns()`、`getRelations()` 这类方法，除非字段很多、逻辑复杂或同模块已有这种固定写法。
- 从 Dao 返回的 Collection 中提取编号数组时，在 Business 中使用 `$rows->pluck('xxx_no')->filter()->unique()->toArray()`；不要默认追加 `values()->all()`。
- 列表/详情返回枚举字段时，必须同步提供对应 `*_str` 文案字段；优先在 Model 用读取原字段时 `$this->append('xxx_str')` 的 accessor 写法，并在 `getXxxStrAttribute()` 中调用对应常量类 `getName()`。
- 列表/详情返回展示文案字段时，字段归属哪个 Model 就在哪个 Model 里追加：主表字段由主表 Model 的 accessor append，关联表字段由关联 Model 的 accessor append。不要在 Business 里写 `appendXxxListFields()`、`each(fn...)` 之类方法专门补展示字段。
- Business 不负责把关联表展示字段扁平化到主表行上，除非接口契约明确要求；默认保留关联结构，例如关系表的 `start_date` 由关系 Model 返回 `start_date_str`。
- 如果确实存在非展示性的批量后处理，优先在赋值后用清晰的 `foreach` 表达，不用 Collection `each(fn...)` 包装复杂逻辑；但展示字段优先回到 Model accessor。
- 列表/详情返回文件 ID 字段时，必须同步提供对应 `*_url` 文件访问地址字段；命名去掉字段末尾的 `_id` 后追加 `_url`，例如 `file_id` 返回 `file_url`、`avatar_id` 返回 `avatar_url`、`common_duty_pic_id` 返回 `common_duty_pic_url`。优先在 Model 用读取原字段时无条件 `$this->append('xxx_url')`，并在 `getXxxUrlAttribute()` 中用 `file_url($this->xxx_id)` 处理，空值返回空字符串。
- 复杂入参场景建议补 `customAttributes` 优化报错提示。
- 跨表写入必须明确事务边界（`DB::transaction` 或 `app('db')->transaction`）。
- 事务范围要尽量小：参数校验、读库查询、存在性校验、日期/状态前置校验、待写入数组组装，默认放事务外；事务内只放必须一起提交/回滚的写操作、日志、快照、同步更新。
- 事务内避免为了刷新统计再读库；能用事务外已读取的数据计算的字段，先组装 `$updateData`，事务内直接 `update()` 并必要时 `$model->fill($updateData)`。
- 需要对数量字段做 `+1`、版本推进、领取次数等自增落库时，Business 只负责计算业务状态和日志快照，具体自增更新封装到 Dao，优先使用 `DB::raw('field + 1')` 或同项目既有原子更新方式；如后续日志需要 afterData，可在 Business 用已知增量 `fill()` 同步当前模型内存态，不要再 `save()` 一次。
- Business 不拼装底层组合查询参数。列表筛选中从用户字段转换成 `xxx_params`、关联筛选参数等逻辑应放在对应 Dao 的 `beforeBuildFiled()`。
- Business 可以负责汇总多个 Dao 的查询结果并计算业务公式，例如先从关系 Dao 取业务编号数组，再分别调用客户 Dao、订单/保单 Dao、员工 Dao 获取统计或展示数据，最后用 `keyBy()`/`foreach` 合并；不要把跨多个职责表的统计查询硬塞进某一个 Dao。
- 写操作要校验业务前置状态，例如已解散/禁用/已完成的实体不可继续编辑、加人、移除或流转。
- 外部 API 调用与事件触发只保留在业务编排层。

## Dao 约束
- 封装查询、分页、落库、局部更新。
- Dao 方法必须补 PHPDoc，写明确定参数的业务含义、查询字段/关联参数、返回 Model/Collection/LengthAwarePaginator/void 等结果；为兼容测试替身或历史子类，返回类型可只写在 PHPDoc 中，不强制写 PHP 方法返回类型。
- 复用项目基础 Dao 能力（如 `getList/getPageList/findBy...`）。
- 查询条件优先复用 Model 的 `scopeXxxQuery`、`scopeXxxLikeQuery`、时间范围 scope；只为需求确认的筛选字段补 scope。
- 常规列表优先让 Business 组 `$params` 后调用 `getList/getPageList`；不要为简单列表额外写自定义 Dao 方法。
- 子资源/Tab 资料接口如果只是按父级业务编号（如 `product_no`）读取子表数据，应直接调用子资源 Dao 按该外键查询；不要为了取子资源而先查父 Model 再通过关系读取，除非需要校验父实体状态或依赖父表字段。
- Dao 中确定参数的查询方法不要走通用 `$params + doQuery()`，应使用 `newBuilder()->select($this->getSelectColumns($columns))->with($relations)->XxxQuery(...)->first/get()`。
- 确定参数的方法签名要直接表达参数含义，例如 `array $employeeNos, string $realNameLike = '', array $columns = []`，不要用泛化 `$params` 兜底；查询字段必须通过 `array $columns = []` 参数从 Business 传入，不在 Dao 方法内部写死 `['xxx_no', ...]`。
- Dao 查询方法一般返回 Model 或 Collection，不为了方便在 Dao 里 `pluck()` 编号数组；编号提取、`filter()`、`unique()`、`toArray()` 放在 Business 编排层处理。
- 每个 Dao 默认只负责自身模型/主表的查询和统计；需要查询不同职责表时，拆到各自 Dao，再由 Business 汇总。比如关系 Dao 只取关系表编号，员工展示信息由 EmployeeDao 查，客户数量由 CustomerDao 统计，订单/保单金额由对应 Dao 统计。
- Dao 查询优先使用模型 `newBuilder()` 和 Model scope，不优先使用 `DB::table()`；除非处理无模型临时表、复杂 union、数据库特定能力或现有模块已有明确例外。使用模型 builder 时避免随意 `from('table as alias')`，以免和 SoftDeletes 全局作用域产生表别名问题。
- 统计类 Dao 方法可使用 `$columns = [...]` + `selectRaw(implode(',', $columns))` + `groupBy(...)` 表达聚合字段，但筛选条件仍优先通过 Model scope 表达；聚合 Raw 字段应为固定 SQL 片段，不拼接用户输入。
- 跨表 join 只在该 Dao 的主模型查询确实需要关联表字段进行统计时使用；关联表筛选条件优先封装为主模型 scope（如 `CustomerAddTimeGteQuery`、`CustomerNotDeletedQuery`），避免 Dao 方法里散落 `where/whereNull/whereBetween`。
- 如果业务明确要求不用 join 或跨不同职责表取值，按“Dao 分别查询自身表，Business 汇总编号再查下一张表”的两步/多步方式实现。
- `beforeBuildFiled(Builder $query, array $params): array` 用于整理组合筛选参数，例如把 `team_member_name` 转成 `team_bind_user_params`，把员工姓名/工号/是否在职转成 `employee_params`。
- 禁止 SQL 字符串拼接；Raw 语句必须参数绑定。

## Model 约束
- 新增业务 Model 优先继承 `App\Kernel\Base\BaseModel`，并用 `protected string $mainNoColumn = 'xxx_no';` 定义唯一编号，不再覆写 `mainNo()`。
- Model 的 relation、accessor、scope 方法必须补 PHPDoc，写明 `Builder`/业务值入参和 `Builder`/Relation/展示字段返回。
- 继承 `BaseModel` 后不要重复 `use SoftDeletes, ModelTimeTraits, ModelMainNoTrait`。
- 不为了普通日志时间覆盖 `setUpdatedAt()`；如果业务字段如 `action_at` 需要记录操作时间，优先在 Business 写日志数据时显式赋值。
- 查询条件放到 Model scope：精确筛选用 `scopeXxxQuery`，模糊筛选用 `scopeXxxLikeQuery`，排除类可用 `scopeNotInXxxQuery`，排序字段必须补 `scopeSortByXxxQuery`。
- 常用时间范围、状态、软删关联条件也优先封装为 scope，例如 `scopeAddTimeGteQuery`、`scopeAddTimeLteQuery`、`scopeApprovalStatusQuery`、`scopeCustomerNotDeletedQuery`，让 Dao 方法保持 query 链清晰。
- 关联筛选优先用 `scopeXxxParamsQuery(Builder $builder, array $params)` + `$this->loadSubModelQuery('relationName', $builder, $params)`，避免在 Model 中手写多层 `whereHas` 闭包。
- 本表多字段 OR 查询可以保留专门 scope，例如 `scopeNameOrShortNameLikeQuery`；跨关联组合筛选不要放在 Business 中拼闭包。

## 高风险点清单
- 状态流转要校验前置状态，避免越级更新。
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
