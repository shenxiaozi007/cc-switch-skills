# AGENTS.md

## 项目说明

本项目为 Vue3 + Vite + TypeScript 前端项目，适合 5 人左右前端团队协作开发。

默认技术栈：

- Vue3
- Vite
- TypeScript
- Pinia
- Vue Router
- Axios
- Element Plus
- SCSS
- Vitest
- ESLint
- Prettier

## 工作语言

所有分析、解释、代码注释、文档说明、Bug 原因说明、验证步骤必须使用简体中文。

## 回复格式

处理开发任务时使用：

1. 需求分析
2. 实现方案
3. 代码实现
4. 注意事项
5. 验证方式

## 开发原则

遵循：

- 简单优先
- 可读性优先
- 类型安全优先
- 最小改动原则
- 单一职责原则
- 避免重复代码
- 避免过度设计
- 优先使用全局组件、hooks、utils
- 使用ElementPlus组件查看是否全局引入
- 新增或修改 hooks、方法、业务逻辑时，必须添加标记注释，说明用途、触发时机、关键输入输出或业务含义

禁止：

- 随意重构无关代码
- 引入无必要依赖
- 使用 any 逃避类型设计
- 提交生产环境 console.log


## 方法、Hooks、业务逻辑注释规范

新增或修改业务代码时必须添加简体中文标记注释：

- 方法上方使用 `// 方法：`，说明方法用途、输入输出或调用场景。
- hooks 上方使用 `// Hook：`，说明封装的状态、行为和复用场景。
- 核心业务分支上方使用 `// 业务逻辑：`、`// 权限判断：`、`// 状态处理：` 等前缀，说明业务原因。
- `watch`、`computed`、生命周期、副作用、接口请求、表单提交、路由跳转、缓存读写都属于需要标记的业务逻辑。
- 禁止添加无意义注释，例如 `// 点击`、`// 处理数据`；注释必须解释业务意图。

示例：

```ts
// 方法：提交用户表单，统一处理新增和编辑两种业务场景
const submitUserForm = async () => {
  // 业务逻辑：编辑场景必须携带用户 id，新增场景由后端自动生成
  const payload = buildSubmitPayload();
  await saveUser(payload);
};
```

## 目录结构

```text
src
├── api
├── assets
├── components
│   ├── base
│   └── business
├── hooks
├── constants
├── directives
├── layouts
├── config
├── router
├── stores
├── styles
├── types
├── utils
└── views
```

## 页面规范

页面放在 `src/views` 下，按业务模块拆分：

```text
src/views/user
├── index.vue
├── components
├── hooks
├── types.ts
├── utils
└── constants.ts
```

规则：

- 页面入口统一使用 `index.vue`
- 页面内组件放在当前模块的 `components` 目录
- 页面内 hooks 放在当前模块的 `hooks` 目录
- 页面内 util 放在当前模块的 `utils` 目录
- 当前模块专属类型可放在 `types.ts`
- 跨模块复用组件放到 `src/components`
- 跨模块复用类型放到 `src/types`
- api接口类型放到 `src/types/api`
- 不允许随意跨页面引用其他页面的私有组件

## 组件规范

统一使用：

```vue
<script setup lang="ts"></script>

<template></template>

<style scoped lang="scss"></style>
```

禁止新增 Options API，除非维护旧代码。

基础组件命名：

```text
BaseButton.vue
BaseTable.vue
BaseDialog.vue
```

业务组件命名：

```text
UserTable.vue
UserForm.vue
OrderFilter.vue
```

## Props 规范

必须使用 TypeScript 类型：

```ts
interface Props {
  title: string;
  loading?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  loading: false,
});
```

## Emits 规范

```ts
const emit = defineEmits<{
  submit: [id: number];
  cancel: [];
}>();
```

## TypeScript 规范

禁止使用 `any`。

优先使用：

```ts
interface UserInfo {
  id: number;
  name: string;
}
```

无法确定类型时使用 `unknown` 并进行类型收窄。

## API 规范

接口按模块拆分：

```text
src/api/user.ts
src/api/order.ts
src/api/role.ts
```

定义接口类型放在 `src/types/api` 下，按业务模块组织：

```text
src/types/api
├── user.ts
├── role.ts
└── product.ts
```

统一响应：

```ts
export interface ApiResponse<T> {
  code: number;
  message: string;
  data: T;
}
```

分页响应：

