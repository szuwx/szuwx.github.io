---
title: how to create blog with hexo
date: 2018-09-20 17:22:55
tags: technology
---
# 利用Github和Hexo搭建个人网站教程

## 概述
**hexo**是一款基于nodejs的静态博客框架，也就是编写的文章可以编译成html文件 ，不需要额外的管理后台，结合github或者coding page即可免费部署上线 ，发布的内容也相对自由一些。语法主要使用markdown，可以方便与[简书](https://jianshu.com)等BPS同步内容。
可访问[hexo官网地址](https://hexo.io/zh-cn/docs/)了解更多。<!--more-->
## 搭建步骤
### 购买个人域名(可选)
github提供github page的域名 ，不需要备案 ，所以可以直接使用
### 创建个人仓库
注册和登陆 [github.com](https://github.com/)，
点击New repository创建一个博客仓库 ， 假如
用户名是**xiaoming** ， 则新建的仓库名是**xiaoming.github.io**，权限设置为Public ， 其它的不勾选或者选为none
### 下载安装软件
- [git](https://git-scm.com/downloads)---用git -v检查是否安装成功
- [Node.js](https://nodejs.org/en/download/)---hexo基础环境，用npm -v命令检查是否安装成功
**假如使用windows系统，建议下载安装集成软件[laragon](https://laragon.org/download/index.html) 就好，其中已经整个了上面两个软件**

安装完成之后 ， 把本地公钥上传到github个人主页Settings里边的[SSH and GPG keys](https://github.com/settings/keys)的SSH keys栏目下面,这一步是使发布文章时候 ，github可以验证你身份而不用每次输入密码
假如没有公钥则通过下面命令来生成:
```
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```
window默认公钥生成位置是C:\Users\电脑用户名\.ssh\id_rsa.pub , 用记事本打开即可复制。
### 安装hexo
```
npm install -g hexo-cli 
```
安装完成hexo框架之后 ， 输入:
```
hexo init blog
```
安装完成之后 ，
可以通过以下4条命令进入新建博客文章
```
cd blog

hexo new test_my_site

hexo g

hexo s
```
 完成之后 ，用浏览器打开地址 http://localhost:4000 就可看到博客
 继续在_config.yml添加上配置
 - language: zh-CN #根据themes\landscape\languages里边来配置语言，一般是zh-CN
 - post_asset_folder: true#设置为true方便使用本地图片 
 - theme: landscape #指定使用的主题 ，默认是landscape 
 - deploy：#还有两个自配置项 一般是使用git配置，参考[文档](https://hexo.io/zh-cn/docs/deployment.html) ，以实现关联到github
#### 注意
常遇到的问题
- `1,配置文件配置值前面需要加一个空格,否则编译不生效而且报错，`
- `2,一开始未设置_config.yml里边language的值的时候 ，生成的文章md文件需要用编辑器convert to utf-8再重新生成才行,`
- `3,_post目录必须要有md文件,`
- `4,要设置githubpage时，创建repo之后 ，还要设置一下Theme Chooser才生效 ，否则404 ，`
- `5,Github page 只能使用master or gh-pages分支 ， 而且需要在项目settings里边GitHub Pages设置！！`
### 发布文章
使用hexo new ‘文章名’ 来创建文章之后 ， 再使用markdown编辑器直接编辑对应的source\_posts里边的文件保存，
本地预览之后可以推送部署上线 ，
删除文章可以直接删除_post里边对应的md文件 ，重新编译推送即可
- 常用hexo命令
现在来介绍常用的Hexo 命令
npm install hexo -g #安装Hexo
npm update hexo -g #升级 
hexo init #初始化博客
命令简写
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署

# 更换主题，调整样式
按照自己需求调整样式，发布文章，推送即可访问

# 一些注意事项和待优化之处
1, hexo文章更新到github之后，md源码也需要另行保存，一般是保存在博客repo的save分支，一般迁移的话，保存下面几个文件就可以了
_config.yml
theme/
source/
scaffold/
package.json
.gitignore


2, 目前有在简书和github page同时发布文章,发布到简书的话需要单独上传图片文件，不能直接复制文本内容，图片统一用CDN可能会好点
