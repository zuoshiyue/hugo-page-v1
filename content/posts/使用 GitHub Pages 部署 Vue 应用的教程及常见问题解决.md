+++
title = '使用 GitHub Pages 部署 Vue 应用'
date = 2024-11-25T21:13:30+08:00
menu = ['posts']
tags = ['hugo', 'github-page']
+++
## 简单概述

Vue 是一个轻量且[灵活的](https://so.csdn.net/so/search?q=%E7%81%B5%E6%B4%BB%E7%9A%84&spm=1001.2101.3001.7020)前端框架，很多开发者选择将 Vue 应用托管到 GitHub Pages 以进行展示。GitHub Pages 提供了免费且稳定的静态网站托管服务，非常适合部署 Vue 应用。然而，部署 Vue 应用到 GitHub Pages 时，可能会遇到页面不显示或路径错误的问题。本文将详细讲解如何使用 GitHub Pages 部署 Vue 应用，并且针对常见的 "部署后不显示" 问题给出解决方案。

* * *

## 前提准备

+   已安装并配置 Node.js 
+   已有GitHub 账号
+   已创建 Vue 项目

* * *

## 配置 vite.config.js

在使用 Vite 构建 Vue 应用时，为了确保项目资源能够正确加载到 GitHub Pages 上，需要修改 vite.config.js 来设置项目的根路径为当前目录。

编辑项目根目录下的 vite.config.js\`文件，添加如下配置：

```javascript
import { fileURLToPath, URL } from 'node:url'import { defineConfig } from 'vite'import vue from '@vitejs/plugin-vue'export default defineConfig({plugins: [vue(),  ],base: './',resolve: {alias: {'@': fileURLToPath(new URL('./src', import.meta.url))    }  }})'
```

这里的 base: './' 选项是 Vite 的一个配置项，它指定了应用程序的基础路径。在 GitHub Pages 中，资源通常会被部署到 \`<username>.github.io/<REPO\_NAME>/\` 路径下，设置 \`base\` 为 \`'./'\` 可以确保资源路径相对于当前目录，而不是根目录，避免路径错误的问题。

* * *

## 创建 GitHub 仓库并推送代码

确保你已经在 GitHub 上创建了一个仓库，并将本地代码推送到 GitHub 仓库。

在项目根目录执行以下命令：

```bash
git initgit remote add origin https://github.com/<username>/<REPO_NAME>.gitgit add .git commit -m "initial commit"git push -u origin main
```

将 \`<username>\` 替换为你的 GitHub 用户名，\`<REPO\_NAME>\` 替换为你的仓库名。

* * *

## 配置 GitHub Actions 自动部署

为了让 GitHub Actions 自动将你的 [Vue 项目部署](https://so.csdn.net/so/search?q=Vue%20%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2&spm=1001.2101.3001.7020)到 GitHub Pages，需要在仓库中添加 GitHub机器人密钥并编写 GitHub Actions 的配置文件。

在个人 setting 中找到 developer setting，新建 personal access tokens，勾选 *workflow 权限。复制密钥备用。*  
![](https://i-blog.csdnimg.cn/direct/0dc821f7ffb147ec9ce5b3ab774e74e5.png)  
 

在项目设置中，找到 security -> action 设置密钥，命名应该和workflow配置文件一致。![](https://i-blog.csdnimg.cn/direct/bd0d6157f4f549289cbcd75d54c0cc7f.png)

在项目根目录下创建 .github/workflows/workflow.yml 文件，并添加以下内容：

```XML
name: Deploy to GitHub Pageson:  push:    branches:      - mainjobs:  build:    runs-on: ubuntu-latest    steps:    - name: Checkout code      uses: actions/checkout@v4    - name: Use Node.js      uses: actions/setup-node@v4      with:        node-version: '22'    - name: Install dependencies      run: npm install    - name: Build      run: npm run build    - name: Deploy to GitHub Pages      uses: peaceiris/actions-gh-pages@v4      with:        github_token: ${{ secrets.GH_PAGES_TOKEN }}        publish_dir: ./dist
```

#### 配置说明        

1\. **Checkout 代码**：actions/checkout@v4 用来将你的代码从 GitHub 仓库中检出。  
2\. **设置 Node.js**：actions/setup-node@v4 用来安装并设置 Node.js 环境，指定使用 Node.js 22 版本。  
3\. **安装依赖**：通过 [npm](https://edu.csdn.net/cloud/pm_summit?utm_source=blogglc) install 安装项目所需依赖。  
4\. **构建项目**：通过 npm run build 构建 Vue 项目，构建输出会被放在 dist 目录中。  
5\. **部署到 GitHub Pages**：\`peaceiris/actions-gh-pages@v4 用来将构建好的 dist 目录部署到 GitHub Pages。

* * *

## GitHub Pages 配置

1\. 在你的 GitHub 仓库页面，点击右上角的 Settings。  
2\. 在左侧栏找到 Pages，在 Source 一栏中选择 gh-pages 分支，保存设置。

完成上述步骤后，GitHub Pages 会在你上推代码到 main 分支的时候，自动部署你的应用，通常几分钟内即可在 https://<username>.github.io/<REPO\_NAME>/ 上查看到你的网站。

* * *

## 可能遇到的问题及解决方案

### 1\. 部署后页面不显示或空白

这是 Vue 项目部署到 GitHub Pages 最常见的问题，是由于应用的静态资源（如 CSS 和 JS 文件）路径设置不正确导致的。如果路径没有正确配置，GitHub Pages 在 \`/REPO\_NAME/\` 路径下无法找到资源。

#### 解决方案

确保 `vite.config.js` 中已设置根目录为当前目录，在 \`vite.config.js\` 中设置 \`base: './'\` 以确保所有静态资源都是相对于当前目录来加载的，而不是从根目录。由于默认访问路径为项目根目录，若未设置根目录为当前目录，就会出现直接访问上层根目录的情况，从而出现访问页面空白的情况。

```javascript
import { fileURLToPath, URL } from 'node:url'import { defineConfig } from 'vite'import vue from '@vitejs/plugin-vue'export default defineConfig({plugins: [vue(),  ],base: './',resolve: {alias: {'@': fileURLToPath(new URL('./src', import.meta.url))    }  }})'
```

### 2. 404 页面找不到问题

当你使用 Vue Router 的 history 模式时，刷新页面或者直接访问子路径（如 /about）时可能会出现 404 错误。

#### 解决方案

Vue Router 的 history 模式会导致 GitHub Pages 无法处理非根路径的 URL。可以考虑改用 hash 模式，或者在 vite.config.js 中使用 build.rollupOptions 来处理这个问题。

+   使用 hash 模式，在构建路由的时候把 createWebHistory 修改为 createWebHashHistory。

```php
const router = createRouter({history: createWebHashHistory(import.meta.env.BASE_URL),routes: [    {path: '/',name: 'home',component: Home,    }  ],})
```

+   ​​​​​​在 vite.config.js 中使用 build.rollupOptions 来处理这个问题。

```javascript
import { defineConfig } from 'vite'import vue from '@vitejs/plugin-vue'export default defineConfig({plugins: [vue()],base: './',build: {rollupOptions: {output: {manualChunks: undefined,      }    }  }})'
```

### 3\. 页面访问时的[缓存问题](https://edu.csdn.net/cloud/pm_summit?utm_source=blogglc)

在部署新版本后遇到旧页面缓存问题，新修改的数据不显示。

#### 解决方案

尝试清除浏览器缓存，或者在项目中使用[版本控制](https://edu.csdn.net/cloud/pm_summit?utm_source=blogglc)的策略，确保静态资源带有版本号以避免缓存问题。

* * *

## 总结

通过 GitHub Actions [自动化](https://edu.csdn.net/cloud/pm_summit?utm_source=blogglc)部署 Vue 应用到 GitHub Pages，大大简化了手动部署的流程。通过正确配置 vite.config.js 中的 base 路径，可以解决部署到 GitHub Pages 后页面空白的问题。

总结步骤：  
1\. 在 vite.config.js 中设置  base: './'。  
2\. 使用 GitHub Actions 自动化部署 Vue 应用到 GitHub Pages。  
3\. 配置 GitHub Pages 的源为 gh-pages 分支。  
4\. 如果遇到路由或资源路径问题，通过修改 vite.config.js 和路由模式来解决。

这样，你的 Vue 应用就可以顺利运行在 GitHub Pages 上，并且可以通过 GitHub Actions 实现持续集成和自动化部署。
