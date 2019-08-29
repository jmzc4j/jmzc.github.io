---
title: Linux命令
date: 2019-08-29 14:32:09
tags: linux
categories: 学习笔记
---
### 命令格式
> 命令名 [选项] [参数]
- 中括号“[]”代表可选。
- 选项可以使以减号“-”开头的缩写，还可以是以双减号“--”开头的英文全称，可以多个选项联合使用。
- 参数可以使0个、1个、多个。

### 常用命令   
##### 帮助   
> `man 命令名`：用来查看指定命令名的帮助，该命令也可以查看配置文件的帮助。

> `命令名 --help`：用来查看命令选项的帮助。

> `whereis 命令名`：可以查找命令的所在目录及该命令帮助的所在目录。

##### 清屏
> `ctrl+l`或者`clear`：清屏。   
> `ctrl+u`和`ctrl+y`：剪切和粘贴命令

##### vi/vim编辑器
> `vi 文件`：创建或打开一个文件。

> 命令行模式（一般模式）：使用vi刚刚打开文件的模式。   
> 常用命令
- `gg`、`G`、`nG`、`0`、`$`：移动光标都第一行、最后一行、第n行、当前行行首、当前行行尾。
- `x`、`D`、`dd`、`ndd`、`dG`：删除当前光标处、剪切光标到行尾、当前行、当前行开始的n行、当前行到最后一行的内容。
- `yy`、`nyy`、`p`、`P`、`r`、`R`、：复制当前行、当前行开始的n行、粘贴到光标下一行、上一行、替换当前光标的字符、光标出开始替换直到ESC结束。
- `u`：撤销上一次操作。
- `ZZ`：退出vi。

> 插入模式（编辑模式）：从命令行模式输入`aioAIO`中任意一个进入的模式，使用`ESC`退出。

> 底行模式（冒号模式）：从命令行模式输入`:`进入模式。   
> 常用命令
- `set nu(set nonu)`：设置显示或取消行号。
- `linenumber`：移动光标到指定的linenumber行。
- `/keyword（？keyword）`：按关键词从光标处向下查找或向上查找。
- `set ic(set noic)`：设置忽略或区分大小写。
- `n1,n2d`：删除n1行到n2行之间的内容。
- `%s/old/new/g`：使用新串全文替换旧串。
- `n1,n2s/old/new/g`：使用新串在指定行间进行替换旧串。
- `w`、`w 目录`、`wq`、`q!`、`wq!`：保存、另存、保存退出、不保存退出、强制保存退出。

> 小技巧
- `:map 快捷键 操作`：自定义快捷键，如 :map ^p I#<ESC> 。
- `:ab 缩写 原意 `：为全称定义一个缩写名字，然后使用缩写名字+空格即可完成全名的输入。
- `~/.vimrc`：可以在此文件中定义底行模式的命令使其永久生效。

##### 定义别名
- `alias 别名 命令`：为命令定义一个临时别名。
- `~/.bashrc`：编辑该文件可以永久定义别名。

##### 目录与文件命令
> `ls [选项] [目录或者文件]`：查看目录或文件的内容或属性。   
> 常用选项-alh
- -a：显示隐藏文件。
- -l：长列表显示。
- -h：人性化显示。
- -i：显示i节点信息。
- 可以使用通配符(\*、?)或者正则匹配([]、[-]、[^]...)。

> `tree [目录]`：列出指定目录的树状结构并统计目录和文件数量。

> `pwd`；查看当前工作目录的绝对路径。

> `cd [目录]`：切换目录。   
- cd ~或者cd：切换到家目录。
- cd ..：切换到上一级目录。

> `mkdir [选项] 目录`：创建目录。    
> 常用选项-pm
- -p：创建多级目录。
- -m: 使用数字表示法指定目录的权限。

> `cp [选项] 目录或文件 目标目录`：复制目录或文件到已存在的目标目录或。
> 常用选项-ri
- -r：递归复制目录内容。
- -i：如果遇到同名文件会提示是否覆盖。

> `mv [选项] 目录或文件 目标目录`：剪切目录或文件到已存在的目标目录或者执行改名操作。   
> 常用选项-if
- -f：如果遇到同名文件不会提示是否覆盖。
- -i：--interactive。如果遇到同名文件会提示是否覆盖.

> `rm [选项] 目录或者文件`：删除一个目录或者文件。    
> 常用选项-rf
- -r：递归删除目录及目录下的所有内容。
- -f：--force，不在有提示信息强制删除。

