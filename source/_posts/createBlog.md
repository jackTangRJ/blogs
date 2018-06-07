---
title: github新建博客并绑定阿里云域名
date: 2018-05-28 08:53:12
---


![image](http://www.wailian.work/images/2018/06/07/day-1.md.png)

> #### 前言

其实平时自己写的文章并不多，偶尔看到一些东西会做点笔记，但是每次写的东西都会到处放，不好找，所以才想着自己搭建一个人博客网站。

具体步骤如下：安装NodeJs->安装hexo->生成SSH并添加到github->部署项目->上传到github->绑定个人域名

<!-- more -->

###  1.到[Node.js官方网址](https://nodejs.org/en/download/)下载对应版本 


```
	输入 node -v 出现版本号即为成功
```


### 2.安装Git


1. 到官方网址下载对应的版本 
    Git各平台下载地址：https://git-scm.com/download 
    Windows平台Git下载地址：https://git-scm.com/download/win 
2. 检查Git版本
    

```
输入 git --version
```

### 3.GitHubPages配置

1. 进入GitHub官网https://github.com/注册账号
2. 新建项目
3. 在建好的项目右侧有Settings 
4. 向下拉可看到GitHub Pages 
    

![image](http://www.wailian.work/images/2018/05/28/555.md.png)



点击对应的网址你会发现该项目已经被部署到网络上，你可以通过外网来访问它。

### 4.安装Hexo

在自己认为合适的地方创建一个文件夹，用来存放之后博客的文档以及配置文件，我是在C盘新建了文件夹，并命名为Blogs。 
然后点击进入创建的文件夹，点击鼠标右键选择Git Bash Here


```
输入 npm install hexo -g 开始安装Hexo
```
输入 <code>npm install hexo -g </code>开始安装Hexo

```
输入 hexo -v 检查是否安装成功
```

![image](http://www.wailian.work/images/2018/05/28/_20180528171548.md.png)


初始化该文件夹

```
输入 hexo init 
```
这个过程也有点漫长需要等待几分钟，最后出现Start blogging with Hexo！是不是很激动！！


开始安装所需要的组件(也可通过淘宝源 cnpm install安装)
```
    输入 npm install
```


最后两步你就可以看到博客雏形了


```
输入 hexo g
```


```
输入 hexo s
```

![image](http://www.wailian.work/images/2018/05/28/666.md.png)

**骚年，打开你的浏览器 输入 http://localhost:4000/ 开启博客之旅**


![image](http://www.wailian.work/images/2018/05/28/1108615-20171022001738037-1195721153.md.png)



### 5.Hexo连接GithubPages

将Hexo与GithubPages联系起来，首次运行的话这里需要设置Git的user name和email 
ctrl+C结束之前的sever


```
输入 git config --global user.name "tangrenjie" 

改成你自己的即可
```

```
输入 git config --g global user.email "406067361@qq.com" 

改成你自己的即可
```

##### 1.首先需要检查你电脑是否已经有 SSH key 

运行 git Bash 客户端，输入如下代码：


```
$ cd ~/.ssh
$ ls
```

![image](http://www.wailian.work/images/2018/05/28/999.md.png)


这两个命令就是检查是否已经存在 id_rsa.pub 或 id_dsa.pub 文件，如果文件已经存在，那么你可以跳过步骤2，直接进入步骤3。


#####  2、创建一个 SSH key


```
$ ssh-keygen -t rsa -C  "your_email@example.com"
```

添加密钥到ssh-agent


```
输入 eval "$(ssh-agent -s)"
```

添加生成的SSH key到ssh-agent


```
输入 ssh-add ~/.ssh/id_rsa
```


代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。


##### 3、添加你的 SSH key 到 github上面去


a、首先你需要拷贝 id_rsa.pub 文件的内容，你可以用编辑器打开文件复制，也可以用git命令复制该文件的内容，如：


```
$ clip < ~/.ssh/id_rsa.pub
```

b、登录你的github账号，从又上角的设置（ Account Settings ）进入，然后点击菜单栏的 SSH key 进入页面添加 SSH key。




![image](http://www.wailian.work/images/2018/05/28/444.md.png)

![image](http://www.wailian.work/images/2018/05/28/5555.md.png)

### 6.配置Deploy

在你的博客所在文件夹根目录，例如我的是在C:\Blogs


找到_config.yml文件，点击编辑，我是用的nodepad++打开的 

![image](http://www.wailian.work/images/2018/05/28/ppp.md.png)

==这里需要注意的是格式一定是：后跟一个空格，名称对应自己的GitHub项目名称==

到这里基本上博客已经搭建成功

### 7.发布文章

安装扩展 


```
输入 npm install hexo-deployer-git --save
```


```
输入 hexo new "Hexo教程" 
```
此时在我的 C:\Blogs\source\_posts 下会出现 Hexo教程.md文件


```
输入 hexo g 编译，每次改动上传都要执行此操作
```

```
输入 hexo d 上传，等待一段时间就可以到github上去看已经提交了
```
### 8.安装主题

	Concise，一款为hexo设计的简约而漂亮的主题


###### 克隆主题
```
    $ git clone https://github.com/huangjunhui/concise.git themes/concise 
```
###### 启用

	修改你的博客根目录下的config.yml配置文件中的theme属性，将其修改为concise
    
######  更新

	$ cd themes/concise
	 $ git pull
	 
更新前，请先备份你的themes/concise/_config.yml文件。

更多请浏览 [concise官网](https://github.com/huangjunhui/concise/blob/master/README_ZH.md)

### 9.绑定阿里云域名

1.ping 一下你的github地址

我的github为  https://jacktangrj.github.io

![image](http://www.wailian.work/images/2018/05/29/ip.png)

2.打开阿里云
![image](http://www.wailian.work/images/2018/05/29/j1.png)

3.更改hexo

打开本地文件地址，在source文件夹下添加CHANGE文件（==注意 没有后缀名==），里面的内容为你的域名

![image](http://www.wailian.work/images/2018/05/29/3333.png)


```
输入 hexo g
    hexo d 
    提交至github
```

4.在github上修改

![image](http://www.wailian.work/images/2018/05/29/888.png)

5.稍等一会，即可看到效果


# 结束


