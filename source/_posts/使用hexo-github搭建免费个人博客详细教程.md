---
title: 使用hexo+github搭建免费个人博客详细教程
date: 2018-09-29 16:01:44
categories:
tags: 
- hexo 
- github
---

转载至 [小茗同学的博客园](https://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html)

# 1. 前言
## 1.1 使用github pages服务搭建博客的好处：
```
    全是静态文件，访问速度快；
    免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
    可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的；
    数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；
    博客内容可以轻松打包、转移、发布到其它平台；
    等等；
```

## 1.2. 准备工作
    在开始一切之前，你必须已经
```
    有一个github账号，没有的话去注册一个；
    安装了node.js、npm，并了解相关基础知识；
    安装了git for windows（或者其它git客户端）
```

# 2. 搭建github博客
## 2.1 创建仓库

            新建一个名为你的用户名.github.io的仓库，比如说，如果你的github用户名是test，
        那么你就新建test.github.io的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 http://test.github.io 了，是不是很方便？
        由此可见，每一个github账户最多只能创建一个这样可以直接使用域名访问的仓库。 
        几个注意的地方：
```
    注册的邮箱一定要验证，否则不会成功；
    仓库名字必须是：username.github.io，其中username是你的用户名；
    仓库创建成功不会立即生效，需要过一段时间，大概10-30分钟，或者更久，我的等了半个小时才生效；
```
## 2.2. 绑定域名(可跳过此步骤)

当然，你不绑定域名肯定也是可以的，就用默认的 xxx.github.io 来访问，如果你想更个性一点，想拥有一个属于自己的域名，那也是OK的。
首先你要注册一个域名，域名注册以前总是推荐去godaddy，现在觉得其实国内的阿里云也挺不错的，价格也不贵，毕竟是大公司，放心！
绑定域名分2种情况：带www和不带www的。
域名配置最常见有2种方式，CNAME和A记录，CNAME填写域名，A记录填写IP，由于不带www方式只能采用A记录，
所以必须先ping一下你的用户名.github.io的IP，然后到你的域名DNS设置页，将A记录指向你ping出来的IP，
将CNAME指向你的用户名.github.io，这样可以保证无论是否添加www都可以访问，如下：
<div align=left><img width="600" height="auto" src="http://image.liuxianan.com/201608/20160823_191336_238_8683.png"/></div>
然后到你的github项目根目录新建一个名为CNAME的文件（无后缀），里面填写你的域名，加不加www看你自己喜好，因为经测试：

如果你填写的是没有www的，比如 mygit.me，那么无论是访问 http://www.mygit.me 还是 http://mygit.me ，
都会自动跳转到 http://mygit.me

如果你填写的是带www的，比如 www.mygit.me ，那么无论是访问 http://www.mygit.me 还是 http://mygit.me ，
都会自动跳转到 http://www.mygit.me

如果你填写的是其它子域名，比如 abc.mygit.me，那么访问 http://abc.mygit.me 没问题，但是访问 http://mygit.me ，
不会自动跳转到 http://abc.mygit.me

另外说一句，在你绑定了新域名之后，原来的你的用户名.github.io并没有失效，而是会自动跳转到你的新域名

# 3.配置SSH key
为什么要配置这个呢？因为你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，
所以我们使用ssh key来解决本地和服务器的连接问题。
```
$ cd ~/. ssh #检查本机已存在的ssh密钥
```
如果提示：No such file or directory 说明你是第一次使用git。无提示则跳过 ssh-keygen -t rsa -C "github邮件地址" 这步
```
ssh-keygen -t rsa -C "github邮件地址"
```
然后连续3次回车，最终会生成一个文件在用户目录下，打开用户目录，找到.ssh\id_rsa.pub文件，
记事本打开并复制里面的内容，打开你的github主页，进入个人设置 -> SSH and GPG keys -> New SSH key：
<div align=left><img width="600" height="auto" src="//image.liuxianan.com/201608/20160818_143914_495_9084.png"/></div>
将刚复制的内容粘贴到key那里，title随便填，保存。
## 3.1. 测试是否成功
```
    $ ssh -T git@github.com # 注意邮箱地址不用改
```
    如果提示Are you sure you want to continue connecting (yes/no)?，输入yes，然后会看到：
```
    Hi liuxianan! You've successfully authenticated, but GitHub does not provide shell access.
```
看到这个信息说明SSH已配置成功！
此时你还需要配置：
```
    $ git config --global user.name "liuxianan"// 你的github用户名，非昵称
    $ git config --global user.email  "xxx@qq.com"// 填写你的github注册邮箱
```
# 4.使用hexo写博客
