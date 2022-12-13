---
title: pages搭建hexo博客
sticky: 999
categories: 运维
description: 零成本方案！教你怎么利用github pages搭建hexo博客！！！
tags:
  - hexo
  - github
  - github pages
  - 博客
abbrlink: 5434c7eb
date: 2022-12-13 11:04:48
---
### 一、准备工作
#### 1、github账号
&#8195;&#8195;作为IT工作者，大家应该都有github账号。如果没有的话，可到官网（https://github.com/） 去注册一个。

#### 2、安装git
&#8195;&#8195;这个很简单，如果电脑上没有安装，可以参照菜鸟教程（https://www.runoob.com/git/git-install-setup.html）

#### 3、安装nodejs
&#8195;&#8195;因为hexo是基于nodejs编写的，所以需要在电脑安装nodejs，具体可以参照菜鸟教程（https://www.runoob.com/nodejs/nodejs-install-setup.html） 安装


### 二、创建仓库
&#8195;&#8195;登录github后，进入github首页（https://github.com/） ，在如下红框的位置点击进入创建仓库页面
![](1.jpg)

仓库名必须为“username.github.io”，其中username为你在github上的用户名
![](2.jpg)

点击创建之后就会有个一个默认的分支叫main，这个分支用来存放hexo生成之后的html、css文件等。另外，最好再建一个分支用来放博客源码，我们日常写文章都在这个分支写，分支名称可以随便写，比如我这里把分支名称起为hexo


### 三、安装hexo
#### 1、安装博客框架
在任意位置打开命令窗口，输入以下命令
```shell
npm install -g hexo-cli
```

#### 2、创建博客项目
打开命令窗口，定位在你想存放博客项目的位置（不用建目录，hexo命令会自动建），依次输入以下命令
```shell
hexo init hexo-blog

cd hexo-blog

npm install
```
其中，hexo-blog为博客项目的名字，你可以换成你想要的任意英文名。
输入完以上命令之后，一个博客项目就建好了，如果想本地启动，只需要依次输入以下命令：
```shell
# 生成博客静态文件
hexo g
# 预览
hexo server
```
启动完成之后，在浏览器输入http://localhost:4000，默认页面如下：
![](3.jpg)

### 四、关联github
&#8195;&#8195;博客项目建好之后，我们还只能在本地看，要想通过github pages访问，则必须和github仓库关联。

&#8195;&#8195;在如下位置找到第二步创建的仓库的克隆地址
![](4.jpg)

在空白位置（非博客项目文件夹内）打开git命令窗口，输入如下命令克隆
```shell
git clone https://github.com/username/username.github.io.git
```
其中https://github.com/username/username.github.io.git是上面复制的地址。

接着，我们需要输入“git checkout hexo”命令切换到hexo分支，hexo分支就是第二步说的存放博客源码的分支。

完成这些操作之后，进入username.github.io.git目录找到.git文件夹，把它移动到博客项目的根目录，这个时候博客项目就跟github关联起来了。


### 五、发布到github
&#8195;&#8195;第四步只是将博客源码关联到github，但没有把生成的博客静态文件发布到github的main分支上。要实现这一步，需要借助一个nodejs插件来实现。在博客项目根目录打开命令窗口，输入以下命令安装插件：
```shell
npm install hexo-deployer-git --save
```
然后修改根目录下的_config.yml，配置github相关信息
```shell
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: main
```
配置好之后，如果需要发布到github，只需要依次输入以下命令：
```shell
hexo clean
# 生成博客静态文件
hexo g
# 发布到github
hexo d
```
发布成功之后，在浏览器输入https://username.github.io/ 就能看到你的博客啦！