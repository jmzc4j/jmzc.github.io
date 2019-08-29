---
title: Html初识
date: 2019-08-29 12:57:50
tags: html
categories: 学习笔记
---
### 一 软件的架构
- C/S架构：(client-server)客户端服务器架构：    
  1. 用户需要安装客户端来使用软件；
  2. 每次服务端更新后需要更新客户端；
  3. 针对不同的操作系统，需要开发不同的客户端程序，跨平台性差；
  4. 使用自有协议，相对来说安全性较好；
- B/S架构：(browser-server)浏览器服务器架构:    
  1. 不需要安装客户端，只要有一个浏览器即可；
  2. 软件更新不需要更新客户端；
  3. 由于只需开发服务端，跨平台性较好；
  4. 使用公共的http协议或者https协议，相对C/S架构来说可能安全性稍差；

- 网页组成：根据w3c的标准，一个网页主要由结构、表现和行为组成，即html、css和js。

### 二 HTML
- html：hypertext markup language,超文本标记语言：
  1. 不是一门编程语言，而是一门告诉浏览器如何组织页面的标记语言，决定了网页的结构；
  2. 元素是html的基本单位，通常情况下，一个元素有开始标签、结束标签和内容构成；
  3. HTML标签不区分大小写，从可读性和一致性方面通常使用小写字母；
  4. 元素按照性质可分成块级元素和内联元素两大类别；
  5. 没有内容的元素称之为空元素；
  6. 可以在开始标签中为元素添加属性，属性通常是一组由等号连接的名值对，值用引号包裹；
  7. 当属性没有值或者其值为属性名本身，这样的属性称之为布尔属性；
- 完整的html页面的组成:   
  1. 文档类型声明<!doctype html>，h5声明，最短的有效的文档声明；
  2. html根元素，包裹了一个完整html页面；
  3. head元素，包裹了所有想包含在html页面中但不想在页面中显示的内容，如标题、描述、关键字、字符集等；
  4. body元素，包裹了所有想在网页中显示的内容，如文本、图片、音频、视频等；

### 三 HTML元素
- 元数据：meta
- 标题和段落：h、p
- 超链接和图片：a、img、figure、picture
- 列表：ul、ol、dl
- 实体：`&amp; &nbsp; &lt; &gt; &copy; &quote;`
- 表格：table、tr、td、th、colgroup、col、thead、tfoot、tbody、caption
- 表单：form、input（text、password、file、hidden、checkbox、radio、email、number、tel、search）、textarea、select、fieldset
- 语气：em、strong、ins、del、small
- 引用：blockquote、q、cite
- 缩略语和上下标：abbr、sup、sub
- 代码：code、pre
- 换行和水平分割线：br、hr
- 布局：div、span
- 语义布局：header、nav、main、section、article、aside、footer
- 音频和视频：audio、video

### 四 CSS
- css：cascading style sheets，层叠样式表：
  1. 指定文档如何呈现给用户的语言，用来定义文档的样式和布局，决定了网页的表现；
  2. css的基本单位就是一个个css规则，一个规则由选择器和css声明块组成（与值配对的属性称之为css声明）；
  3. css工作原理： 浏览器加载html并解析，然后加载css并解析，在然后将解析后的html和css在dom树上进行渲染，最后呈现给用户；
  4. 添加样式表的方式：外部样式表、内部样式表和内联样式表；
### 五 css选择器
- 基本选择器：元素选择器、类选择器、ID选择器、通用选择器、分组选择器；
- 关系选择器：子元素选择器（>）、后代选择器、交集选择器、兄弟选择器(+、~)
- 属性选择器：E[att]、E[att="val"]、E[att^="val"]、E[att$="val"]、E[att*="val"]、E[att|="val"]、E[att~="val"]
- 伪类选择器：E:link、E:visited、E:hover、E:active、E:focus、E:not()、E:first-child、、E:nth-child(n)、E:nth-of-type(n)、E:empty、E:checked、E:disabled、E:enabled
- 伪元素选择器：E:after/E::after、E:before/E::before

### 六css中单位
- 长度单位：px、em、百分比
- 颜色单位：rgb、代表颜色的单词、十六进制
### 七 规则优先级
- 在外部添加样式的情况下，id选择器>类选择器、伪类选择器、属性选择器>元素选择器、伪元素选择器

