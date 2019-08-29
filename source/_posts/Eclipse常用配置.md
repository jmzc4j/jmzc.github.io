---
title: Eclipse常用配置
date: 2019-08-29 00:48:08
tags: eclipse
categories: IDE工具
---

### Eclispe常用配置
1. Window->Preferences->General->勾选"show heap status";
2. Window->Preferences->General->Appearance->取消勾选"show most recently used tabs";
3. Window->Preferences->General->Appearance->Colors and Fonts->Basic->Text Font->Edit->调整字体大小;
4. Window->Preferences->General->Content Types->Java Properties File->Default Encoding->修改为UTF-8->Update;
5. Window->Preferences->General->Editors->Text Editors->Background color->背景色调整为RGB(199,237,204);
6. Window->Preferences->General->Editors->Text Editors->Spelling->取消勾选"enable spell checking";
7. Window->Preferences->General->Project Natures->取消勾选"Automatically detect missing ...";
8. Window->Preferences->General->Sartup and Shutdown->仅保留git UI;
9. Window->Preferences->General->Workspace->Text file encoding->修改为UTF-8;

1. Window->Preferences->Install/Update->Automatic Updates->取消勾选"Automatically find new ...";
2. Window->Preferences->Java->Code Style->Code Templates->Comments->Types->修改为`@author ${user}  <mailto:jmzc_top@aliyun.com>`;
3. Window->Preferences->Java->Code Style->Code Templates->Code->去掉注释部分;
4. Window->Preferences->Java->Code Style->Formatter->创建一个新的myself模板->然后去掉Comments选择卡中的JavaDoc和block comment格式化;
5. Window->Preferences->Java->Editors->Content Assist->Auto activation triggers for java->修改为`abcdefghigklmnopqrstuvwxyzABCDEFGHIGKLMNOPQRSTUVWXYZ.`;
6. Window->Preferences->Java->Editors->Content Assist->Advanced->取消勾选"Java Proposals(Code Recommenders)";
7. Window->Preferences->Java->Editors->Templates->可以修改预定义模板或者新建模板;
8. Window->Preferences->Java->Installed JREs->修改JRE为安装的JDK;

1. Window->Preferences->JavaScript->Code Style->Formatter->创建一个新的myself模板->然后去掉Comments选择卡中的JavaDoc和block comment格式化;
2. Window->Preferences->JavaScript->Editors->Content Assist->Auto activation triggers for javascript->修改为`abcdefghigklmnopqrstuvwxyzABCDEFGHIGKLMNOPQRSTUVWXYZ.`;

1. Window->Preferences->Maven->Installings->添加外部的maven安装目录->进行勾选;
2. Window->Preferences->Maven->User Settings->User Settings选择外部settings文件路径;

1. Window->Preferences->Server->Runtime Environment->Add->添加外部tomcat;
2. Window->Preferences->Web->JSP Files->修改Encoding为UTF-8;

