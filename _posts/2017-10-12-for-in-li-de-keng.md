---
ID: 98
post_title: For in里的坑
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=98
published: true
post_date: 2017-10-12 03:26:08
---
突然今天给我报了这错: `<__NSArrayM: 0x60000005de20> was mutated while being enumerated.` 表面原因是: 在遍历的时候修改了数组值 深层原因是([来自网上][1]):里面的iterator(迭代器)发生变化了 for in是使用 里面的iterator(迭代器) 来进行迭代的, , 数组值修改了之后, iterator也会发生变化, 所以会报错=.= 用 <pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">[mtArr enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {}]
</code></pre> 就好啦 或者用 for ( ; ; )

 [1]: http://www.jianshu.com/p/ad80d9443a92 "来自网上"