> `touch 文件`：创建一个新的空白文件或者是更新一下已存在文件的修改时间。

> `ln -s 目录或文件 软链接文件`：为一个文件或目录创建一个软链接，类似于快捷方式，软连接文件的权限是777.

##### 查看文件内容命令
> `cat [选项] 文件`：使用与浏览内容较少的文件。   
> 常用选项-bn
- -b：--number-nonblank，行号只标记有效行(即非空白行)。
- -n：--number，带行号显示，不挑剔。

> `more 文件`：可以分页查看文件内容。  
> 辅助命令f、b、enter、q
- b：上一页。
- f：下一页，也可以使用空格键。
- enter：下一行。
- q：退出。

> `less [选项] 文件`：同样是分页查看文件内容，但是可以带行号，还能搜索。    
> 常用选项MN
- -M：使用后类似于more的百分比。
- -N：带行号显示。
- 辅助命令同more，可以使用“/keyword”进行搜索。

> `head [-n 数字] 文件`；显示到指定行号的文件内容，默认显示前10行。

> `tail [选项] 文件`；从尾部开始显示文件内容，默认显示后10行。    
> 常用选项-nf
- -n 数字：显示指定的后几行。
- -f：--follow，跟踪显示文件的变化，常用来查看日志。

##### 输出重定向与管道命令、历史命令
> `> 文件`:将一个到终端的输出重定向到文件中，即向文件中覆盖内容。   
> `>> 文件`：同样是重定向到终端的输出，追加到文件的末尾。
> `&>> 文件`：将错误和正确结果都追加到文件末尾。

> `命令A | 命令B`：将命令A的输出结果通过管道符作为命令B的输入参数，可以通过管道符“|”执行多个命令。

> `ehco 字符串`：输入指定内容到终端，相当于编程语言中的打印。

> `history [数量]`：可以查看历史命令列表或者查看指定数量的历史命令列表，之后可以使用“!历史命令编号”来执行指定命令。    
> `~/.bash_history`：历史命令保存在该文件中，默认保存上限可以在profile中修改。

##### 查找命令
> `find 目录 搜索条件`：在指定目录范围内根据指定条件查找指定的内容。    
> 常用选项
- -name：按名称查找，可以使用通配符或者正则。
- -size：按大小查找，如-size +200K（大于200K的）、size -10M（小于10M的）。
- -user：按用户名查找。
- -type：按文件类型查找。如-type "d"、-type "-"。
- -mtime：按文件的最后修改时间查找，该时间单位是天，如-mtime +10（最后修改时间距现在10天以上的）。
- 可以使用“-a”或者“-o”进行条件的拼接，前者表示and，后者表示or。
- 可以使用“-exec”选项来对查找的结果进行处理，如find /etc -name init -exec ls -l `{} \;`,着重部分是固定格式。

> `locate [-i] 要查找的内容`：快速查找或者忽略大小写查找。    
> 说明
- 可能系统为安装该命令，需要执行yum -y install mlocate。
- 每次使用前最好执行updatedb命令更新locate检索库。

> `grep [选项] 字串 文件或者是命令的输出结果`：从指定的文件或者一个命令输出结果中过滤查找需要的内容。   
> 常用选项-ivn
- -i：忽略大小写查找。
- -v：排除指定字串查找。
- -n:查找结果带行号显示。
- 字串可以使用通配符或者正则，如*、？、^、$。

##### 时间日期命令
> `date [-s] [+时间格式`：按指定格式设置或显示系统时间。   
> 格式说明：%Y表示年份、%m表示月份、%d表示天、%H表示0-23的小时、%M表示分钟、%S表示秒。

> `cal [年份]`：查看当前时间的日历或者查看指定年份的日历。

> `ntpq -p`：与NTP服务器进行时间同步，CentOS7请使用`chronyc sources`命令。

> `/etc/sysconfig/clock`：配置时区，CentOS7使用`tiemdatectl set-timezone Asia/Shanghai`命令。

> `/etc/sysconfig/i18n`：配置语言环境，CentOS7使用`localectl set-locale LANG=zh_CN.UTF-8`命令。

##### 文件压缩和解压缩命令
> `gzip 文件`和`gunzip 文件`：以gzip格式压缩和解压文件，只能压缩文件且不会保留原文件。

> `zip [-r] 文件或目录 压缩后的文件 要压缩的文件`：以zip格式压缩目录或文件。   
> `unzip [-d 目录] 要解压的文件`：以zip格式解压文件到当前目录或指定目录。

