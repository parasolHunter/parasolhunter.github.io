---
layout:     post
title:      Windows 多版本 Node 切换：nvm-windows 实践
subtitle:   一台机器兼容 14 / 16 / 18 / 20 等不同项目
date:       2026-09-10
order:      14
section:    tips
author:     Parasol
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Node.js
    - 工程化
    - 前端架构
---

前后端分离项目跨度几年，**Node 版本往往不一致**：老项目锁 14，新项目用 18 或 20。在 Windows 上我使用 **nvm-windows** 管理多版本，比手动改 `PATH` 或装多个安装包可靠得多。

### 一、背景与选型

| 方案 | 优点 | 缺点 |
|------|------|------|
| 官方安装包覆盖安装 | 简单 | 全局只有一个版本 |
| 手动改环境变量 | 无需额外工具 | 易忘、易冲突 |
| **nvm-windows** | 一条命令切换 | 需单独安装，路径有要求 |

Node 官方历史版本列表：https://nodejs.org/dist/

nvm-windows 发布页：https://github.com/coreybutler/nvm-windows/releases

### 二、安装前注意

1. **若已安装 Node，建议先卸载**，避免与 nvm 管理的 symlink 冲突。
2. nvm-windows **建议安装在当前用户目录**，例如 `C:\Users\<用户名>\nvm`，安装向导中 Node  symlink 目录可设为 `C:\Program Files\nodejs`（默认即可）。
3. 安装完成后**新开终端**再执行命令。

### 三、常用命令

```bash
nvm version              # 查看 nvm 版本
nvm list                 # 已安装 Node 列表（* 为当前使用）
nvm list available       # 可安装的远程版本（视 nvm 版本而定）

nvm install 18.20.0      # 安装指定版本
nvm install 16.20.2
nvm use 18.20.0          # 切换当前 shell 使用的版本

node -v
npm -v
```

### 四、国内镜像加速

下载 Node 二进制较慢时，可设置镜像（以 npmmirror 为例）：

```bash
nvm node_mirror https://npmmirror.com/mirrors/node/
nvm npm_mirror https://npmmirror.com/mirrors/npm/
```

> 原淘宝镜像已迁移至 npmmirror，旧 `npm.taobao.org` 地址请勿再使用。

设置后重新 `nvm install <version>`。

### 五、项目级版本约定

仓库根目录增加 `.nvmrc` 或 `package.json` 的 `engines` 字段，团队对齐版本：

**.nvmrc**

```text
18.20.0
```

**package.json**

```json
{
  "engines": {
    "node": ">=18.20.0 <19"
  }
}
```

进入项目后：

```bash
nvm use
# 若 .nvmrc 存在且已安装对应版本，nvm-windows 部分版本支持自动读取
```

未安装则先 `nvm install 18.20.0`。

### 六、与 pnpm / yarn / 全局包

切换 Node 版本后，**全局安装的 CLI 可能需要重装**（npm 全局目录随 Node 变化）。项目内依赖以 lockfile 为准，切换版本后建议：

```bash
rm -rf node_modules
npm ci   # 或 pnpm install
```

### 七、常见问题

| 现象 | 处理 |
|------|------|
| `nvm use` 提示无权限 | 以管理员运行终端，或检查 symlink 目录权限 |
| 切换后仍是旧版本 | 关闭所有终端 / IDE 内置终端，重新打开 |
| 安装失败 | 检查镜像、代理、磁盘路径无中文乱码 |
| 与 Volta / fnm 冲突 | 同一机器只保留一种版本管理器 |

### 延伸阅读

- [Flutter 开发环境配置](/2026/09/09/Flutter开发环境配置指南/)
- [Git 工作流与分支规范](/2026/09/08/Git工作流与分支规范/)
