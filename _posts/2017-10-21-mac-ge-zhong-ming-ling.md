---
ID: 111
post_title: Mac 各种命令
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=111
published: true
post_date: 2017-10-21 03:20:30
---
显示Mac隐藏文件的命令：
<code>defaults write com.apple.finder AppleShowAllFiles -bool true</code>

隐藏Mac隐藏文件的命令：
<code>defaults write com.apple.finder AppleShowAllFiles -bool false</code>

复制ssh 公钥
<code>pbcopy &lt; ~/.ssh/id_rsa.pub</code>

重命名(移动文件)
<code>mv pathA pathB</code>(输入文件名就是当前路径)