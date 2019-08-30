---
title: MySQL常用配置
date: 2019-08-28 18:41:59
tags: mysql
categories: 配置文件
---
### my.cnf常用配置
[client]     
\#password = your_password      
port = 3306							#端口设置       
socket = /tmp/mysql.sock 			#本地客户端通讯需要使用的套接字文件，需要保护起来       
default-character-set=utf-8			#客户端默认字符集       

[mysqld]        
lower_case_table_names=1            #设置表名一律转小写，即大小写不敏感设置，Linux下默认是0         
port = 3306							#服务器端口          
basedir="/usr/local/mysql"			#设置mysql的安装目录                
datadir="/usr/local/mysql/data"		#设置mysql数据库的数据的存放目录              
default-storage-engine=INNODB 		#创建新表时将使用的默认存储引擎                
socket = /tmp/mysql.sock 			#服务器与本地客户端通讯的套接字文件位置                 
character-set-server=utf-8			#服务端使用的字符集                 
max_connections=100					#服务器支持的最大并发连接数               
query_cache_size=0					#缓存查询结果的缓存大小              
table_cache=256						#为所有线程打开的表的数量               
tmp_table_size=35M					#内存中的每个临时表允许的最大大小             
thread_cache_size=8					#缓存的最大线程数             

\#MyISAM相关参数                      
myisam_max_sort_file_size=100G  #重建索引时允许使用的临时文件最大大小               
myisam_sort_buffer_size=68M		#快速创建索引的临时文件的缓冲区大小            
key_buffer_size=54M				#缓存MyISAM表索引块的缓冲区大小，不要把它设置得超过可用内存的30%                  
read_buffer_size=64K			#全表扫描时使用的缓冲区大小                
read_rnd_buffer_size=256K		#执行全表扫描的缓冲区的大小                  
sort_buffer_size=256K			#重建索引时为每个线程分配的缓冲区大小                 

\#InnoDB相关参数                 
innodb_additional_mem_pool_size=34M #存储元数据信息的额外内存池大小，一般不需修改                  
innodb_flush_log_at_trx_commit =1	#事务提交频率，1表示每次都直接写入磁盘，不写入内存，更符合ACID的行为；其他值还有0和2，不推荐     
innodb_log_buffer_size=2M			#缓冲日志数据的缓冲区大小，一旦已经满了，InnoDB必须把它刷新到磁盘上。              
innodb_buffer_pool_size=105M		#缓存索引和行数据的缓冲池大小，一般将该值设为物理内存的80%               
innodb_log_file_size=53M			#日志组中每个日志文件的大小。一般设为innodb_buffer_pool_size的25%到100%               
innodb_thread_concurrency=10		#允许连接INNODB内核的最大并发线程数量              

\#SQL模式为严格模式
sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"               

### 附件
{% asset_link mysql.cnf 配置文件 %}
