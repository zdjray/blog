---
layout: post
category: "tools"
title:  "git push不输入密码 Windows/Linux"
tags: [git,工具,效率]
summary: "有时,我们只想节省那么几秒钟,用来多陪陪家人."
---
新开了博客,使用jelly+github搭建.具体方法可以参考[阮一峰的博客](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html).  
按照指引搭一个博客并不复杂,但还是要经过很多道工序.在此期间每次提交的时候都要输入密码,这点非常的不方便.  
于是就想着怎么能够让信任的终端不再输入密码,折腾一番总算找到解决方案.  

现在把具体的步骤记录下来:  
我们这里打算用SSH验证的方式来实现.  
##生成公私钥对
首先我们准备好密钥生成工具ssh-keygen,若是Windows则只要安装了Git Windows客户端,则可以在其安装目录的bin下面找到这个工具.

在命令行输入
>ssh-keygen -t rsa -C 'you@yourdomain.com'
  
连击三个回车,即可生成公私钥对.并且可以在~/.ssh目录下得到id_rsa和id_rsa.pub两个文件.

##添加密钥到github
假定我们已经拥有了[github](https://github.com)账号.若没有,[这里](https://github.com/join)可以注册.  
登录之后,点击github网站右上角的View profile and more(头像所在位置).  
找到左侧边栏Personal settings的SSH keys选项,在右侧面板中点击Add SSH key,看到如下界面:  
![Add SSH key](/image/20160118152913.png)

在title中输入一个名字用于标识即将输入的key  
在key中输入上述生成的id_rsa.pub文件内容(以ssh-rsa开头,以你的邮箱结尾)  
然后点击Add key按钮将key保存  
这时会右侧的SSH keys列表中看到你刚才输入的key条目,包括输入的标识,公钥指纹信息,添加的时间以及最后一次使用的时间  

##客户端配置
打开命令行,配置git用户名和邮箱
>git config --global user.name "你的用户名"  
>git config --global user.email "你的邮箱"

##修改git config
github允许两种方式来clone一个仓库:SSH和HTTPS,默认为HTTPS.  
![clone URL](/image/20160118171349.png)

若仓库以SSH的方式clone到本地,则无需更多修改.
否则将方式切换为SSH,点击右侧的复制链接按钮  
并且打开仓库的.git目录(隐藏目录)中的config文件,找到[remote origin]的位置
>[remote "origin"]  
>url = git@github.com:zdjray/blog.git  
>fetch = +refs/heads/*:refs/remotes/origin/*  

将上述url后面的部分替换为剪切板中的内容即可

##测试SSH连接
再次打开命令行,输入如下命令
>ssh -T git@github.com

得到如下提示即表示大功告成了!  
![ssh test result](/image/20160118172532.png)

##开始使用
此时在命令行使用git push就不用再输入用户名和密码了.  
正常使用时会得到如下提示:  
>Warning: Permanently added the RSA host key for IP address 'xxx.xxx.xxx.xxx' to the list of known hosts.