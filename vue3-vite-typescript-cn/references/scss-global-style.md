# 全局 SCSS 资源使用规范

## 必查文件

处理任何样式相关任务前，先在项目中查找以下文件，优先使用实际存在的路径：

1. `src/styles/_mixin.scss`
2. `src/styles/variables.scss`
3. `src/styles/element.scss`
4. `src/style/_mixin.scss`
5. `src/style/variables.scss`
6. `src/style/element.scss`

如果用户只贴出单个 Vue 文件但没有提供项目文件树，必须提醒用户补充 `_mixin.scss`、`variables.scss` 或相关路径；在无法查看这些文件时，不要声称已经使用了项目变量。

## 生成样式代码前的检查流程

1. 先读取 `_mixin.scss`，提取已有 mixin 名称、参数顺序和使用示例。
2. 再读取 `variables.scss`，提取 CSS 变量、SCSS 变量、颜色、间距、字号、圆角、阴影、z-index 等 token。
3. 如涉及 Element Plus 主题，读取 `element.scss`，复用 `--el-*` 变量。
4. 写 Vue SFC 样式时，只使用已经确认存在的 mixin 和变量。
5. 对不确定的变量名，使用“根据项目变量替换为 xxx”这类占位说明，不要凭空发明。

## Vite 全局注入要求

如果组件内直接使用 `@include` 或 SCSS 变量 `$xxx`，项目必须通过 Vite 注入全局 SCSS。检查 `vite.config.ts` 或 `vite.config.js` 是否包含：

```ts
import { fileURLToPath, URL } from 'node:url';
import { defineConfig } from 'vite';

export default defineConfig({
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `
          @use "@/styles/variables.scss" as *;
          @use "@/styles/_mixin.scss" as *;
        `,
      },
    },
  },
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url)),
    },
  },
});
```

如果项目使用 `src/style`，把路径改为：

```scss
@use "@/style/variables.scss" as *;
@use "@/style/_mixin.scss" as *;
```

不要在每个 Vue 文件里重复 `@use`，除非项目没有配置 `additionalData` 且用户明确要求局部引入。

## Sass 写法约束

- 优先使用现代 Sass `@use ... as *`。
- 不新增已废弃的 `@import`，除非项目历史代码已统一使用且用户要求保持一致。
- 不把会输出真实 CSS 的文件放进 `additionalData`，否则会在每个组件中重复输出样式。
- `_mixin.scss` 和 `variables.scss` 应只包含变量、mixin、function 等不会直接输出 CSS 的内容。

## 回复要求

样式相关回复必须说明：

- 使用了哪些项目 mixin。
- 使用了哪些项目变量或 token。
- 是否需要补充或检查 Vite 的 `css.preprocessorOptions.scss.additionalData`。
- 如果无法读取全局 SCSS 文件，必须明确说明当前样式代码只是基于约定的示例，需要以项目文件为准。
