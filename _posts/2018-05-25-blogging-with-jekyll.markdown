---
id: blogging-with-jekyll
layout: post
title:  使用Jekyll在Github Pages上搭建博客
date: "2018-05-25 15:40"
permalink: blogging-with-jekyll.html
---
最近开始打算搭建一个博客系统来总结和记录自己的学习成果，首先想到了 [`Github Pages`](https://pages.github.com/)。然后在主页看到了使用 `Jekyll` 来作为静态站点生成器，下面来总结一下学习笔记。

## 什么是Github Pages和Jekyll?

Github Pages 是一个静态站点托管服务，可以从 Github仓库 直接托管你的个人、组织或者项目页面。可以使用 Github Pages 默认提供的域名 github.io 或者自定义域名来发布站点。Github Pages 支持纯静态HTML文档，也支持利用 Jekyll 来生成静态页面。只需要推送代码，页面就会被自动构建。

Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和 Liquid 渲染器转化成一个完整的可发布的静态网站，可以发布在任何服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。

下面通过几个步骤来搭建自己的博格。

## Jekyll快速安装

Jekyll 需要 Ruby 运行环境，需要先安装  [`Ruby`](https://www.ruby-lang.org/en/downloads/) 和  [`RubyGems`](https://rubygems.org/pages/download)。以下是一个简单的例子：

```bash
# Install Jekyll and Bundler gems through RubyGems
gem install jekyll bundler

# Create a new Jekyll site at ./myblog
jekyll new myblog

# Change into your new directory
cd myblog

# Build the site on the preview server
jekyll serve

# show drafts
jekyll serve -D

# Now browse to http://localhost:4000
```

如果你希望把 Jekyll 安装到当前目录，你可以运行 `jekyll new .` 来代替。如果当前目录非空，你还需要增添 `--force` 参数，所以命令应为 `jekyll new . --force`。

## 目录结构

基本的目录结构如下：

```bash
.
├── _config.yml
├── _data
|   └── members.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.md
|   └── on-simplicity-in-technology.md
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
|   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
|   ├── _base.scss
|   └── _layout.scss
├── _site
├── .jekyll-metadata
└── index.html # can also be an 'index.md' with valid YAML Frontmatter
```

| 文件/目录 | 描述 |
| - | - |
| _config.yml | [配置](https://www.jekyll.com.cn/docs/configuration/)文件，一定要仔细阅读 |
| _data | 数据配置? |
| _drafts | 草稿 |
| _includes | 可以被包含到文章或布局中重用 |
| _layouts | 模板 |
| _posts | 文章 |
| _sass | 主题的样式文件? |
| .jekyll-metadata | 该文件帮助 Jekyll 跟踪哪些文件从上次建立站点开始到现在没有被修改，哪些文件需要在下一次站点建立时重新生成。该文件不会被包含在生成的站点中。将它加入到你的 .gitignore 文件可能是一个好注意。 |
| index.html | 入口 |
| 其他目录等 | ? |

因为安装了 [`jekyll-admin`](https://github.com/jekyll/jekyll-admin/)，可以访问 `/admin` 来简单的进行后台访问和操作。

## 撰写博客

项目运行起来后，可以在 `_posts` 目录里面编写文章了，虽然 Jekyll 支持多种格式的文档，但是暂时我们先只尝试 `Markdown` 来编写。基本的语法可以查看 [`这里`](http://www.markdown.cn/)。Jekyll 要求一篇文章的文件名遵循下面的格式：

```
年-月-日-标题.MARKUP

# 例如
2018-01-01-this-is-my-first-blog.markdown
2018-01-12-this-is-my-another-blog.textile
```

在编写文章的时候，还需要注意一些额外的事项，比如每个文章必须包含 [YAML头信息](http://jekyllcn.com/docs/frontmatter/)，静态资源的引用，[常用的变量](http://jekyllcn.com/docs/variables/)，[模板](http://jekyllcn.com/docs/templates/)，[插件](http://jekyllcn.com/docs/plugins/)，[主题](http://jekyllcn.com/docs/themes/) 等。

## 部署

作为个人页面，在自己的github里面新建一个以 `yourname.github.io` 命名的项目，其中的 `yourname` 就是你在github上的用户名。给刚刚创建的blog项目添加remote，并推送：

```bash
git add --all

git commit -m "Initial commit"

# 注意替换其中的 yourname
git add remote origin https://github.com/yourname/yourname.github.io

git push -u master
```

现在，可以使用浏览器来访问你的博客了 `https://username.github.io`，以后，每次推送到github的时候，都会被自动部署。还可以在项目的setting页面，来指定项目的某个分支作为该项目的 Github Pages 页面。

在升级 ruby 版本后，运行 `jekyll server -D` 时可能报错，执行 `gem pristine --all` 修复即可。