```ts
export interface PageResult<T> {
  list: T[];
  total: number;
  pageNum: number;
  pageSize: number;
}
```

API 示例：

```ts
const BASE_URL = "/common";
// 获取时间戳
export const getTimeNow = () => {
  return request({
    url: `${BASE_URL}/time/now`,
    method: "get",
  });
};
```

规则：

- 页面中不要直接写请求 URL
- API 参数必须声明类型
- API 返回值必须声明类型
- API 必须补充注释说明用途

## Pinia 规范

统一使用 Setup Store。

禁止新增 Vuex 和 EventBus。

## Router 规范

路由必须懒加载：

```ts
component: () => import("@/views/user/index.vue");
```

路由权限放在 `meta.permission`。

## 权限规范

权限码格式：

```text
user:list
user:add
user:edit
user:delete
```

按钮权限：

```vue
<el-button v-permission="'user:add'">
  新增用户
</el-button>
```

禁止使用 `isAdmin` 代替权限码。


## 全局 SCSS 资源强制规则

当生成或修改任何样式代码时，必须先检查项目中的全局 SCSS 文件：

- `src/styles/_mixin.scss` 或 `src/style/_mixin.scss`
- `src/styles/variables.scss` 或 `src/style/variables.scss`
- `src/styles/element.scss` 或 `src/style/element.scss`

规则：

- 优先复用 `_mixin.scss` 中已存在的 mixin，不要凭空发明 `@include` 名称。
- 优先复用 `variables.scss` / `element.scss` 中已存在的 CSS 变量、SCSS 变量和 Element Plus token。
- 如果无法读取这些文件，先说明需要用户提供对应文件，不能假装已经使用项目变量。
- 如果组件直接使用 `@include` 或 `$xxx`，必须确认 `vite.config.ts` 已通过 `css.preprocessorOptions.scss.additionalData` 注入全局 SCSS；未配置时需要补充配置。
- Vite 注入优先使用 `@use "@/styles/variables.scss" as *;` 和 `@use "@/styles/_mixin.scss" as *;`，项目使用 `src/style` 时相应改为 `@/style/...`。

## 样式规范

统一使用 SCSS。

禁止内联样式。

禁止硬编码颜色。

使用 BEM：

```scss
.user-card {
}
.user-card__header {
}
.user-card--active {
}
```

样式优先使用项目 `_mixin.scss` 中已经存在的 mixin。只有确认项目中存在对应 mixin 后，才允许生成 `@include xxx(...)`。

```vue
<style scoped lang="scss">
.card {
  // 示例仅在项目 _mixin.scss 存在这些 mixin 时使用
  @include flex(space-between, center);
  @include text-ellipsis(2);
}
</style>
```

使用 Design Token：

```scss
color: var(--color-primary);
padding: var(--spacing-md);
```

## Loading 规范

所有异步请求必须处理 Loading。

```ts
const loading = ref(false);

async function fetchData() {
  try {
    loading.value = true;
    await getUserList(queryForm);
  } finally {
    loading.value = false;
  }
}
```

## 空状态规范

列表为空时显示空状态：

```vue
<el-empty v-if="!loading && list.length === 0" />
```

## 错误处理规范

禁止空 catch。

```ts
try {
  await createUser(form);
} catch (error) {
  console.error(error);
  ElMessage.error("请求失败");
}
```

## 日志规范

提交前删除 `console.log`、`console.table`、`console.dir`。

生产代码允许 `console.error` 输出异常。

如果项目有 logger，统一使用 logger。

## 注释规范

公共函数必须写注释：

```ts
/**
 * 获取用户列表
 * @param params 查询参数
 * @returns 用户分页数据
 */
```

复杂逻辑必须说明原因。

禁止无意义注释。

## 安全规范

禁止直接使用 `v-html` 渲染不可信内容。

禁止把密钥、Token、账号密码写入前端代码。

## 性能规范

- 路由必须懒加载
- 大组件可异步加载
- 列表超过 100 条考虑分页或虚拟滚动
- 图片资源应压缩或懒加载

## Git 提交规范

```text
feat: 新增用户管理页面
fix: 修复登录状态丢失问题
refactor: 重构权限判断逻辑
perf: 优化表格渲染性能
docs: 更新接口说明
style: 调整页面样式
test: 新增用户模块测试
chore: 更新依赖配置
```

## 提交前检查

必须执行：

```bash
yarn lint
yarn type-check
yarn build
```
