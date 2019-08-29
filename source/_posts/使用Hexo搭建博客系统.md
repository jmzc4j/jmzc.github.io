---
title: 使用Hexo搭建博客系统
date: 2019-08-28 06:29:42
tags: hexo
categories: 学习笔记
---
### Hexo是什么
- Hexo是Node编写的一个基于markdown引擎的用来快速开发静态博客系统的框架；
- 官方网站：[hexo.io](https://hexo.io/ "hexo.io");

### Hexo怎么用
- 准备环境
1. 安装node.js,[下载地址](https://nodejs.org/en/);
2. 安装git,[下载地址](https://git-scm.com/);
- 安装 hexo-cli 
1. 打开GitBash，执行`$ npm install -g hexo-cli`;
2. 创建一个hexo的工作目录，进入目录执行`hexo init <folder>`;
3. 进入初始化的那个目录，执行`npm install`来构建hexo并安装相应依赖;
4. 配置_config.yml文件;
	```
	# Site
	title: Jmzc's Blog
	subtitle:
	description: Java,Study,Notes,Daily Life,
	keywords:
	author: Jmzc
	language: zh-CN
	timezone:

	# URL
	## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
	url: https://jmzc4j.github.io
	root: /
	permalink: :year/:month/:day/:title/
	permalink_defaults:

	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repo: git@github.com:jmzc4j/jmzc4j.github.io.git
	  branch: master
	```
5. 执行`hexo new <title>` 新建一个blog；
6. 在hexo根目录的source/_posts/找到title.md文件进行博客编辑;
7. 执行`hexo g 或 hexo generate` 生成html静态页面（该页面在pubic文件夹下）
8. 执行`hexo s 或 hexo server` 启动hexo服务在本地进行测试；
9. 执行`hexo d 或 hexo devlop` 将生成的静态页发布到github上;
- 推送源码到github
1. `git init`
2. `ssh-keygen -C 'jmzc-blog'`,然后将家目录中的公钥复制到github上
3. `git remote add hexo-ssh git@github.com:jmzc4j/jmzc4j.github.io.git`
4. `git checkout -b source`
5. `git add .`
6. `git commit -m 'hexo init'`
7. `git push hexo-ssh source` 

### 视频教程
{% xvideos //player.bilibili.com/player.html?aid=55851824&cid=97634618&page=1 %}
