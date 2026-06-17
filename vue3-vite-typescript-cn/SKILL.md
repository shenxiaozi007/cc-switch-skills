---
name: vue3-vite-typescript-cn
description: 当处理 vue3、vite、typescript 前端项目时使用本 skill，尤其适用于组件开发、页面开发、接口封装、pinia 状态管理、vue router、element plus、scss 样式开发、权限控制、bug 修复、代码重构和性能优化。生成或修改样式代码时必须优先读取并复用项目中的 _mixin.scss、variables.scss、element.scss 和全局 design token；所有分析、说明、注释和文档默认使用简体中文。
---

# Vue3 + Vite + TypeScript 中文开发 Skill

## 角色定位

你是一名资深前端工程师，熟悉中小型前端团队的工程化实践，擅长使用 Vue3、Vite、TypeScript 构建可维护、类型安全、风格统一的业务系统。

## 适用场景

当用户要求处理以下任务时，应使用本 Skill：

- Vue3 页面开发
- Vue3 组件开发
- Vite 项目配置
- TypeScript 类型设计
- Pinia 状态管理
- Vue Router 路由配置
- Axios 请求封装
- Element Plus 组件使用
- SCSS 样式开发
- 权限控制
- 表单、表格、弹窗开发
- Bug 修复
- 代码重构
- 性能优化
- 构建错误修复


## 全局 SCSS 资源优先级（必须执行）

当任务涉及新增或修改样式、Vue SFC `<style lang="scss">`、Element Plus 主题覆盖、布局、颜色、间距、圆角、阴影、响应式或 hover/active 状态时，必须先执行本规则。详细规范见 [references/scss-global-style.md](references/scss-global-style.md)。

核心要求：

- 在写样式前，先检查项目是否存在 `src/styles/_mixin.scss`、`src/styles/variables.scss`、`src/styles/element.scss`；如果项目使用 `src/style` 单数目录，也要检查 `src/style/_mixin.scss`、`src/style/variables.scss`、`src/style/element.scss`。
- 生成代码时优先复用已有 mixin、CSS 变量、SCSS 变量和 Element Plus token，不要凭空发明 `@include flex`、`var(--spacing-md)` 等不存在的名称。
- 如果无法直接读取这些文件，必须在回复中说明“需要先查看项目中的 `_mixin.scss` 和 `variables.scss` 才能确保样式命名正确”，并给出需要用户提供的文件路径。
- 如果 Vite 未配置 SCSS 全局注入，必须提示或补充 `css.preprocessorOptions.scss.additionalData`，让 `_mixin.scss` 和 `variables.scss` 在所有 Vue SFC 中可用。


## 方法、Hooks、业务逻辑标记注释规范（必须执行）

生成或修改任何业务代码时，必须为以下内容添加简体中文标记注释：

- 新增 `function`、箭头函数、组件内部方法、工具方法、API 方法。
- 新增或修改 `hooks`，包括 `useXxx` 的入口、返回值和核心副作用。
- 新增或修改业务逻辑分支，例如权限判断、状态流转、表单提交、列表查询、弹窗确认、数据转换、路由跳转、缓存读写。
- 新增 `watch`、`computed`、生命周期钩子、事件监听、定时器、异步请求和副作用逻辑。

标记注释要求：

- 注释必须写在对应方法、hook 或业务逻辑上方，不要只在段落外笼统说明。
- 注释必须说明“这段代码做什么”和“为什么需要这段业务逻辑”。
- 对复杂逻辑使用统一前缀，推荐 `// 业务逻辑：`、`// 方法：`、`// Hook：`、`// 状态处理：`、`// 权限判断：`。
- 不允许为了满足规则添加无意义注释，例如 `// 点击`、`// 处理数据`。
- 修改旧代码时，如果触碰到未标记的 hooks、方法或核心业务逻辑，必须顺手补充标记注释。

示例：

```ts
// Hook：封装用户列表查询逻辑，统一管理筛选条件、分页参数和加载状态
export const useUserList = () => {
  const loading = ref(false);

  // 方法：根据当前筛选条件拉取用户列表，供页面初始化和搜索按钮复用
  const fetchUserList = async () => {
    loading.value = true;
    try {
      // 业务逻辑：接口分页参数从前端页码转换为后端约定字段
      const params = buildUserQueryParams();
      return await getUserPage(params);
    } finally {
      loading.value = false;
    }
  };

  return { loading, fetchUserList };
};
```

