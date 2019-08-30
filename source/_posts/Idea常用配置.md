---
title: Idea常用配置
date: 2019-08-30 15:29:27
tags: idea
categories: IDE工具
---
### IntelliJ IDEA常用配置
1. View->勾选Tool Bar与Tool Buttons ；设置显示常见的视图,调出工具条和按钮组；
1. File->Settings：打开设置视口；
	1. Appearance & Behavior->Theme ：设置主题；
	1. Keymap->选择eclispe模式 ：设置快捷为 Eclipse；或者 File Import Settings ：导入已有的快捷键设置；
	1. Editor->General->勾选Change font size(Zoom)... ：设置Ctrl + 鼠标滚轮修改字体大小；
	1. Editor->General->勾选Show quick document on mouse... ：设置鼠标悬浮提示；
	1. Editor->General->Auto Imporp->勾选Add unambigous import...和Optimize imports... ：设置自动导包；
	1. Editor->General->Appearance->勾选Show line number和Show method seperators ：设置显示行号和方法间的分隔符;
	1. Editor->General->Code Completion->Case sensitive completion->none ：忽略大小写提示；
	1. Editor->General->Editor Tabs->取消勾选Show tabs in single row ：取消单行显示tabs；
	1. Editor->Font ：设置字体；
	1. Editor->File and Code Templates->Includes->File Header : 修改类头的文档注释信息;
	1. Editor->File Encodings-encoding相关全部改为utf-8，并勾选Transparent native-to-ascii conversion ： 设置项目文件编码；
	1. Editor->Live Templates ；自定义模板；
	1. Build,Execution,Deployment->勾选build project auto..和Compile independent.. ：设置自动编译；
	1. Build,Execution,Deployment->Build Tools->Maven : 配置maven；
	1. Build,Execution,Deployment->Application Server->'+'号，添加配置tomcat；
	1. Version Control->Git ： 配置git；
### 常用插件
- .ignore : git忽略文件；
- CamelCase ：驼峰式命名和下划线命名交替变化；
- CheckStyle-IDEA ： 代码样式检查；
- Statistic : 代码统计；
- Eclipse Code Formatter ： Eclispe风格格式化；
- Maven Helper ： Maven辅助；
- GsonFormat : 把 JSON 字符串直接实例化成类；
- FindBugs-IDEA ： 代码 Bug 检查；
- Lombok ；Lombok支持；
- PlanUML intergration ：UML插件；

### 如何删除模块
- 选中模块->右键Open Module Settings->点击'-'号->将module从project移除->delete;

### 附件
{% asset_link idea.pdf idea配置详解 %}
{% asset_link keymap-shkstart.jar 快捷键定制包 %}