> `tar [-zcvf] 要压缩打包的目录或文件`：以gzip压缩并使用tar进行打包指定文件。   
> `tar [-zxvf] 要解压的文件 [-C 目标目录]`：解压.tar.bz2文件到当前目录或指定的已存在的目录。

> `tar [-jcvf] 要压缩打包的目录或文件`：以bzip2压缩并打包指定文件。   
> `tar [-jxvf] 要解压的文件 [-C 目标目录]`：解压.tar.bz2文件到当前目录或指定目录。
##### 网络与服务命令
> `ipconfig`或者`ip addr`：查看网卡信息，后者是CentOS7上或者Ubuntu18上使用。

> `ping IP地址`：测试与目标IP地址的连通性。

> `netstat -rn`：可以查看路由列表。   
> `netstat -tulnp`或者`netstat -anp`：查看进程的网络连接状态。   
> 选项说明：r表示路由、n表示IP地址和端口号、t表示tcp连接、u表示udp连接、l表示监听状态、p表示PID和应用、a表示全部。

> 如何配置静态IP地址
- CentOS中配置方式：修改 /etc/sysconfig/network-scripts/ifcfg-eth0(或者ens33)配置文件，修改内容类似下面。

```
# 修改部分
BOOTPROTO="static" # 设置使用静态IP协议
ONBOOT="yes"  # 设置开机启动网络服务
# 添加部分
DNS1="114.114.114.114" #设置DNS1
DNS2="8.8.8.8"
GATEWAY="192.168.124.2" # 设置网关
IPADDR="192.168.124.124"  # 设置IP地址
NETMASK="255.255.255.0"  # 设置子网掩码

# 重启网络服务CentOS6.x：network，CentOS7.x:NetworkManager
```

- Ubuntu18中配置方式：修改/etc/netplan/01-network-manager-all.yaml配置文件，修改内容类似下面。

```
network:
　version: 2
    renderer: NetworkManager
    ethernets:
　　ens33:
　　dhcp4: no #关闭动态主机配置协议
　　addresses: [192.168.124.125/24] #设置IP地址，24表示24位的子网掩码(即255.255.255.0)
　　gateway4: 192.168.124.2 #设置网关
　　nameservers:
　　　　addresses: [8.8.8.8, 114.114.114.114] #设置DNS

# Ubuntu18中重启方式：sudo netplan apply
```
-

> `ls /etc/init.d`：可以系统中通过rpm安装的独立服务列表。

> 服务管理
- `/etc/init.d/服务名 stop|start|restart|status`:对某个服务停止、启动、重启，查看状态。
- `service 服务名 stop|start|restart|status`。
- `systemctl stop|start|restart|reload|status 服务名`。（CentOS7的管理方式）
- `service --status-all`；查看所有的服务状态。CentIS7使用`systemctl list-units`。

> 自启动管理
- `chkconfig --list [| grep xxx]` : 查看所有服务或者具体服务在各个运行级别的状态。    
- `chkconfig [--level runlevel] 服务名 on/off`：设置服务在所有级别或指定级别的开启自启与关闭。   
- `systemctl enable/disable xxx.service`：同样可以设置开机自启与关闭，使用于CentOS7.
- `systemctl list-unit-files`：查看所有的自启动状态。

> 防火墙管理
- `/etc/init.d/iptables`：配置该文件可放开端口。
- `firewall-cmd --zone=public [--remove-port|--query-port|--add-port]=80/tcp --permanent `：CentOS7的方式。
- `firewall-cmd --reload`：重启防火墙。
- `firewall-cmd --zone=public --list-ports`：查看防火墙列表。

##### 系统运行级别管理命令
> 系统运行级别分为六大类（括号内的表示在CentOS7中的标识）。
- 0：表示关机。 （poweroff.target）。
- 1：表示单用户模式 （emergency.target）。
- 2：表示多用户无网络模式 （rescure.target）。
- 3：表示多用户有网络模式 （multi-user.target）。
- 4：表示保留级别，未被使用。（无）
- 5：表示图形界面模式 （graphical.target）。
- 6：表示重启 （reboot.target）。
- CentOS7中的这些名称可以通过`ll /lib/systemd/system/runlevel*`查到。

> `runlevel`：能够查看系统之前和当前的运行级别。    
> `vi /etc/inittab`；通过该配置文件也可以查到系统运行级别。    
> `systemctl get-default`；在CentOS7中可以通过该命令查看当前运行级别。

> `init 级别ID`；可以切换运行级别。   
> `vi /etc/inittab`：在该文件中配置默认运行级别。     
> `systemctl set-default xxx.target`：CentOS7的配置默认运行级别的方式。

