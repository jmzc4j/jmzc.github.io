---
title: Mybatis常用配置
date: 2019-08-28 18:23:58
tags: mybatis
categories: 配置文件
---
### what is
- MyBatis 是一款优秀的持久层框架，它支持定制化SQL、存储过程以及高级映射;避免了几乎所有的JDBC 代码和手动设置参数以及获取结果集;
- MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Old Java Objects)映射成数据库中的记录;
- 官方文档：[mybatis](http://www.mybatis.org/mybatis-3/);

### mybatis-config.xml配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 
		在配置文件中，属性的加载有三种方式：
		1. resource或者url引入外部资源；
		2. 在properties标签中使用property子标签进行定义，然后使用表达式引用；
		3. 直接在要使用的位置硬编码；
		注意：以上三种方式的优先级从低到高；
	 -->
	<properties resource="mybatis/db.properties" />
	
	<!-- 
		全局配置信息
		logImpl：mybatis日志的实现；
		cacheEnabled：二级缓存开关；
		lazyLoadingEnabled：懒加载开关；
		aggressiveLazyLoading：积极懒加载开关，当懒加载时是否主动进行属性的初始化；
		mapUnderscoreToCamelCase：数据库列映射Java对象属性开关；
		useActualParamName：参数命名开关；
	 -->
	<settings>
		<setting name="logImpl" value="slf4j"/>
		<setting name="cacheEnabled" value="true" />
		<setting name="lazyLoadingEnabled" value="true" />
		<setting name="aggressiveLazyLoading" value="false" />
		<setting name="mapUnderscoreToCamelCase" value="true"/>
		<setting name="useActualParamName" value="true"/>
	</settings>
	
	<typeAliases>
		<!-- 
			以包扫描的方式进行别名定义，默认类名首字母小写（注意别名其实是不区分大小写的）；
			当子包有相同的 类名存在时，可以使用@Alias注解进行签名；
		-->
		<package name="top.jmzc.pojo"/>
	</typeAliases>
	
	<plugins>
		<!-- 
			插件配置：以AOP的方式对Executor、MappedStatement、ParameterHandler、ResultSetHandler四大对象的方法进行拦截；
			自定义插件的步骤：
			1. 实现Intercept接口，逐一实现setProperties()、plugin()和intercept()方法；
			2. 为定制的插件进行签名，使用@@Intercepts注解；
			3. 在mybatis配置文件中进行声明；
			
			pagehelper5中可以自动配置dialect，当进行定制配置时，dialect的值一定是全类名，源码中发现使用dialectClass进行反射
		 -->
		<plugin interceptor="com.github.pagehelper.PageInterceptor">
			<!-- <property name="dialect" value="com.github.pagehelper.dialect.helper.MySqlDialect"/> -->
		</plugin>
	</plugins>
	
	<environments default="dev_mysql">
		<!-- 
			配置Session的环境，可以有多个环境，通过environments标签的default属性来切换；
			transactionManager：需要指定事务工厂的类型，这里使用的是别名，也可以实现ibatis提供的接口来进行定制；
			dataSource：需要指定数据源工厂的类型，同样使用的是别名，同样可以实现ibatis提供的接口进行定制；
		 -->
		<environment id="dev_mysql">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${mysql.driver}" />
				<property name="url" value="${mysql.url}" />
				<property name="username" value="${mysql.username}" />
				<property name="password" value="${mysql.password}" />
			</dataSource>
		</environment>
	</environments>
	
	<!-- 
		为不同的数据库厂商的ProductName设置别名；
		ProductName可以从Connection接口的MetaData中得到；
	 -->
	<databaseIdProvider type="DB_VENDOR">
		<property name="MySQL" value="mysql"/>
        <property name="Oracle" value="oracle"/>
        <property name="SQL Server" value="sqlserver"/>
	</databaseIdProvider>
	
	<mappers>
		<!-- 指定映射文件的包路径 -->
		<package name="top.jmzc.mapper"/>
	</mappers>
</configuration>
```

### 附件
{% asset_link mybatis-config.xm %}
l