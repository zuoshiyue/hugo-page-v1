+++
title = 'Quickstart'
date = 2024-11-25T21:13:30+08:00
menu = ['posts']
tags = ['hugo']
+++

## Hugo 快速开始指引

> 注意：这篇文章依赖 Hugo v0.11 版本的功能。如果你的 Hugo 是早期版本，你需要在此之前 [升级](https://www.gohugo.org/doc/overview/installing/) 你的版本。

## 第一步 安装 Hugo

到 [Hugo Releases](https://github.com/spf13/hugo/releases) 下载适合你的操作系统的版本。

把 `hugo` （或者是 Windows 的 `hugo.exe`） 放到你的 环境变量 `PATH` 所在的目录，因为下一步我们将会用到它。

更加完整的安装指南请参考： [Installing Hugo](https://www.gohugo.org/doc/overview/installing/).

## 第二步 让 Hugo 为你创建一个站点

Hugo 可以创建一个站点框架：

```
$ hugo new site path/to/site
```

接下来的操作，我们将在刚创建的 site 目录里执行所有的命令。

```
$ cd path/to/site
```

新创建的站点目录结构如下：

```
  ▸ archetypes/
  ▸ doc/content/
  ▸ data/
  ▸ layouts/
  ▸ static/
    config.toml
```

当前的站点没有任何内容，也没有进行任何配置。

## 第三步 创建文章内容

> 如果你之前使用了其他的博客平台，比如 Jekyll 、 Ghost 或者 Wordpress ，并且希望转换你的内容，看看这个 [迁移工具](https://www.gohugo.org/doc/tools/#migration-tools) 列表。

Hugo 可以创建一个文章内容页面：

```coffeescript
$ hugo new about.md
```

新创建的文件在目录 `content/` 里，内容如下：

```
+++
date = "2015-01-08T08:36:54-07:00"
draft = true
title = "about"

+++

```

注意 date 字段是根据你创建文章的时间自动生成的。

在 `+++` 下面，用 Markdown 格式写一点内容。 比如：

```markdown
## A headline

Some Content
```

此外，我们再创建另外一篇文章并且同样写一些 Markdown 内容。

```coffeescript
$ hugo new post/first.md
```

新创建的文件会在 `content/post/first.md`

我们仍然缺少任何的模板来显示我们的内容。

## 第四步 安装皮肤

Hugo 对皮肤支持的很好，有大量的现成皮肤可选，并且一直在增加中。 要安装所有的 Hugo 皮肤，只需要 clone 完整的 **hugoThemes** 库到你的工作目录：

```bash
$ git clone --depth 1 --recursive https://github.com/spf13/hugoThemes.git themes
```

## 第五步 运行 Hugo

Hugo 包含了内置的高性能 web server 。通过简单的运行 `hugo server` ， Hugo 将会找一个有效的端口运行服务器用于你的文章。

```
$ hugo server --theme=hyde --buildDrafts
2 of 2 drafts rendered
0 future content
2 pages created
0 paginator pages created
0 tags created
0 categories created
in 15 ms
Watching for changes in /home/user/exampleHugoSite/{data,content,layouts,static,themes}
Serving pages from memory
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

我们指定了两个参数：

+   `--theme` 选择哪个皮肤;
+   `--buildDrafts` 由于想显示我们的内容，包括设置了 draft 草稿状态的内容。

想要了解更多其他的选项，执行：

```
$ hugo help
```

想要了解 server 相关的选项：

```sql
$ hugo help server
```

## 第六步 编辑内容

Hugo 不仅可以运行一个 server ，它还能够监控你的文件变化并自动重新编译你的站点。 Hugo 会和你的浏览器通信并且自动重新加载所有打开的页面。这同样也适用于移动端的浏览器。

按 Ctrl+C 可以停止 Hugo 进程。运行如下：

```
$ hugo server --theme=hyde --buildDrafts
2 pages created
0 tags created
0 categories created
in 5 ms
Watching for changes in exampleHugoSite/content
Serving pages from exampleHugoSite/public
Web Server is available at http://localhost:1313
Press Ctrl+C to stop
```

打开你喜欢的 [编辑器](http://vim.spf13.com/) ，编辑和保存你的文章内容，然后等待 Hugo 重新编译并自动重新加载。

将你的浏览器打开在另外一个显示器上，在你保存时只需要瞄一眼，这非常的高效。你甚至不需要切换到你的浏览器。 大多数情况下，当你看浏览器时，Hugo 已经足够快的刷新成了新的页面。

修改然后保存一个文件，看看命令行里发生了什么：

```sql
Change detected, rebuilding site
2015-11-27 15:13 +0100
2 of 2 drafts rendered
0 future content
2 pages created
0 paginator pages created
0 tags created
0 categories created
in 11 ms
```

## 第七步 玩得开心

最好的学习方法是去使用它。

试试这些：

+   添加一个 [new content file](https://www.gohugo.org/doc/content/organization/)
+   创建一个 [new section](https://www.gohugo.org/doc/content/sections/)
+   修改 [a template](https://www.gohugo.org/doc/layout/templates/)
+   使用 [TOML front matter](https://www.gohugo.org/doc/content/front-matter/) 格式创建一篇文章
+   在 [front matter](https://www.gohugo.org/doc/content/front-matter/) 里定义一个自己的字段
+   显示 [field in the template](https://www.gohugo.org/doc/layout/variables/)
+   创建一个 [new content type](https://www.gohugo.org/doc/content/types/)