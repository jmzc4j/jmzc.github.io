---
title: Dos命令
date: 2019-08-29 17:57:50
tags: dos
categories: 学习笔记
---
### dos 下查看端口
- `netstat -ano | findstr 4000`：查看4000端口详细信息;
- `tasklist | findstr 6068`：在任务列表中找到对应PID为6068的进程;
- `taskkill /F /PID 6068`：强制杀死6068进程;
	