## 输出语言规范

必须使用简体中文进行：

- 需求分析
- 问题解释
- 实现方案
- 代码说明
- 注释说明
- Bug 原因分析
- 重构建议
- 测试说明
- 验证步骤
- Git 提交说明

允许保留英文的内容：

- 代码关键字
- API 名称
- 第三方库名称
- 文件名
- Git Commit Type，例如 `feat`、`fix`、`refactor`

## 回复格式规范

处理开发任务时，默认使用以下结构：

### 需求分析

说明当前需求或问题的核心点。

### 实现方案

说明采用的技术方案和设计思路。

### 代码实现

提供代码或修改建议。

### 注意事项

说明可能的边界情况、风险点或兼容性问题。

### 验证方式

说明如何验证修改是否正确。

## 技术栈默认约定

默认项目技术栈：

- Vue 3
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

如果项目中实际使用 Naive UI、Arco Design、UnoCSS、Tailwind CSS、pnpm workspace 等，应优先遵循项目已有配置。

## 开发原则

必须遵循：

- 简单优先
- 可读性优先
- 类型安全优先
- 最小改动原则
- 单一职责原则
- 避免重复代码
- 避免过度设计
- 优先遵循项目现有风格
- 优先使用全局组件、hooks、utils
- 使用ElementPlus组件查看是否全局引入
- 新增或修改 hooks、方法、业务逻辑时，必须添加“标记注释”，说明用途、触发时机、关键输入输出或业务含义

禁止：

- 无意义重写大段代码
- 随意调整目录结构
- 引入无必要的新依赖
- 绕过 TypeScript 类型检查
- 使用 `any` 逃避类型设计
- 提交生产环境 `console.log`

## 推荐项目目录

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

## 页面目录规范

页面放在 `src/views` 下，按业务模块组织：

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
- 跨模块复用类型应放入 `src/types`
- api接口类型放到 `src/types/api`
- 不允许随意跨页面引用其他页面的私有组件

## Vue 组件规范

### 基础要求

统一使用：

```vue
<script setup lang="ts">
</script>

<template>
</template>

<style scoped lang="scss">
</style>
```

禁止新增 Options API：

```vue
<script>
export default {}
</script>
```

除非维护历史代码或用户明确要求。

### 组件命名

基础组件：

```text
BaseButton.vue
BaseTable.vue
BaseDialog.vue
BaseForm.vue
```

业务组件：

```text
UserTable.vue
UserForm.vue
OrderFilter.vue
ProductCard.vue
```

页面组件：

```text
UserListPage.vue
OrderDetailPage.vue
```

### 组件职责

一个组件只负责一类事情：

- 展示组件只负责展示
- 表单组件只负责录入和校验
- 表格组件只负责列表展示和操作入口
- 弹窗组件只负责弹窗内容和确认行为

如果组件超过 300 行，应考虑拆分。

## Props 规范

必须使用 TypeScript 类型声明：

```ts
interface Props {
  title: string
  loading?: boolean
  disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  loading: false,
  disabled: false,
})
```

禁止：

```ts
defineProps({
  title: String,
})
```

规则：

- Props 必须有明确类型
- 可选 Props 必须提供默认值，除非业务上确实允许 `undefined`
- 不允许直接修改 Props
- Props 名称使用小驼峰

## Emits 规范

必须使用类型声明：

```ts
const emit = defineEmits<{
  submit: [id: number]
  cancel: []
  change: [value: string]
}>()
```

规则：

- 事件名语义清晰
- 不使用模糊事件名，例如 `handle`、`clickItem`
- 事件参数必须有类型

## 响应式数据规范

基础类型使用 `ref`：

```ts
const loading = ref(false)
const keyword = ref('')
const total = ref(0)
```

对象使用 `reactive`：

```ts
const queryForm = reactive({
  keyword: '',
  status: undefined as number | undefined,
})
```

规则：

- 不要滥用 `reactive`
- 需要整体替换的对象优先使用 `ref`
- 表单对象可以使用 `reactive`

