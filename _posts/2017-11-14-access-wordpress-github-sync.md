---
ID: 124
post_title: Access WordPress-GitHub-Sync
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=124
published: true
post_date: 2017-11-14 09:55:03
---


# WordPress GitHub Sync

> Let's do IT

## 前言(发牢骚)
我一个弟兄帮我搭了个centOS的服务器, 还帮我搭了一个WordPress=.=, 这个WordPress还支持MarkDown, 什么JB都帮我做完了. 还告诉我MWeb可以绑定WordPress, 可以在上面写完文章之后可以直接push到服务器上, 然后还能在MWeb原文上修改完继续push到服务器, 可以同步修改! 我曹. 简直爽到死. 好好用. 

但是我在公司写完一些md之后, 回到家还想继续完善, 坑爹的事情来了=.=. 我怎么能像git一样. 把服务器的东西先clone下来. 再慢慢修改. 因为我家电脑没那些md, 真是尴了个尬. 重新弄一个一模一样的md从MWeb push上去, 结果在服务器创建了一篇新的md, 没办法覆盖原来的. 问了一下那个帮我搭服务器的弟兄, 他也说没辙=.=

不行, 不能用. 我又不是只在公司写md.

那也没办法, 我又不知道要怎么搞. 所以就放弃了一段时间. 

到了上个星期, 我受不了了, 直接在WordPress上写md太麻烦, 没什么事我都懒得上去写, 主要是为了zhuang bi, 顺便记录下自己学的东西才做了这个blog的, 每次写东西都叫上我去写, 网页又不好用. 不行. 要解决这个问题. 看看有没有办法把在WordPress上做个GIT仓库----`git init`就好啦

## 正文
> 环境: MacOS
> 文章学习: 
> [WordPress GitHub Sync](https://cn.wordpress.org/plugins/wp-github-sync/) 
> [GitHub Development](https://developer.github.com/v3/guides/)

上Google搜一下, WordPress GIT, 第一个就蹦出, WordPress GitHub Sync. -- WordPress与GitHub同步!! 好东西啊(不发牢骚了, 直接写步骤)

教程-->[WordPress GitHub Sync](https://cn.wordpress.org/plugins/wp-github-sync/)

#### 1. 给WordPress安装WordPress GitHub Sync插件
> 这个方法很多, 一般安装都是直接在WordPress的仪表盘(Dashboard)里找. 这里介绍一个比较zhuang bi的办法=.=

1. 在你电脑把WordPress GitHub Sync插件下载下来(一个zip包). 
2. 把zip包上传到服务器, 使用sftp
    1. `sftp -oPort=12312 root@host`
    2. 输入密码, 然后如下: 
    4. ![terminal_sftp_put](https://leezix13.com/wordpress/wp-content/uploads/2017/11/terminal_sftp_put.png)
3. 然后把zip包解压到这个路径下`/wp-content/plugins/`(这里我不懂官方要我解压到`/wp-content/plugins/ directory`, 可是我没这个路径, 就算创建了directory, 把插件放进去也没生效=.=, 可能是我小伙伴安装WordPress方式不对😝)
4. 然后去你的WordPress Dashboard就激活就好啦.

#### 2. 配置你的WordPress GitHub Sync插件
> 这步我搞得最久!!

1. 去这个网址, 创建你的OAuth Token
    1. [Create OAuth Token](https://github.com/settings/tokens/new)
    2. 这个相当于生成一个字符串, 当你的账号密码使用, 待会需要你给这个字符串赋予什么功能
    3. ![create_oauth_token](https://leezix13.com/wordpress/wp-content/uploads/2017/11/create_oauth_token.jpg)
    4. 其他scopes可以不用选, 必须要选public_repo, 然后拉倒最下面直接create就好了.
    5. 最后生成一个40位的字符串. 要把它记下来, 贴在其他地方都好. 因为关了这个页面你就再也见不到它了.
2. 配置你的WordPress
    1. 在你的Dashboard里, setting->GitHub Sync
    2. ![WordPress_setting](https://leezix13.com/wordpress/wp-content/uploads/2017/11/WordPress_setting.png)
    3. ![WordPress_setting_content](https://leezix13.com/wordpress/wp-content/uploads/2017/11/WordPress_setting_content.png)
    4. 没有仓库的就去创建仓库, 我等穷人只能创建public的, 你也可以创建private, 创建完之后, 你自己随便改点东西, push上去, 使得他有个init commit, (网上是这么说的, 不知道我有没有理解错)
    4. 我之所以说我用了太多时间, 是因为我第一项就填错了, 填成https://github.com, 大家不要学我, 我是看了这个才知道我填错了: [GitHub Development](https://developer.github.com/v3/guides/)

3. 配置Webhook

> Webhook是什么? 有待探讨哈

1. 去你的GitHub的repo地址(不是在骂人), 找到setting, 找到Webhook, 然后设置, 如下
2. ![GitHub_Webhook_setting](https://leezix13.com/wordpress/wp-content/uploads/2017/11/GitHub_Webhook_setting.png)
最后大功告成, 回到WordPress setting GitHub Sync的页面, 点击Export to GitHub, 然后你的空仓库就会多了点东西了, 然后你的文章都在_posts文件夹里
![after_export](https://leezix13.com/wordpress/wp-content/uploads/2017/11/after_export.png)
但是呢, 你会发现, 这特么的都是HTML格式的, 老子要写MarkDown, 不是HTML!

淡定, 再装一个插件就好.

>但实际上, 此时GitHub的repo已经和你的WordPress连在一起了, 不信你可以挑一个文件, 轻轻修改一下, push上去. 刷新一下你的WordPress也会跟着修改的.

### 安装 WP-Markdown插件
> 从GitHub到出MarkDown格式的文件.

这个插件在你的Dashboard上是搜索不到的, 只能去这里, 把它zip包下载下来, 然后解压, 就像刚才装WordPress GitHub Sync一样.
地址: [WP-Markdown](https://wordpress.org/plugins/wp-markdown/)

成功安装之后...就成功了=.=, 再去export to github一下. 就好了. _posts下的文件内容就是MarkDown了.

### 本地创建新的md, push到远端
需要创建一个符合WordPress格式的文件, 才能正常显示在WordPress上的. 格式如下:

```

\-\-\-
post_title: 'Post Title'
layout: post_type_probably_post
published: true_or_false
\-\-\-
Post goes here.

```
layout一般写post, title写文章标题, 去掉单引号, published写true或者false, 表示是否公开

成功推上去之后, 不知道谁会自动帮你添加多一个commit, 你可以看一下他改了什么. 
`git show [commitID]` 大概就是在头文件插多了几条信息而已=.=

大功告成, 以上!




