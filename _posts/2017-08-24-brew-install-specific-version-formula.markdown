---
id: brew-install-specific-version-formula
layout: post
title:  使用brew安装指定版本软件
date:   '2017-08-24 23:44 +0800'
permalink: brew-install-specific-version-formula.html
---

使用 `brew` 来安装软件很容易：

```
brew search [TEXT|/REGEX/]
brew install FORMULA...
```

有些软件想安装特定版本，比如 `php5.6`：

```
brew install php@5.6
```

但是有时候会找不到特定的版本，比如想安装 `php56-phalcon 2.0.13` 版本，发现最新的为 `3.1`，并且在 tap 里找不到，可以做如下操作：

```bash
brew log php56-phalcon #找到需要的2.0.13版本的commit id: 1602109
brew uninstall/unlink php56-phalcon #先卸载或者unlink
cd $(brew --prefix)/Homebrew/Library/Taps/homebrew/homebrew-php
git checkout 1602109 Formula/php56-phalcon.rb
brew install php56-phalcon
brew pin php56-phalcon #固定
brew list --pinned
git reset --hard
```

接下来就可以查看是否安装正确了：

```
brew info php56-phalcon
```

上面的步骤应该可以安装其他软件的历史版本。

请注意，[Homebrew/php](https://github.com/homebrew/homebrew-php) 这个 tap 已经被 brew [废弃了](https://brew.sh/2018/01/19/homebrew-1.5.0/)，所以 php56-phalcon 这个包已经在brew里面搜不到了，本文只做例子展示指定小版本的安装。
