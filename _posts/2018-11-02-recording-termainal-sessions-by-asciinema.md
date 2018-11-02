---
layout: post
title: 终端录屏工具asciinema
date: '2018-11-02 17:38 +0800'
permalink: recording-termainal-sessions-by-asciinema.html
---

## asciinema

在阅读 docker 文档的时候，偶然搜索单词，发现视频区域中的内容可以被高亮，并且能被鼠标选中和复制，发现了工具 [`asciinema`](https://asciinema.org)。

asciinema 发音 \[as-kee-nuh-muh\]，是一个以文本的方式，录制终端交互，并能回放和分享的工具。支持 Linux，macOS，\*BSD。工作原理可以查看 [这里](https://asciinema.org/docs/how-it-works)。

asciinema 项目由互补的几部分组成：

-   基于命令行的终端交互记录者，称之为 asciinema （用于本地录制，回放，上传）
-   具有 api 的网站 asciinema.org （用于分享，上传）
-   javascript 播放器 （嵌套进网页时播放）

## 安装 asciinema

本地只需要安装命令行工具即可，在 Mac 上使用 `brew` 来安装：

```bash
brew install asciinema
```

在这里 [查看更多安装方法](https://asciinema.org/docs/installation)。

## 录制和回放

使用如下命令来开始录制：

```bash
asciinema rec # 录制终端并上传到 asciinema.org
```

这句命令将会产生一个新的 shell 实例，并记录所有终端的输出。当你准备结束完成时，只需输入 `exit` 或者输入 `Ctrl-D`。

使用如下命令开始回放：

```bash
asciinema play demo.cast # 播放一个本地文件
```

还有一些其他命令：

```bash
asciinema rec -t "My git tutorial" # 录制终端并上传到 asciinema.org，并指定标题
asciinema rec demo.cast # 录制终端到本地文件
asciinema rec -i 2.5 demo.cast # 录制终端到本地文件，限制空闲时间最多 2.5 秒（当空闲很长时间时，回放时也不会超过 2.5 秒）
asciinema play https://asciinema.org/a/difqlgx86ym6emrmd8u62yqu8 # 播放  asciinema 上的文件
asciinema cat demo.cast # 打印已记录文件的所有输出
```

## 分享和嵌入

在使用 `asciinema rec` 来开始录制，在结束的时候，会提示是否要存储到云端：

```bash
asciinema: recording finished
asciinema: press <enter> to upload to asciinema.org, <ctrl-c> to save locally
```

回车后会返回一个 url，输入 `asciinema auth` 会返回一个包含唯一识别码的地址，在浏览器输入就可以和自己的账号关联，并对已经存在的记录做操作（修改标题描述，主题，删除，设置公开性等）。

你可以通过点击 asciicast 页面上的 `Share` 按钮，获得三种分享方法：

-   分享链接
-   嵌入图片链接
-   嵌入播放器

### 嵌入图片链接

分享图片链接可以获得一个包含预览图的链接，有 HTML 和 Markdown 两种语法。优点是可以放在不能运行 Javascript 的场合。

```
# HTML:
<a href="https://asciinema.org/a/CtK6uFGprp7M6CvFOAiZ53aIs" target="_blank"><img src="https://asciinema.org/a/CtK6uFGprp7M6CvFOAiZ53aIs.svg" /></a>

#Markdown:
[![asciicast](https://asciinema.org/a/CtK6uFGprp7M6CvFOAiZ53aIs.svg)](https://asciinema.org/a/CtK6uFGprp7M6CvFOAiZ53aIs)

```

### 嵌入播放器

你可以将录制结果嵌入到可以运行 Javascript 的网页或网站上，在网页上嵌入如下代码：

```
<script id="asciicast-CtK6uFGprp7M6CvFOAiZ53aIs" src="https://asciinema.org/a/CtK6uFGprp7M6CvFOAiZ53aIs.js" async></script>
```

应该可以看到如下内容：

<script id="asciicast-CtK6uFGprp7M6CvFOAiZ53aIs" src="https://asciinema.org/a/CtK6uFGprp7M6CvFOAiZ53aIs.js" async></script>

### 自定义回放

播放器支持一些控制播放行为的参数。将它们以参数的形式设置在 url 上（`?speed=2&theme=tango`），或者以 `data-option-name="value"` 的形式嵌入到 script 上（`data-speed="2" data-theme="tango"`）。可选属性如下：

-   `t`：指定从什么时间开始播放，默认值为 0（从头播放），接收的格式为：ss，mm:ss，hh:mm:ss。当 t 被指定时，同时暗含 `autoplay=1`，要阻止这个行为则必须显示的设置 `autoplay=0`
-   `autoplay`：当加载完成时是否自动播放，可选值为： `0 / false`（默认），`1 / true`
-   `preload`：预加载，是否立即获取数据；可选值为：`0 / false`（不要预加载数据，等待用户点击开始），`1 / true`。asciinema.org URL 默认为 1，script 默认为0
-   `loop`：控制是否循环播放，通常与 `autoplay` 结合使用，可选值为：`0 / false`（默认），`1 / true`
-   `speed`：控制回放速度，默认为 1（原始速度播放）
-   `size`：控制终端字体大小，可选值为：`small`（默认），`medium`，`big`
-   `theme`：主题，默认是用户在自己账号上设置的值，如果没有关联用户则为 `asciinema`，可选值为：`asciinema`，`tango`，`solarized-dark`，`solarized-light`，`monokai`
-   `cols`：允许覆盖模拟终端的宽度（以字符记），默认为录制的终端宽度
-   `rows`：允许覆盖模拟终端的高度（以行记），默认为录制的终端高度
