---
layout:     post
title:      Vue 3 + TypeScript 技术栈与工程规范
subtitle:   Vite、Pinia、目录约定、Git 流程与 ESLint/Prettier 配置
date:       2026-09-12
order:      10
section:    tech
author:     Parasol
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Vue
    - TypeScript
    - 前端架构
    - 工程化
---

本文整理一套 **Vue 3 + TypeScript + Vite** 中后台项目的常用技术栈、目录约定与团队协作规范，适用于新成员 onboarding 或团队统一脚手架认知。外部文档保留官方链接；与流程、环境相关的说明尽量指向本站已有文章，方便长期维护。

### 一、快速上手

#### 1. 开发环境

| 工具 | 说明 |
|------|------|
| [Node.js](https://nodejs.org/) | 建议 **18 LTS 及以上**；多项目并存时可参考 [Windows 多版本 Node 切换](/2026/09/10/Windows多版本Node切换/) |
| [pnpm](https://pnpm.io/) | 包管理器，下文命令均基于 pnpm |
| [Visual Studio Code](https://code.visualstudio.com/) / [WebStorm](https://www.jetbrains.com/webstorm/) | 任选其一 |

**推荐 VS Code 插件**

- [Vue - Official (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) — Vue 3 语言支持
- [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [i18n Ally](https://marketplace.visualstudio.com/items?itemName=Lokalise.i18n-ally) — 国际化辅助

#### 2. 常用命令

```bash
# 全局安装 pnpm（若尚未安装）
npm install pnpm -g

# 安装依赖
pnpm install

# 本地开发
pnpm dev

# 生产构建
pnpm build
```

### 二、技术栈

#### 开发与构建

- [Vite](https://vitejs.dev/) — 开发与构建工具

#### 前端框架与生态

| 类别 | 选型 |
|------|------|
| 框架 | [Vue 3](https://vuejs.org/) |
| 状态管理 | [Pinia](https://pinia.vuejs.org/) |
| 路由 | [Vue Router](https://router.vuejs.org/) |
| UI 组件库 | [Arco Design Vue](https://arco.design/vue/component/select) |
| 语言特性 | [ES6+](https://es6.ruanyifeng.com/) |
| 样式 | [Tailwind CSS](https://tailwindcss.com/docs/installation) |
| HTTP | [Axios](https://axios-http.com/) |

### 三、项目目录

```text
.
├── dist/                    # 构建产物
├── .vscode/                 # IDE 配置
├── mock/                    # Mock 数据
├── public/                  # 静态资源（不经 Vite 处理）
├── src/
│   ├── api/                 # 接口封装
│   ├── assets/              # 未编译静态资源
│   ├── components/          # 公共组件
│   ├── hooks/               # 组合式函数 / 公共逻辑
│   ├── config/              # 常量、项目与插件配置
│   ├── layouts/             # 布局组件（框架级，慎改）
│   ├── libs/                # 未走 npm 的三方库
│   ├── plugins/             # 全局插件（含 i18n 等）
│   ├── router/              # 路由
│   ├── store/               # Pinia 状态
│   ├── styles/              # 全局样式
│   ├── views/               # 页面视图
│   ├── App.vue              # 根组件
│   └── main.ts              # 入口（TS 项目）
├── node_modules/
├── .editorconfig
├── .env / .env.*            # 环境变量
├── .eslintrc.js
├── .eslintcache             # ESLint 缓存，可显著缩短二次校验时间
├── .prettierrc.js
├── vite.config.ts
├── pnpm-lock.yaml
├── package.json
├── Dockerfile
├── tailwind.config.js
└── tsconfig.json
```

约定：**业务代码只写在对应模块目录内**；`layouts`、构建配置等基础设施变更需经评审。

### 四、Git 工作流

分支策略、发布与热修复流程见本站 **[Git 工作流与分支规范](/2026/09/08/Git工作流与分支规范/)**。

IDE 内日常操作（fetch、stash、merge 冲突处理等）可参考 **[git 流程规范及使用指导](/2019/02/28/git流程规范/)**。

### 五、代码规范

项目启用 **ESLint + Prettier**，扩展配置示例：

```javascript
extends: [
  'plugin:vue/vue3-recommended',
  'plugin:tailwindcss/recommended',
  'plugin:prettier/recommended',
]
```

风格细节遵循 [Vue 官方风格指南](https://cn.vuejs.org/style-guide/)。

#### VS Code 推荐设置

保存时自动格式化，并与 ESLint 联动：

```json
{
  "git.enableSmartCommit": true,
  "git.autofetch": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.requireConfig": true,
  "editor.formatOnSave": true,
  "editor.formatOnSaveMode": "file",
  "files.autoSave": "onFocusChange",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "css.validate": false,
  "less.validate": false,
  "scss.validate": false,
  "editor.quickSuggestions": {
    "strings": true
  },
  "files.associations": {
    "*.css": "tailwindcss"
  }
}
```

> 若团队统一 Prettier 版本与配置，避免每人本地格式化结果不一致。

### 六、项目配置

#### 环境变量

在根目录按环境维护 `.env.[mode]` 文件：

| 文件 | 用途 |
|------|------|
| `.env` | 默认 / 公共变量 |
| `.env.dev` | 本地开发 |
| `.env.stage` | 测试环境 |
| `.env.prod` | 灰度 / 生产 |

示例：

```bash
# .env 片段
VITE_APP_GATEWAY_XXX=/XXX
```

只有以 **`VITE_`** 开头的变量会暴露给客户端，在代码中通过 `import.meta.env.VITE_APP_XXX` 访问。

#### 样式与主题

全局样式放在 `src/styles`。主题可通过 **Less 变量** 或 **CSS 变量** 两种方式扩展。

**Vite 中覆盖 UI 库主题（Less）**

```javascript
// vite.config.ts
css: {
  preprocessorOptions: {
    less: {
      modifyVars: {
        'primary-6': '#FC3441',
      },
      javascriptEnabled: true,
    },
  },
},
```

**CSS 变量（运行时换肤更灵活）**

```css
:root {
  --color-primary: #fc3441;
}
```

更多 CSS 变量说明见 [MDN：使用自定义属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)。移动端基础样式备忘可参考 [手机端基础样式（base.css）](/2019/02/28/基础样式css/)。

### 七、国际化

语言包位于 `src/plugins/i18n/locales`。配合 VS Code 的 **i18n Ally** 插件，可在编辑器内预览键值与缺失翻译。

### 八、路由

路由定义在 `src/router`。侧边栏菜单由路由 **`meta`** 驱动，常用字段如下：

```javascript
/**
 * name: 'router-name'     路由名，建议全局唯一
 * meta: {
 *   hidden: true          true 时不在侧边栏展示
 *   title: 'menu.key'       菜单标题（i18n 键名）
 *   icon: 'svg-name'        菜单图标
 *   noCache: true           true 时不被 keep-alive 缓存
 *   activeMenu: '/path'     高亮指定菜单路径
 * }
 */
```

### 延伸阅读

- [Git 工作流与分支规范](/2026/09/08/Git工作流与分支规范/)
- [Windows 多版本 Node 切换](/2026/09/10/Windows多版本Node切换/)
- [防抖与节流策略](/2026/09/11/防抖与节流策略/)（列表搜索、窗口 resize 等场景）
