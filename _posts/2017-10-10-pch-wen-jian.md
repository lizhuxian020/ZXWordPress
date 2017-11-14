---
ID: 89
post_title: PCH文件
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=89
published: true
post_date: 2017-10-10 07:33:36
---
在主工程创建pch文件
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/10/create_new_pch.png" alt="create_new_pch" />￼

进行主工程配置
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/10/config_main_project.png" alt="config_main_project" />￼
其中Prefix Header里填写你的pch路径: 
<code>$(SRCROOT)/ZXUIKit_example/PrefixHeader.pch</code>
$(SRCROOT)代表你的主工程目录

如果你想引用Pod工程里的ZXConvenienceCode的interface里的ZXSugarCode.h

<blockquote>
  就是一些pod工程里有方便的全局宏什么的, 你主工程里也想引用, 需要这么做
</blockquote>

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/10/pod_config_pch.png" alt="pod_config_pch" />￼

我就只是做了把Precompile改为yes, Prefix Header里面的内容是自己本来就有的, 就不管=.=

最后结果如下, 可以这么引用了:

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/10/result_pod_pch.png" alt="result_pod_pch" />￼
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/10/main_pch.png" alt="main_pch" />￼
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/10/main_pch_use.png" alt="main_pch_use" />￼