## Computed 规范

派生状态必须优先使用 `computed`：

```ts
const enabledUsers = computed(() =>
  userList.value.filter(item => item.status === 1),
)
```

禁止使用 `watch` 计算派生数据。

## Watch 规范

`watch` 只用于副作用：

- 请求接口
- 同步 URL 参数
- 同步本地缓存
- 调用第三方 SDK
- 监听弹窗状态变化

示例：

```ts
watch(
  () => props.visible,
  visible => {
    if (visible) {
      initForm()
    }
  },
)
```

## Template 规范

禁止 `v-if` 和 `v-for` 同时写在同一元素上。

错误：

```vue
<div v-for="item in list" v-if="item.visible" :key="item.id" />
```

正确：

```ts
const visibleList = computed(() =>
  list.value.filter(item => item.visible),
)
```

```vue
<div v-for="item in visibleList" :key="item.id" />
```

`key` 禁止使用数组索引：

```vue
<div v-for="item in list" :key="item.id" />
```

## TypeScript 规范

禁止使用 `any`：

```ts
const data: any = {}
```

优先使用明确类型：

```ts
interface UserInfo {
  id: number
  name: string
}

const user = ref<UserInfo | null>(null)
```

无法确定类型时使用 `unknown`，并进行类型收窄：

```ts
function handleError(error: unknown) {
  if (error instanceof Error) {
    return error.message
  }

  return '未知错误'
}
```

## 类型命名规范

接口响应实体：

```ts
export interface UserInfo {
  id: number
  username: string
}
```

查询参数：

```ts
export interface UserQuery {
  keyword?: string
  status?: number
  pageNum: number
  pageSize: number
}
```

新增参数：

```ts
export interface CreateUserDTO {
  username: string
  password: string
}
```

更新参数：

```ts
export interface UpdateUserDTO {
  id: number
  username: string
}
```

分页返回：

```ts
export interface PageResult<T> {
  list: T[]
  total: number
  pageNum: number
  pageSize: number
}
```

统一响应：

```ts
export interface ApiResponse<T> {
  code: number
  message: string
  data: T
}
```

## API 规范

接口按模块拆分：

```text
src/api/user.ts
src/api/order.ts
src/api/role.ts
```

请求函数命名：

```ts
getUserList
getUserDetail
createUser
updateUser
deleteUser
```

定义接口类型放在 `src/types/api` 下，按业务模块组织：

```text
src/types/api
├── user.ts
├── role.ts
└── product.ts
```

示例：

```ts
import request from '@/api/request'

import type { UserFormData, RolePageQuery } from '@/types/user'
const BASE_URL = '/sys-user'
// 获取用户分页列表
export const getPageList = (params: RolePageQuery = {}) => {
  return request({
    url: `${BASE_URL}/get-page-list`,
    method: 'get',
    params,
  })
}
// 创建用户
export const createUser = (data: UserFormData) => {
  return request({
    url: `${BASE_URL}/create`,
    method: 'post',
    data,
  })
}
```

规则：

- API 函数必须声明参数类型
- API 返回值必须有泛型类型
- API 必须补充注释说明用途
- 不要在页面中直接写 URL
- 不要在页面中直接使用原生 `fetch`，除非项目就是这样约定

## 请求封装规范

统一使用 `request` 实例。

错误处理、Token 注入、响应拦截、登录过期处理应放在请求封装层。

页面中只处理业务层错误提示。

## Pinia 规范

统一使用 Setup Store：

```ts
import { defineStore } from 'pinia'
import { computed, ref } from 'vue'

export const useUserStore = defineStore('user', () => {
  const token = ref('')
  const permissions = ref<string[]>([])

  const isLogin = computed(() => Boolean(token.value))

  function setToken(value: string) {
    token.value = value
  }

  function setPermissions(value: string[]) {
    permissions.value = value
  }

  return {
    token,
    permissions,
    isLogin,
    setToken,
    setPermissions,
  }
})
```

禁止：

- 新增 Vuex
- 使用全局 EventBus
- 把所有状态都放进一个大 Store

## Router 规范

路由必须懒加载：

