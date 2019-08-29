---
title: Linux概述
date: 2019-08-29 14:32:00
tags: linux
categories: 学习笔记
---
### 一、科普知识
##### 操作系统(operating system)
  > 操作系统就是一个可以直接操作硬件的特殊的软件，是具有如下作用的软件。
  - 管理和操作硬件设备。
  - 将对硬件的操作封装成一个又一个的系统调用供其他应用程序使用。

  > 针对应用领域的不同可以将操作系统分成桌面、服务器、嵌入式和移动设备四大类。
  - 桌面操作系统（desktop OS）：Windows、macOS、Linux。
  - 服务器操作系统（server OS）：Linux、Windows Server。
  - 嵌入式操作系统（embedded OS）：Linux。
  - 移动设备操作系统（mobile OS）：IOS、Android。

  > 针对用户不同又分为单用户和多用户操作系统。
  - 单用户操作系统：一台计算机同一时间只能由一个用户使用，该用户独享全部的硬件和软件资源。
  - 多用户操作系统：一台计算机同一时间只能由多个用户使用，多个用户共享全部的硬件和软件资源。

##### 网卡(netcard)与IP地址(internet protocol address)
  > 网卡是一个专门负责网络通讯的硬件设置，就是那个连接网线的有线网卡和连接wifi的无线网。

  > IP地址就是设置在网卡上的一个逻辑地址信息，它可以唯一标识一个网卡，同时它也是计算机在互联网上的一个唯一标识。

  > IP地址由网络号段和主机号段组成，以“点-数法”表示，分成ABCDE五类；子网掩码的作用就是来区分网络号段和主机号段的；网关实质上也是一个具有路由功能的设备的IP地址。

##### 域名(domain name)与端口号(port number)
  > 域名说白了就是IP的别名，具有更好的易记性，通过域名也能准确定位到一台计算机；而端口号指的是TCP/IP协议的端口号，是用来区分网络上不同的应用程序的，通过域名和端口号可以准确的定位到一台计算机上的服务应用程序。

### 二、细说Linux
##### Linux的历史
  > 1965年贝尔实验室(Bell Labs)、麻省理工(MIT)以及通用电气(GE)合作的MULTICS计划，欲要开发一套多用户的(multi-user)、多任务的(multi-processor)、多层次的(multi-level)操作系统,最终因各种原因失败。

  > 1969年贝尔实验室的Ken Thompson为了能更好的玩“星际旅行”的游戏，历时一个月使用汇编写出了UNIX操作系统的原型；1970年设计出了B语言，使用该语言完成了第一个UNIX(UNICS的谐音)操作系统。

  > 1971年同样酷爱“星际旅行”的Dennis Ritchie加入了Thompson的开发项目进行B语言的改造；到1972年的时候成功在B语言的基础上设计出了C语言；于1973年二人用C语言重写了UNIX操作系统。

  > 1991年芬兰大学生Linus Torvalds在Minix的基础上开发了Linux的原型，之后利用GNU的bash开发环境和gcc编译工具编写了Linux内核；后来和众多爱好者共同完成了现如今强大的Linux操作系统。
##### Linux的概述
  > Linux就是指Linus's UNIX，它是一个开放源代码的UNIX，本意是Linux内核的意思，后来被人们称之为Linux操作系统。具有免费、稳定、安全的特点。

  > Linux分为内核版本和发行版本。
  - 内核版本：是Linux的核心，是运行程序和管理硬件设备的核心程序，官网地址为<https://www.kernel.org>。
  - 发行版本：是封装了Linux内核的功能更加强大的Linux操作系统，又分成RedHat系列(如CentOS)和Debian系列(如Ubuntu)。
  - CentOS官网：<https://www.centos.org>;Ubuntu官网：<https://www.ubuntu.com>;踩点和扫描网址：<https://www.netcraft.com>。

##### Linux的文件系统和目录结构
  > Linux的文件系统是一个层级的树状目录结构，没有盘符的概念，只有一个根目录“/”,所有文件都在它下面。

  > Linux的主要目录介绍：
  - /：根目录，一切目录的源头。一般只保存目录。
  - /boot:启动目录，保存系统启动需要的文件，如内核文件/boot/vmlinuz、引导器/boot/grub。(<font color="red">重点</font>)
  - /bin、/usr/bin；可执行二进制文件目录，保存的是如ls、tar这样的普通命令，可供所有用户使用。(<font color="red">重点</font>)
  - /sbin、/usr/sbin：系统可执行二进制文件目录，保存跟系统管理相关命令，但该目录的命令只能是超级管理员使用。
  - /etc：系统配置文件的默认存放目录，一般只存放配置文件，如/etc/inittab、/etc/init.d、/etc/sysconfig、/etc/fstab、/etc/issue。(<font color="red">重点</font>)
  - /home：系统默认的普通用户的家目录，如新建一个名为jmzc的用户，那么家目录就是/home/jmzc。(<font color="red">重点</font>)
  - /root：超级用户root的家目录。(<font color="red">重点</font>)
  - /lib、/usr/lib、/usr/local/lib：系统的函数库目录，程序执行需要额外调用的函数都保存在这里。
  - /usr：全称是unix sofrware resource,系统软件资源目录，类似于windows中的Program Files目录。(<font color="red">重点</font>)
  - /mnt、/media：挂载目录，前者是用户临时挂着其他文件系统的目录，后者是系统自动识别的设备的默认挂载目录。(<font color="red">重点</font>)
  - /lost+fount：系统异常产生错误时，会将一些遗失的片段放到此目录中。
  - /opt；安装第三方软件的目录；通常我们也会使用/usr/local目录作为第三方软件的安装目录。(<font color="red">重点</font>)
  - /proc:虚拟文件系统，该目录的文件只保存在内存中，如/proc/version、/proc/cpuinfo。
  - /sys、/srv：和/proc目录一样都是跟系统内核相关的目录，非高手不要动它。
  - /tmp：临时文件目录。
  - /dev：硬件设备存放目录。(<font color="red">重点</font>)
  - /var：动态数据的存在目录，如，日志，邮件等。(<font color="red">重点</font>)
  - /selinux：全称security enhanced linux，安全增强子系统，类似于360防护。
  - /dev/null、/dev/zero：2个特殊的目录，一个回收站，一个用于磁盘复制（dd命令）。
### 三、使用Linux你需要知道的几个常识
  > 1. linux中的内容都是以文件的形式保存的，包括硬件，即“在Linux中，一切皆文件”。
  > 2. linux中的存储设置一般需要挂载之后才能使用。
  > 3. Linux作为远程服务器时一般不允许关机，只能重启而且重启前最好同步数据并关闭服务。
  > 4. Linux中的文件不是以后缀名来区分，而是用文件的权限来区分的。
  > 5. linux作为服务器要定期的备份重要数据和日志。
  > 6. 用户密码需要具有规范性、时效性、易记性和复杂性。
  > 7. 通常不要直接使用系统预设用户“root”来登录，而要使用普通用户登录。

