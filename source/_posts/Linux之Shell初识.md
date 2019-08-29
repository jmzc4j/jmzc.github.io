---
title: Linux之Shell初识
date: 2019-08-29 14:32:27
tags: linux
categories: 学习笔记
---
### Shell概述
> Shell是一个命令解释器，为用户提供一个向内核发送请求的界面；同时它还是解释执行的脚本语言。

> Shell分为两大类，B家族Shell和C家族Shell，可以在/etc/shells中查看系统支持哪些Shell。

### Shell语言
##### 编写和执行
> 脚本文件通常以".sh"后缀标记，脚本的开头要标注Shell类型"#!/bin/bash"。

> 执行方式：赋予执行权限，也可以使用“bash xxx.sh”或者“sh xxx.sh”的方式执行。

##### 注释
> 单行注释“#”和多行注释“:!<<...!”

##### 多命令的执行顺序
> ";"：使用分号连接多个命令，这些命令间没有关系，不管前面的命令是否正确执行后面的都会执行。

> "&&"：使用逻辑与连接多个命令，只有前面的命令正确执行后面才执行。

> "||"：使用逻辑或连接多个命令，只有前面的命令不正确后面才执行。

##### Shell中变量
> `set`：可以查看所有变量包括环境变量；`env`：可以查看所有环境变量。
> `unset 变量名`：可以销毁一个变量；`$变量名`：可以使用一个变量。
> `$(命令)`：可以将命令的结果当做变量的值。或者使用反引号也可以。   

> 自定义变量
- 格式：变量=值 （注意：等号两边没有空格）。
- 使用readonly关键字声明静态变量，此时该变量不能被销毁。
- `export 自定义变量`：可以将自定义变量提升为系统变量。

> 系统变量（环境变量）
- 系统变量名一般大写。
- `source`或者`.` 可以重新加载配置文件。

> 位置参数变量
- 用来接受命令中的相应位置的参数。
- `$n`：$0表示这条命令本身，$1-$9、$(10)、$(11)...表示第n个参数。
- `$*`：表示所有的参数，将所有参数作为一个整个。
- `$@`：也表示所有参数，将所有参数分别对待，常用在循环中。
- `$#`：表示所有参数的个数。

> 预定义变量
- `$$`：表示当前进程的PID。
- `$!`：表示后台进程中的最后一个进程的PID。
- `$?`：表示一个命令的执行状态，0表示正确执行。

##### 键盘输入
> `read [选项] [变量]`    
> 常用选项
- -t：该命令的等待时间，单位是秒。
- -p：给出提示信息。
- -s：隐藏输入数据。
- read -s -t 30 -p "请输入你的年龄：" age   

##### 提取命令
> `cut -d 分隔符 -f 指定列` ：按指定分隔符提取指定列，通常和grep配合使用。
- cat /etc/passwd | grep /bin/bash | grep -v root | cut -d ":" -f 1

> `awk '条件1{动作1} 条件2{动作2}...' 文件`
- 还是不太懂！！！

##### sed命令（流编辑器）
> 比vi更强大，用的不多，可以对管道符的结果进行编辑。
- `sed [选项] '动作' 文件`
- 还是不懂！！！

##### 排序命令和统计命令
> `sort`与`wc`

##### 运算符
> `$((运算式))`或者`$[运算式]`：可以使用这样的方式取得运算式的值。

##### 判断符
> `[ 判断式 ]`；注意中括号与判断式中有空格。

> 文件比较
- `A -oq B`和`A -nq B`：判断文件A的修改时间是不是比B旧或者新。
- `-e`、`-d`、`-f`：判断文件是否存在，存在并且是目录、存在并且是文件。
- `A -ef B`：判断A文件与B文件的inode是否一致。

> 数值比较
- `-eq`、`-gt`、`-lt`、`ge`、`le`、`ne`。

> 字符串比较
- `str1==str2`或者`str1 = str2`：判断字符串相等，注意等号的空格。
- `-n`或者`-z`：判断是否是空串，前者非空返回真，后者空返回真。
- `str1!=str2`：不等判断。

##### 流程控制
> if结构

```
if [ 条件 ];then
  程序片段
fi
```
或者
```
if [ 条件 ]
  then
    程序片段
fi
```
或者
```
if [ 条件 ]
  then
    程序片段
  elif [ 条件 ]
    then
      程序片段
  else
    程序片段
fi
```
> case结构

```
case $变量名 in
"值1")
 等于值1的代码
;;
"值2")
 等于值2的代码
;;
*)
 不匹配的代码
;;
esac
```

> for结构

```
for 变量 in 值1 值2 值3 ...
do
  程序片段
done
```
或者
```
for((初始值;条件;增量))
do
  程序片段
done
```

> while结结构

```
while [ 条件式 ]
do
 程序片段
done
```

##### 函数
> 系统函数    
> basename [filepath] [suffix] ：取得filepath路径中的文件名并排除suffix后缀
- basename /home/aaa/bbb.txt .txt   结果是bbb
- basename /home/aaa/bbb.text	结果是bbb.txt

> dirname [filepath] :取得filepath路径的目录部分
- dirname /home/aaa/bbb.txt	结果是/home/aaa

> 自定义函数
```
[function] functionname(){  
 函数体  
 [return xxx]  
}   
```

### 综合案例
> 要求
	1. 每天凌晨2：10备份数据库mydb到/data/backup/db
	2. 备份开始和结束要有提示信息
	3. 备份后的文件以备份时间作为文件名，并打包*.tar.gz
	4. 备份的同时检查有10天前的备份，如果有删除

> 代码

```
#!/bin/bash     

#定义备份目录
BACKUP=/data/backup/db
#获取当前系统时间
NOWDATE=$(date +%Y_%m_%d_%H%M%S)
#定义数据库的主机名
DBHOST=192.168.238.133
#定义数据库的端口
DBPORT=3306
#定义数据库的用户名
DBUSER=root
#定义数据库的密码
DBPWD=root
#定义要备份的数据库
DBNAME=mydb

echo "=====开始备份====="
#确保备份路径存在,路径不存在或者不是目录就创建一个目录
if [ ! -d "$DACKUP/$NOWDATE" ]
then
  mkdir -p "$DACKUP/$NOWDATE"
  echo "备份文件绝对路径为 $DACKUP/$NOWDATE.tar.gz"
fi

#进行mysql数据库的冷备份,比正常备份多了一个压缩（即 | gzip部分且不能直接使用tar）
mysqldump -h$DBHOST -P$DBPORT -u$DBUSER -p$DBPWD $DBNAME | gzip >  $DACKUP/$NOWDATE/$NOWDATE.sql.gz
#切换到备份目录
cd $DACKUP
#进行打包操作
tar -zcvf $NOWDATE.tar.gz  $NOWDATE
#删除临时的目录
rm -rf ./$NOWDATE

#找到备份目录下最后修改时间大于10天并且以.tar.gz结尾的文件进行删除
find $DACKUP -mtime +10 -a -name "*.tar.gz" -exec rm -rf {} \;
echo "=====备份完毕====="
```