### sts插件(老版本地址推演)
1. 在[sts插件官网](https://spring.io/tools3/sts/all)找到eclipse4.9.0的下载地址,如下：
```
分解：
https://download.springsource.com/release/TOOLS/update (保留1)
/3.9.9.RELEASE/e4.9
/springsource-tool-suite (保留4)
-3.9.9.RELEASE-e4.9.0
-updatesite.zip (保留6)
```
2. 在[STS官网](https://spring.io/tools3/sts/legacy)找到对应的Eclipse的版本的下载地址,如下
```
分解：
https://download.springsource.com/release/STS
/3.9.6.RELEASE (保留2)
/dist
/e4.9 (保留3)
/spring-tool-suite
-3.9.6.RELEASE-e4.9.0 (保留5)
-win32-x86_64.zip
```
3. 推算sts插件其他版本的地址，只需知道STS的下载地址，然后安装保留序号进行拼接即可；如需要下载eclispe4.7.3a的sts插件：
- STS下载地址：https://download.springsource.com/release/STS/3.9.4.RELEASE/dist/e4.7/spring-tool-suite-3.9.4.RELEASE-e4.7.3a-win32-x86_64.zip
- 按照上面的推导规则，得到sts插件的离线下载包地址为：
https://download.springsource.com/release/TOOLS/update/3.9.4.RELEASE/e4.7/springsource-tool-suite-3.9.4.RELEASE-e4.7.3a-updatesite.zip
4. 下载安装
- 打开eclispe->Help->Install New Software->Add->Achieve->选择下载的压缩包->OK->选择带有IDE的4个进行安装（取消勾选下面的站点升级）

### 安装lombok
- [官网下载](https://www.projectlombok.org/)lombok.jar或者通过maven下载；
- 双击jar，在出现的界面选择eclispe根目录中的eclispe.exe，再点击install/update即可;

### uml插件
- planUML安装
	1. 官网地址：http://plantuml.com/zh;
	2. Help->Install New Software->Add->添加地址(http://hallvard.github.io/plantuml )->全部安装即可;
	3. [下载Graphviz](https://www.graphviz.org/download/)并安装;
	4. Window->Preferences->PlantUML->配置dot.exe的路径;
	5. Window->Show View->PlanUML;
- AmaterasUML安装
	1. Help->Install New Software->Add->添加地址(https://takezoe.github.io/amateras-update-site/)->全部安装即可;

### eclipse手动安装插件
1. 在eclipse根目录下建文件夹MyPlugins;
2. 下载插件并解压得到包含features和plugins的文件夹XXX;
3. 完成复制和建立文件夹的操作之后，如${eclipse_Home}\MyPlugins\XXX\eclipse\features和..\plugins文件夹;
4. 在eclipse_Home下建links文件夹,并建立一个p1_xxx.link文件,内容为`path=${eclipse_Home}/MyPlugins/xxx`
	- 注意:
		- 路径中是"/"或者"\\"  而不是"\" ;
		- ${eclipse_Home}换成实际绝对地址：如D:\\eclipse\\MyPlugins\\quantum303;
		- link目录下的文件eclipse都要读入,小心！;
		- path下面应该有eclipse文件夹，而不是将path设置成eclipse文件夹;
		- 让path下面有plugins和features这两个目录;
5.  删除${eclipse_Home}\configuration中的org.eclipse.update目录;
6.  重启eclipse,ok.  其实，myeclipse的插件安装就是如此操作的;

### eclispe注释模板
```
文件(Files)注释标签：
/**  
* @Title: ${file_name}
* @Package ${package_name}
* @Description: ${todo}(用一句话描述该文件做什么)
* @author A18ccms A18ccms_gmail_com  
* @date ${date} ${time}
* @version V1.0  
*/

类型(Types)注释标签（类的注释）：
/**
* @ClassName: ${type_name}
* @Description: ${todo}(这里用一句话描述这个类的作用)
* @author A18ccms a18ccms_gmail_com
* @date ${date} ${time}
*
* ${tags}
*/

字段(Fields)注释标签：
/**
* @Fields ${field} : ${todo}(用一句话描述这个变量表示什么)
*/

构造函数标签：
/**
* <p>Title: </p>
* <p>Description: </p>
* ${tags}
*/

方法(Constructor & Methods)标签：
/**
* @Title: ${enclosing_method}
* @Description: ${todo}(这里用一句话描述这个方法的作用)
* @param ${tags}    设定文件
* @return ${return_type}    返回类型
* @throws
*/

覆盖方法(Overriding Methods)标签：
/* (非 Javadoc)
* <p>Title: ${enclosing_method}</p>
* <p>Description: </p>
* ${tags}
* ${see_to_overridden}
*/

代表方法(Delegate Methods)标签：
/**
* ${tags}
* ${see_to_target}
*/

getter方法标签：
/**
* @return ${bare_field_name}
*/

setter方法标签：
/**
* @param ${param} 要设置的 ${bare_field_name}
*/
```


