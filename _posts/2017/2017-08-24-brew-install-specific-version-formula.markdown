---
layout: post
title:  使用brew安装指定版本软件
date:   '2017-08-24 23:44 +0800'
categories: brew
---

使用`brew`来安装软件很容易.
```
brew search [TEXT|/REGEX/]
brew install FORMULA...
```

有些软件想安装特定版本, 比如`php5.6`.
```
brew install php56
```

但是有时候会找不到特定的版本, 比如想安装`php56-phalcon 2.0.13`版本, 发现最新的为`3.1`, 并且在tap里找不到, 可以做如下操作:
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

接下来就可以查看是否安装正确了.
```
brew info php56-phalcon
```

上面的步骤应该安装其他软件的历史版本.
