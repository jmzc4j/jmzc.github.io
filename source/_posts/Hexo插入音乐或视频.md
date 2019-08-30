---
title: Hexo插入音乐或视频
date: 2019-08-29 18:50:33
tags: hexo
categories: 学习笔记
---
### 使用hexo-tag-aplayer插件
- 官网地址：[hexo-tag-aplayer](https://github.com/grzhan/hexo-tag-aplayer);
- 安装命令：`npm install --save hexo-tag-aplayer`;
- 语法格式：
	```
	{% aplayer title author url [picture_url, narrow, autoplay, width:xxx, lrc:xxx] %}
	```
- 语法参数说明：
	```
	title : 曲目标题;
	author: 曲目作者
	url: 音乐文件 URL 地址;
	picture_url: (可选) 音乐对应的图片地址;
	narrow: （可选）播放器袖珍风格;
	autoplay: (可选) 自动播放，移动端浏览器暂时不支持此功能;
	width:xxx: (可选) 播放器宽度 (默认: 100%);
	lrc:xxx: （可选）歌词文件 URL 地址;
	```
- 当开启Hexo的文章资源文件夹功能时可以将图片、音乐文件、歌词文件放入与文章对应的资源文件夹中，然后直接引用;
	1. hexo根路径配置文件中开启配置，`post_asset_folder: true`;
	2. 正确的引用图片方式是使用下列的标签插件而不是 markdown ：
		```
		{% asset_path slug %}
		{% asset_img slug [title] %}
		{% asset_link slug [title] %}
		
		例如：要引入一个图片:
		{% asset_img example.jpg This is an example image %}
		```
	3. 开启资源目录后，只需在歌词和路径属性中填写文件名即可，例如：
		```
		{% aplayer "Caffeine" "Jeff Williams" "caffeine.mp3" "picture.jpg" "lrc:caffeine.txt" %}
		```
#### 歌词标签
- 除了使用标签 lrc 选项来设定歌词，你也可以直接使用 aplayerlrc 标签来直接插入歌词文本在博客中;
	```
	{% aplayerlrc "title" "author" "url" "autoplay" %}
	[00:00.00]lrc here
	{% endaplayerlrc %}
	```
#### 播放列表
```
{% aplayerlist %}
{
	"narrow": false,      // （可选）播放器袖珍风格
	"autoplay": true,     // （可选) 自动播放，移动端浏览器暂时不支持此功能
	"mode": "random",     // （可选）曲目循环类型，有 'random', 'single', 'circulation', 'order' (列表)， 默认：'circulation' 
	"showlrc": 3,         // （可选）歌词显示配置项，可选项有：1,2,3
	"mutex": true,        // （可选）该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停
	"theme": "#e6d0b2",	  // （可选）播放器风格色彩设置，默认：#b7daff
	"preload": "metadata",      // （可选）音乐文件预载入模式，可选项： 'none' 'metadata' 'auto', 默认: 'auto'
	"listmaxheight": "513px",   // (可选) 该播放列表的最大长度
	"music": [
		{
			"title": "CoCo",
			"author": "Jeff Williams",
			"url": "caffeine.mp3",
			"pic": "caffeine.jpeg",
			"lrc": "caffeine.txt"
		},
		{
			"title": "アイロニ",
			"author": "鹿乃",
			"url": "irony.mp3",
			"pic": "irony.jpg"
		}
	]
}
{% endaplayerlist %}
```
#### MeingJS 支持 (3.0 新功能)
- MetingJS 是基于Meting API的APlayer衍生播放器，引入 MetingJS 后，播放器将支持对于 QQ音乐、网易云音乐、虾米、酷狗、
百度等平台的音乐播放;
- 如果想在本插件中使用 MetingJS，请在 Hexo 配置文件 _config.yml 中设置：
	```
	aplayer:
	  meting: true
	```
- 接着就可以通过 {% meting ...%} 在文章中使用 MetingJS 播放器了(开启后似乎不能使用第一种方式):
	```
	<!-- 简单示例 (id, server, type)  -->
	{% meting "60198" "netease" "playlist" %}

	<!-- 进阶示例 -->
	{% meting "60198" "netease" "playlist" "autoplay" "mutex:false" "listmaxheight:340px" "preload:none" "theme:#ad7a86"%}
	```
- 有关 {% meting %} 的选项列表如下:
	```
	id	必须值	歌曲 id / 播放列表 id / 相册 id / 搜索关键字
	server	必须值	音乐平台: netease, tencent, kugou, xiami, baidu
	type	必须值	song, playlist, album, search, artist
	fixed	false	开启固定模式
	mini	false	开启迷你模式
	loop	all	列表循环模式：all, one,none
	order	list	列表播放模式： list, random
	volume	0.7	播放器音量
	lrctype	0	歌词格式类型
	listfolded	false	指定音乐播放列表是否折叠
	storagename	metingjs	LocalStorage 中存储播放器设定的键名
	autoplay	true	自动播放，移动端浏览器暂时不支持此功能
	mutex	true	该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停
	listmaxheight	340px	播放列表的最大长度
	preload	auto	音乐文件预载入模式，可选项： none, metadata, auto
	theme	#ad7a86	播放器风格色彩设置
	```
### hexo-tag-dplayer插件(好像不太好用)
- 官网地址：[hexo-tag-dplayer](https://github.com/MoePlayer/hexo-tag-dplayer);
- 安装命令：`npm install hexo-tag-dplayer --save`;
- 语法格式：
```
{% dplayer key=value ... %}
```
- 语法参数说明：
	```
	常用选项：url、loop、volume、autoplay、hotkey、logo、mutex、highlight、preload、theme;
	
	autoplay	false	视频自动播放;
	theme	'#b7daff'	主题色;
	loop	false	视频循环播放;
	hotkey	true	开启热键，支持快进、快退、音量控制、播放暂停;
	preload	'auto'	视频预加载，可选值: 'none', 'metadata', 'auto';
	volume	0.7	默认音量，请注意播放器会记忆用户设置，用户手动设置音量后默认音量即失效;
	logo	-	在左上角展示一个 logo，你可以通过 CSS 调整它的大小和位置;
	highlight	[]	自定义进度条提示点;
	mutex	true	互斥，阻止多个播放器同时播放，当前播放器播放时暂停其他播放器;
	
	url	-	视频链接;
	pic	-	视频封面;
	thumbnails	-	视频缩略图;
	
	type	'webvtt'	字幕类型，可选值: 'webvtt', 'ass'，目前只支持 webvtt;
	fontSize	'20px'	字幕字号;
	bottom	'40px'	字幕距离播放器底部的距离，取值形如: '10px' '10%';
	color	'#fff'	字幕颜色;
	
	id	required	弹幕池id，必须唯一;
	api	required	见[弹幕接口](http://dplayer.js.org/zh/guide.html#%E5%BC%B9%E5%B9%95%E6%8E%A5%E5%8F%A3);
	addition	-	额外外挂弹幕,见[bilibili 弹幕](http://dplayer.js.org/zh/guide.html#bilibili-%E5%BC%B9%E5%B9%95);
	token	-	弹幕后端验证 token;
	maximum	-	弹幕最大数量;
	unlimited	false	海量弹幕模式;
	user	'DIYgod'	弹幕用户名;
	
	其他参数请参考[dplayer](https://github.com/MoePlayer/DPlayer);
	```
###  举例
```
{% dplayer "url=https://moeplayer.b0.upaiyun.com/dplayer/hikarunara.mp4" "addition=https://dplayer.daoapp.io/bilibili?aid=4157142" "api=https://api.prprpr.me/dplayer/" "pic=https://moeplayer.b0.upaiyun.com/dplayer/hikarunara.jpg" "id=9E2E3368B56CDBB4" "loop=yes" "theme=#FADFA3" "autoplay=false" "token=tokendemo" %}
```
{% meting "27789126" "netease" "song" %}
{% dplayer "url=http://p0e6ktyto.bkt.clouddn.com/MSN%20%E6%9C%80%E5%90%8E%E7%9A%84%E4%B8%89%E9%87%8D%E5%A5%8F_%20%E7%BB%9D%E5%94%B12017_%E9%AB%98%E6%B8%85.mp4"  "pic=https://i.imgur.com/n3YAGhq.jpg" "loop=yes" "theme=#FADFA3" "autoplay=false" "token=tokendemo" %}