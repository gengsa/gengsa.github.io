---
id: intercept-monitor-https-request-with-mitmproxy
layout: post
title:  使用mitmproxy监听HTTPS请求
date:   '2018-07-11 18:57 +0800'
permalink: intercept-monitor-https-request-with-mitmproxy.html
---

在开发api的时候，想要查看服务端的响应，以及想要参考、抓取别的应用的访问，遇到了工具 `mitmproxy`。[`官网`](https://www.mitmproxy.org/) 介绍：

>mitmproxy 是您的瑞士军刀，用来调试，测试，隐私测量和渗透测试。它可以用来拦截，检查，修改和重放 web 流量，如 HTTP/1，HTTP/2，WebSockets，或者任何其他受 SSL/TLS 保护的协议。

我们可以用到其中的拦截，检查功能。

## 安装 mitmproxy

mitmproxy 介绍了多种安装方式，我在 Mac 上使用 `brew` 来安装：

```bash
brew install mitmproxy # 安装 mitmproxy
```

当然也可以使用 python 来安装：

```bash
sudo apt install python3-pip # Debian 10 或更高, Ubuntu 17.10 或更高
sudo dnf install python3-pip # Fedora 26 或更高
sudo pacman -S python-pip # Arch Linux

sudo pip3 install -U pip # 升级 pip3

sudo pip3 install mitmproxy # 安装 mitmproxy
```

也提供了 windows 平台的安装方法，以及 Docker 镜像，不在此列举。

## 启动 mitmproxy

mitmproxy 的启动很简单，使用如下命令：

```bash
mitmproxy -p 9527 # 在指定端口启动

mitmproxy --listen-host localhost -p 9527 # 绑定在指定地址

# 允许外部连接，默认是阻止外部可访问网络的连接，可以用来在其他设备上连接这个代理
mitmproxy --set block_global=false --listen-host 0.0.0.0 -p 9527
```

mitmproxy 还额外提供了两个命令，mitmweb 和 mitmdump，其中 mitmweb 是一个 web 化的工具，操作起来其实更便利。

```bash
# 启动的 web 工具绑定在8888端口
mitmweb --web-port 8888 --set block_global=false  --listen-host 0.0.0.0 -p 9527
```

程序启动后，给你的设备配上代理就可以访问了：

![配置http代理](/images/mitmproxy/config-http-proxy.png)

访问网络请求后，如图：

![截图](/images/mitmproxy/screenshot.png)

在 mitmproxy 中，可以按 `?` 来进入帮助页面，可以查看每个命令的详细解释。

## 监听 https 请求

上述操作只能监听到 http 请求，如果想要监听 https 请求，还需要安装 CA 证书。给设备配置好已启动的代理后，访问连接 `mitm.it`，如果配置正确的话会看到如下界面，按照步骤安装证书即可：

![安装CA证书](/images/mitmproxy/install-ca.png)

没有配置正确的话也会给出相应的提示。

给你的设备配置上 https 的代理，就可以抓到 https 的请求了。具体更详细的文档请看 [`官网`](https://www.mitmproxy.org/)。