##### 进程管理命令
> `ps aux`与`ps au`：查看当前系统进程的详细信息，一般使用后者即可，表示只显示终端上的进程。   
> `ps -ef`：同样能查看当前系统进程信息，还能查到父进程的PID。     
> `pstree [选项]`：查看系统进程树。   
> 常用选项：
- -p：查看指定PID的进程。
- -u：查看指定用户名的进程。    

> `top`：动态实时的显示系统进程并排序，还可以监控系统的健康状态。

> `kill -1 PID`：重启指定PID的进程。    
> `kill [-9] PID`：结束或强制结束指定PID的进程。   
> `killall 进程名`：结束与指定进程相关的所有进程。    
> `pkill` : 可以用来踢出指定终端的用户

##### 用户和组管理命令
> 相关配置文件分别是`/etc/passwd`、`/etc/shadow`、`/etc/group`、`/etc/gshadow`。
- /etc/passwd：分别表示用户名、标识口令、UID、初始GID、备注、家目录、登录Shell。
- /etc/shadow：分别表示用户名、加密口令、最新修改日期、下次可修改间隔、有效期限、提前警告天数、过期可用天数、截止日期、保留。
- /etc/group：分别表示组名称、标识口令、组ID、组用户列表。
- /etc/gshadow：分别表示组名称、加密口令、组管理者列表、组用户列表。

> `useradd [选项] 用户名`：添加新用户。    
> 常用选项
- -g：添加到初始组，一个用户只能有一个初始组（也称之为主组）。
- -G：添加到附加组，一个用户可以有多个附加组（也就是组配置文件中用户列表中所对应的组）。
- -d：指定家目录的位置。
- -u：指定UID，普通用户的UID都是500之后的数字，超级用户的UID是0。
- -s：设置使用的登录shell类型
- -r：--system ，创建一个系统账号，这样的账号不会再/home下创建家目录。
- -m：--creat-home，设置要在/home下创建家目录。
- -M：--no-create-home ，设置不创用户的家目录。

> `passwd 用户名`：为指定用户设置口令。

> `userdel [-r] 用户名`：删除指定用户或者同时删除家目录。   
> `usermod [选项] 用户名`：可以修改用户信息，选项同新增用户选项。

> `id [用户名]`；查看用户的UID和GID信息。   
> `who`或者`w`；查看所有登录用户的详细信息。
> `whoami`：查看当前用户名称。

> `su [-] 用户名`：切换到指定目录。   
> 可以不用切换root用户，而使用`sudo`来管理系统。

> `groupadd 组名`、`groupdel 组名`、`groupmod 组名`。

##### 文件权限管理命令
> 使用`ls -l`可以查看文件或目录的详细信息。     
- 这些信息分别表示文件类型和ugo权限、硬链接数(可访问方式的数量)、所有者、所属组、文件大小、最后修改时间、文件名（即basename）。
- 文件的rwx：表示可查看、修改文件内容、可执行该文件。
- 目录的rwx：表示可查看、在目录中创建目录文件或修改目录内容、可进入该目录。

> `chmod [-R] {[ugoa]+/-/=[rwx]} 目录或文件`：修改(添加、减少、设置)所有者或所属组或其他用户或所有用户对指定目录或文件的读写执行权限。   
> `chmod [-R] [rwx=421] 目录或文件`：使用数字表示法设置目录或文件的权限，如chmod 755 temp.sh。

> `chown [-R] 用户名  目录或文件`：变更目录或文件的所有者为指定用户。    
> `chgrp [-R] 组名 目录或文件`： 变变目录或文件的所属组为指定组。      
> `chown [-R] 用户名:组名 目录或文件`：同时变更目录或文件的所有者和所属组。

##### 磁盘管理命令
> `lsblk [-f]`：查看分区和挂载信息,-f,--fs。   使用`fdisk -l`也能查看。
> `df [-lha]`：查看整个磁盘的使用情况,-l,--local。   
> `du [-ach --max-depth=0开始的数字]  [目录或文件]`：查看指定目录或目录所有内容的磁盘使用情况并统计，还能灵活的指定目深度。    
> `du -sh 目录`

> `mount`：查看系统中挂载哪些分区。
> `mount /dev/sr0 /mnt/cdrom`：挂载光驱设备到/mnt/cdrom。      
> `mount -t vfat /dev/sdb1 /mnt/usb`：挂载一个fat32格式的U盘，U盘的设备名称也是以“sd”开头的。   
> Linux默认不支持NTFS格式的U盘，需要安装ntfs-3g软件包才能正常使用。    

