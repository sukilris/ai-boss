<!-- BEGIN:nextjs-agent-rules -->
# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` before writing any code. Heed deprecation notices.
<!-- END:nextjs-agent-rules -->

<!-- BEGIN:project-specific-rules -->
# 项目专属规则

## 技术栈
- Next.js 16.2.9 + React 19.2.4 + TypeScript 5
- Tailwind CSS v4（不使用 `tailwind.config.js`，主题配置在 `app/globals.css`）
- UI 基础：`@base-ui/react` + shadcn/ui（style: `base-maia`）
- 图标库：`@hugeicons/react`
- 工具库：`ahooks`、`zustand`、`class-variance-authority`、`clsx`、`tailwind-merge`

## 组件开发
- shadcn/ui 组件基于 `@base-ui/react` 构建。新增组件应优先使用 `@base-ui/react` 原始组件，并参考 `components/ui/button.tsx` 的封装方式。
- 组件变体使用 `class-variance-authority` (cva) 定义，`className` 合并统一使用 `@/lib/utils` 中的 `cn()`。
- 所有组件文件使用 `.tsx`，props 必须定义 TypeScript 类型。
- 新增的 shadcn 组件必须放在 `components/ui/` 目录下，业务组件放在 `components/` 下。
- 新增组件前先检查 `components/ui/` 是否已有类似实现，避免重复创建。

## 样式规范
- 使用 Tailwind CSS v4 语法，主题变量来自 `app/globals.css`。
- 颜色、圆角、字体等必须使用 CSS 变量/token，例如 `bg-primary`、`text-foreground`、`rounded-4xl`。
- 避免使用任意值如 `bg-[#123456]`，除非设计稿确实没有对应 token。
- dark 模式通过 `.dark` 类切换，已配置好对应变量。
- 字体使用 `font-sans`（Figtree / Geist）和 `font-mono`（Geist Mono）。

## 图标
- 统一使用 `@hugeicons/react` 提供的图标。
- 不要在项目中引入 `lucide-react` 或其他图标库。

## 状态管理
- 全局状态使用 `zustand`，store 文件放在 `stores/` 目录。
- 局部状态优先使用 React `useState` / `useReducer`。
- 通用 hooks 优先使用 `ahooks`，自定义 hooks 放在 `hooks/` 目录。

## 类型
- TypeScript 已启用 `strict` 模式，禁止 `any`。
- 全局类型定义放在 `types/` 目录。
- 组件 props 类型优先在组件文件内定义，跨组件复用的类型放到 `types/`。

## 路径别名
- 使用 `components.json` 中定义的别名：
  - `@/components/ui` 引用 shadcn 组件
  - `@/lib/utils` 引用 `cn`
  - `@/hooks` 引用自定义 hooks
  - `@/lib` 引用其他工具

## Server / Client 组件
- 默认组件为 Server Component。
- 只有使用 `useState` / `useEffect` / `useRef` / 事件监听 / 浏览器 API 时才添加 `"use client"`。
- 数据获取优先在 Server Component 中完成。
- 交互组件可通过 props 接收 Server Component 传递的数据。

## 页面与路由
- 开发业务功能时，如果没有明确要求新开一个页面，应根据业务场景的复杂度、独立性和复用需求，合理选择是新增独立页面，还是在当前页面内通过组件、弹窗、抽屉、Tab 等方式实现。
- 功能相对独立、有独立 URL 需求、需要被分享或收藏的场景，优先新建页面。
- 功能属于同一流程的附属操作、数据量小、上下文强依赖的场景，可在当前页面内通过组件或交互容器实现。
- 避免为每一个小操作都新建页面，也避免把多个独立业务流程强行塞进同一个页面。
- 所有页面优先使用 SSG（Static Site Generation）方式生成：
  - 默认将页面实现为静态渲染，避免不必要的动态数据获取。
  - 动态路由应使用 `generateStaticParams` 预生成常见路径。
  - 仅在需要读取用户身份、请求头、Cookie、URL 查询参数等动态信息时，才使用动态渲染。
  - 需要定时更新的静态页面，优先使用 `export const revalidate = N` 实现 ISR，而不是改为动态渲染。

## 新增依赖
- 新增依赖前检查是否已有同类库。
- shadcn 组件优先使用 `pnpm dlx shadcn add <component>` 安装。
- 不要随意替换已确定的核心库版本。
<!-- END:project-specific-rules -->
