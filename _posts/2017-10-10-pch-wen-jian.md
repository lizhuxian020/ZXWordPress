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
在主工程创建pch文件 ![create_new_pch][1]￼ 进行主工程配置 ![config_main_project][2]￼ 其中Prefix Header里填写你的pch路径: `$(SRCROOT)/ZXUIKit_example/PrefixHeader.pch` $(SRCROOT)代表你的主工程目录 如果你想引用Pod工程里的ZXConvenienceCode的interface里的ZXSugarCode.h 
> 就是一些pod工程里有方便的全局宏什么的, 你主工程里也想引用, 需要这么做 ![pod_config_pch][3]￼ 我就只是做了把Precompile改为yes, Prefix Header里面的内容是自己本来就有的, 就不管=.= 最后结果如下, 可以这么引用了: ![result_pod_pch][4]￼ ![main_pch][5]￼ ![main_pch_use][6]￼

 [1]: https://leezix13.com/wordpress/wp-content/uploads/2017/10/create_new_pch.png
 [2]: https://leezix13.com/wordpress/wp-content/uploads/2017/10/config_main_project.png
 [3]: https://leezix13.com/wordpress/wp-content/uploads/2017/10/pod_config_pch.png
 [4]: https://leezix13.com/wordpress/wp-content/uploads/2017/10/result_pod_pch.png
 [5]: https://leezix13.com/wordpress/wp-content/uploads/2017/10/main_pch.png
 [6]: https://leezix13.com/wordpress/wp-content/uploads/2017/10/main_pch_use.png