# 不拔卡看Tik Tok

[youtu教程](https://www.youtube.com/watch?v=W55UO8eizgA)

shadowRocket代码
```
[URL Rewrite]
(?<=_region=)CN(?=&) JP 307
(?<=&mcc_mnc=)4 2 307
^(https?:\/\/(tnc|dm)[\w-]+\.\w+\.com\/.+)(\?)(.+) $1$3 302
(^https?:\/\/*\.\w{4}okv.com\/.+&.+)(\d{2}\.3\.\d)(.+) $118.0$3 302

[MITM]
hostname = *.tiktokv.com,*.byteoversea.com,*.tik-tokapi.com

```

拿到Tik Tok先进行重签名,(具体看RSA篇)

装好app之后, 先别打开, 去配置shadowRocket
1. 点击"配置"tab
2. 点击default.conf, 选择编辑纯文本
3. 在最底部添加上述代码(那个URL Rewrite那个代码), 在最底部添加, 原有已经有一个URLRewrite的配置了, 是谷歌的, 不影响)
4. 加完之后去按右上角保存
5. 然后点击感叹号去HTTPS解密, 打开最顶部的HTTPS解密按钮
6. 然后安装证书, 然后去系统设置安装配置文件, 去关于手机信任证书(跟装Charles证书一样)
7. 然后回到ShadowRocket, 点击完成, 启动HTTPS解密, 然后重启VPN即可
8. 最后打开Tik Tok就好了

最后无法完美实现Tik Tok, 猜测是因为服务器只认某个指定证书问题.(但如果是这样, 老app都应该看不了才对)
最后只能去使用网页版, 但网页版阉割了很多功能=.=

## 使用另外的app, Tik Tok++
[下载地址](https://ihax.io/tiktok-plus-plus-ipa/)

然后重签名安装即可
