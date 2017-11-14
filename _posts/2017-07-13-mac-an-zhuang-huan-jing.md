---
ID: 20
post_title: Mac安装环境
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=20
published: true
post_date: 2017-07-13 08:39:13
---
# 安装环境:

## 安装MAC OS App Store 下载最新系统更新 

## 安装Xcode App Store 下载最新Xcode 

## 安装Homebrew

> 命令行的管理工具, 用于安装cocoapods, node, watchman等等, 网上说等同于MacPort, ***并优于MacPort*** 
*   可以上[Homebrew官网][1]查看安装Homebrew流程, 就一行命令: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

> 安装完Homebrew之后, 就可以使用brew指令 执行`brew -v`, 显示 <pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">Homebrew 1.2.4
Homebrew/homebrew-core (git revision 84bb; last commit 2017-07-12)
</code></pre>

* * *

## 安装cocoapods

> *   gem是系统自带指令
> *   安装完cocoapods之后, 会在~/里多个.cocoapods文件夹

*   安装完cocopods之后, 就可以使用pod指令
*   使用`sudo gem install cocoapods`安装最新cocoapods(我当时用这个, 安装完之后是1.2.1)
*   如果要安装指定版本: `sudo gem install cocoapods -v 1.2.1`
*   安装完毕, 可通过`pod --version`来查看pod版本

#### 疑难 安装完毕后, 走pod install老是报错: Unable to find a specification for ....什么鬼的, 网上说要pod setup, 但是pod setup又超慢!!(根本看不到希望) 

#### 解决方案

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">pod repo remove master
sudo gem uninstall cocoapods
cd ~ & rm -f .cocoapods
</code></pre> 把gem换源, 使用淘宝(= . =) 

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">gem sources --remove https://rubygems.org/
gem sources -a http://ruby.taobao.org/
gem sources -l
</code></pre> 开始安装cocoapods 

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">sudo gem install cocoapods
pod setup
</code></pre> 然后的pod setup就会稍微快一些(起码能看到完成的希望啊

<del>) </del>之后在F项目上, 执行pod install, 就会自动下载'11-repo-ios'(也不知道是什么东西=.=) 
> 以上方案参考[解决CocoaPods各种慢的方案（gem换源+pod repo换源）]<del></del>  (http://hyichao.github.io/ios/2015/12/06/cocoapods-slow.html) ##安装Node 可以上[react_native中文网][2]直接按上面搞, 不过此时你已经安装完Homebrew了, 可以跳过第一步, 安装Xcode也可以跳过了, 此时主要是安装个watchman <pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">brew install watchman
</code></pre>

> 每次使用brew安装东西的时候, 都会先update brew, 要好些时间才会进入砸门想要的安装, 也不知道怎么跳过这步骤=.= 这次我装完之后, 执行`npm -v`, 显示我是5.0.3

 [1]: https://brew.sh/index_zh-cn.html
 [2]: http://reactnative.cn/docs/0.46/getting-started.html