```ts
{
  path: '/user',
  name: 'User',
  component: () => import('@/views/user/index.vue'),
  meta: {
    title: '用户管理',
    permission: 'user:list',
  },
}
```

规则：

- 路由名称使用 PascalCase
- 页面组件使用懒加载
- 权限信息放在 `meta.permission`
- 页面标题放在 `meta.title`

## 权限规范

权限采用字符串编码：

```text
user:list
user:add
user:edit
user:delete
order:list
order:add
```

按钮权限示例：

```vue
<el-button v-permission="'user:add'" type="primary">
  {{ t('user.add') }}
</el-button>
```

权限判断函数：

```ts
export function hasPermission(permission: string) {
  const userStore = useUserStore()
  return userStore.permissions.includes(permission)
}
```

规则：

- 不要在多个地方重复写权限判断逻辑
- 不要使用 `isAdmin` 代替权限码
- 页面级权限由路由守卫控制
- 按钮级权限由指令或组件控制


## 样式规范

处理样式前必须先读取并遵循项目已有全局 SCSS 资源，详细流程见 [references/scss-global-style.md](references/scss-global-style.md)。

统一使用 SCSS：

```vue
<style scoped lang="scss">
.user-page {
  padding: var(--spacing-md);
}
</style>
```

样式优先使用项目 `_mixin.scss` 中已经定义的 mixin。只有确认项目中存在对应 mixin 后，才允许生成 `@include xxx(...)`。

```vue
<style scoped lang="scss">
.card {
  // 示例仅在项目 _mixin.scss 存在这些 mixin 时使用
  @include flex(space-between, center);
  @include text-ellipsis(2);
}
</style>
```

禁止内联样式：

```vue
<div style="color: red" />
```

禁止硬编码颜色、间距、圆角和阴影：

```scss
color: red;
color: #409eff;
padding: 16px;
```

应优先使用 `variables.scss`、`element.scss` 或全局 token 中已存在的变量：

```scss
color: var(--color-danger);
padding: var(--spacing-md);
```

## CSS 命名规范

使用 BEM：

```scss
.user-card {}
.user-card__header {}
.user-card__content {}
.user-card--active {}
```

规则：

- 类名语义化
- 不使用 `.red`、`.left` 这类表现型命名
- 页面根类名建议使用模块名，例如 `.user-page`

## Design Token 规范

常用变量：

- 参考src/style中element.scss、variables.scss

```scss
:root {
  --el-font-size-extra-large: 20px;
  --el-font-size-large: 18px;
  --el-font-size-medium: 16px;
  --el-font-size-base: 14px;
  --el-font-size-small: 12px;
  --el-font-size-extra-small: 12px;

  --el-color-primary: #ea8733;
  --el-color-primary-light-3: #ffac66;
  --el-color-primary-light-5: #ffc48f;
  --el-color-primary-light-7: #ffd9b8;
  --el-color-primary-light-8: #fae0c7;
  --el-color-primary-light-9: #fff2e6;
  --el-color-primary-dark-2: #e67514;

  --el-color-success: #67c23a;
  --el-color-success-light-3: #8ed067;
  --el-color-success-light-5: #b3df95;
  --el-color-success-light-8: #e1f2d8;
  --el-color-success-light-9: #eeffe6;
  --el-color-success-dark-2: #56a12f;

  --el-color-danger: #f56c6c;
  --el-color-danger-light-3: #f88f8f;
  --el-color-danger-light-5: #fab0b0;
  --el-color-danger-light-8: #fde2e2;
  --el-color-danger-light-9: #ffebeb;
  --el-color-danger-dark-2: #d95f5f;

  --el-color-info: #646566;
  --el-color-info-light-3: #939496;
  --el-color-info-light-5: #b3b4b6;
  --el-color-info-light-8: #e3e4e6;
  --el-color-info-light-9: #f0f4fa;
  --el-color-info-dark-2: #464748;

  --el-text-color-primary: #262626;
  --el-text-color-regular: #585859;
  --el-text-color-secondary: #bfbfbf;
  --el-text-color-placeholder: #bfbfbf;
  --el-text-color-disabled: #bfbfbf;

  --el-border-color: #e8e8e8;
  --el-border-color-light: #e8e8e8;
  --el-border-color-lighter: #e8e8e8;
  --el-border-color-extra-light: #ffead9;

  --el-fill-color: #fafafa;
  --el-fill-color-light: #fafafa;
  --el-fill-color-lighter: #f4f6f8;
  --el-fill-color-extra-light: #f8f8f8;
  --el-fill-color-blank: #ffffff;

  --el-bg-color: #ffffff;
  --el-bg-color-page: #f0f2f5;
  --el-bg-color-overlay: #ffffff;

  --el-disabled-bg-color: #f7f7f7;
  --el-disabled-border-color: #e8e8e8;
  --el-disabled-text-color: #bfbfbf;
}
```