> 如何为系统添加一块新硬盘(以第二块硬盘sdb为例)。
- 分区分为MBR(主引导记录)和GPT(GUID分区表)两大类，前者主分区最多4个，支持硬盘容量有限；后者主分区无上限，支持容量达18EB，可能受操作系统限制。
- 对于SCSI(小型机系统接口)或SATA(串行高级技术附件)硬盘在Linux文件系统中都是保存在/dev下的以'sd'开头的目录中。
- 准备好要添加的硬盘。
- `fdisk /dev/sdb`：为sdb硬盘进行分区。
- `mkfs /dev/sdb`：格式化sdb硬盘，根据提示进行，格式化进行将文件系统写入硬盘。
- `mkdir /mnt/newdisk`：创建挂载点。
- `mount /dev/sdb /mnt/newdisk`：执行临时挂载操作，当然也可通过`unount`进行卸载。
- `vi /etc/fstab`：修改配置文件以便使挂载永久生效。
- `mount -a`：根据/etc/fstab的配置重新进行挂载。

##### 定时调度命令
> `crontab -e`：打开定时调度配置文件进行编辑
- 从左到右五个时间符号分别表示：分钟、小时、日、月、星期。
- “\*”表示任意时间，“\*/n”表示每隔多长时间，“-”表示一段时间，“，”表示几个时间点。    

> `crontab -l`：查看有定时任务列表。    
> `crontab -r`：删除所有定时任务。

##### 软件包管理命令
> rpm管理   
- `rpm -qa 包名`：查询指定rpm包的安装情况。   
- `rpm -ql 包名`:查询指定rpm包的安装位置。    
- `rpm -qR 包名`:查询指定rpm包的依赖信息。   
- `rpm -qf 目录`：查询指定目录下安装的rpm包列表。
- `rpm -ivh 包全名`；安装指定rpm包，显示安装详情和安装进度。
- `rpm -e [--nodeps] 包名`：卸载或者强制卸载指定rpm包。
- `rpm -Uvh 包名`：升级安装包。

> yum管理
- `yum [-y] list`：查询安装列表。
- `yum [-y] install 包名`：安装。
- `yum [-y] remove 包名` ：卸载。
- `yum [-y] update 包名`：升级。
- `yum [-y] grouplist`：查询软件组列表。
- `yum [-y] groupinstall 软件组名`、`yum [-y] removegroup软件组名`。
- 可以更新yum源：位置在`/etc/yum.repos.d/`目录下。

> Ubuntu apt管理
- `sudo apt-get update`：修改软件源之后可以使用该命令重新加载软件源。
- 源更新方式可以去tuna上找：<https://mirrors.tuna.tsinghua.edu.cn/> 。
- `sudo apt-get install 包名`：安装包。
- `sudo apt-get remove 包名`：卸载包。
- `sudo apt-get upgrade 包名`：更新已安装的包。
- `sudo apt-cache show 包名`：获取包的相关信息。
- `sudo apt-get source 包名`：下载该包的源代码。

##### 关机和重启命令
> `shutdown -r time `：指定时间重启，默认是1分钟。`shutdown -r now`等价于`reboot`。    
> `shutdown -h time` ：指定时间关机。`shutdown -h now`等价于`halt`。   
> `shutdown -c`；取消上一个shutdown命令。
> `sync`：同步命令，同步数据到磁盘。

##### 其他命令
> `hostname`：查看主机名。    
> `uname -a`：查看内核版本详情。   
> `/proc/cpuinfo`；查看cpu信息。   
> `free -m`：以MB单位查看内存信息。   
> `/etc/issue`：查看查看发行版本。   

### SSH
> windows上使用公私密钥对登录  
- 创建一组密钥对并保存好私钥。
- 将公钥内容导入到指定用户的家目录的.ssh目录下的authorized_keys文件中。
- 禁用密码登录 vi /etc/ssh/sshd_config 设置密码认证为no。
- service sshd restart
- 禁用root登录：vi /etc/ssh/sshd_config -> PermitRootLogin no
- 改掉ssh端口：vi /etc/ssh/sshd_config -> Port=xxx

> Linux上使用ssh
- ssh -p username@IP
- ssh-sshgen -t rsa -b 2048
- ssh-copy-id username@IP

##### SCP
- scp -P user@服务器IP:绝对路径 客户机文件    

##### SFTP
- sftp -oPort user@IP
- 使用help查看使用

