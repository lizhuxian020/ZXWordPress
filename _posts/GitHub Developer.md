---
post_title: GitHub Developer
layout: post
published: true
---

# GitHub Developer

Article: [Getting Start| GitHub Developer](https://developer.github.com/v3/guides/getting-started/)

读上文章, 后感:

#### 生成OAuth Token:
```
curl -i -u your_username -d '{"scopes": ["repo", "user"], "note": "getting-started"}' \
   https://api.github.com/authorizations
```

然后Token字段里, 就是OAuth Token
![get_OAuth_Token](media/15108003271337/get_OAuth_Token.jpg)

--
#### 查询你的GitHub资料: 
`curl -i -u your_username https://api.github.com/user`
`curl -i -H 'Authorization: token 5199831f4dd3b79e7c5b7e0ebe75d67aa66e79d4' https://api.github.com/user`

以上两句都是可以查询, 只是第一句需要输入密码, 第二句不用. 所以OAuth Token可以当你的账号密码这么使用. 要谨慎不要外漏

`curl https://api.github.com/users/your_username`
也行, 查看你的或者别人的GitHub资料, 可以加个`-i`, 会多打印Header

--
#### 查看自己每一个仓库资料
`curl -i -H 'Authorization: token 5199831f4dd3b79e7c5b7e0ebe75d67aa66e79d4' https://api.github.com/user/repos`

也可以查看别人每一个仓库的资料
`curl -i https://api.github.com/users/technoweenie/repos`

--
#### 创建仓库
`curl -i -H 'Authorization: token 5199831f4dd3b79e7c5b7e0ebe75d67aa66e79d4' -d '{"name": "blog","auto_init": true,"private": true, "gitignore_template": "nanoc"}' https://api.github.com/user/repos`

这是一个创建private的仓库, 可能会提示你upgrade, 就是去交钱. 
把上面的true, 改为false了, 就好了.

如果你建了个private仓库, 然后去curl -i https://api.github.com/repos/username/private
然后就报404, 因为是private, 别人看不到你仓库资料.