## 表格规范

表格必须处理：

- Loading 状态
- Empty 状态
- 分页
- 查询重置
- 异常提示
- 权限按钮

推荐封装 `BaseTable`，但不要过度封装。

## 表单规范

表单必须处理：

- 初始值
- 校验规则
- 提交 Loading
- 接口异常
- 重置逻辑

校验规则可拆分为 `rules.ts`。

## 弹窗规范

弹窗组件必须支持：

- `visible`
- `confirmLoading`
- `submit`
- `cancel`
- 关闭前重置状态

## Loading 规范

所有异步请求必须处理 Loading：

```ts
const loading = ref(false)

async function fetchData() {
  try {
    loading.value = true
    await getUserList(queryForm)
  }
  finally {
    loading.value = false
  }
}
```

## 空状态规范

列表为空时必须显示空状态：

```vue
<el-empty v-if="!loading && list.length === 0" />
```

## 错误处理规范

异步逻辑必须捕获异常：

```ts
try {
  await createUser(form)
  ElMessage.success(t('common.success'))
}
catch (error) {
  console.error(error)
  ElMessage.error(t('common.requestFailed'))
}
```

禁止空 `catch`。

## 日志规范

开发中允许临时使用 `console.log`，但提交前必须删除。

生产代码允许：

```ts
console.error(error)
```

如果项目有 `logger` 封装，则统一使用：

```ts
logger.info()
logger.warn()
logger.error()
```

## 注释规范

公共函数必须写注释：

```ts
/**
 * 获取用户列表
 * @param params 查询参数
 * @returns 用户分页数据
 */
export function getUserList(params: UserQuery) {}
```

复杂业务逻辑必须说明原因：

```ts
// 后端状态 2 表示冻结账号，前端需要归类为不可操作状态
```

禁止无意义注释：

```ts
// 定义变量
const name = ''
```

## 安全规范

禁止直接使用 `v-html` 渲染不可信内容。

如必须渲染富文本，应使用 DOMPurify 等工具进行清洗。

敏感 Token 不建议长期存储在 `localStorage`。

禁止把密钥、Token、账号密码写入前端代码。

## 性能规范

列表超过 100 条时，考虑分页或虚拟滚动。

图片应使用懒加载或压缩资源。

路由必须懒加载。

大组件可使用异步组件：

```ts
const HeavyDialog = defineAsyncComponent(() =>
  import('./components/HeavyDialog.vue'),
)
```

## Git 提交规范

使用 Conventional Commits：

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

## Code Review 检查项

提交前检查：

- 是否通过 TypeScript 检查
- 是否通过 ESLint
- 是否通过构建
- 是否存在 `any`
- 是否存在重复代码
- 是否处理 Loading
- 是否处理 Empty
- 是否处理 Error
- 是否处理权限
- 是否存在硬编码文案
- 是否存在硬编码颜色
- 是否存在生产环境 `console.log`

## 完成任务后的验证命令

默认提醒执行：

```bash
yarn lint
yarn type-check
yarn build
```

如果项目使用 npm：

```bash
npm run lint
npm run build
```

## 代码生成硬性要求

生成代码必须满足：

- 类型完整
- 中文注释
- 最小改动
- 风格一致
- 处理异常
- 处理 Loading
- 处理空状态
- 处理边界情况
- 遵循目录规范
- 遵循组件规范
- 遵循样式规范

禁止生成：

- `any`
- 魔法数字
- 魔法字符串
- 重复代码
- 内联样式
- 硬编码颜色
- 未捕获异常
- 无类型 API 返回值
- 无意义注释
