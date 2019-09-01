---
title: LogBack配置
date: 2019-08-28 18:51:03
tags: logback
categories: 配置文件
---
### what is
-Logback是由log4j创始人设计的另一个开源日志组件,[官方网站] (http://logback.qos.ch);
- 它当前分为下面几个模块：
1. logback-core：其它两个模块的基础模块;
2. logback-classic：它是log4j的一个改良版本，同时它完整实现了slf4jAPI使你可以很方便地更换成其它日志系统如log4j或JDK14 Logging;
3. logback-access：访问模块与Servlet容器集成提供通过Http来访问日志的功能;

### why use it
- 更快的实现：Logback的内核重写了，在一些关键执行路径上性能提升10倍以上。而且logback不仅性能提升了，初始化内存加载也更小了；
- 非常充分的测试：Logback经过了几年，数不清小时的测试。Logback的测试完全不同级别的；
- Logback-classic非常自然实现了SLF4j，在使用SLF4j中，你都感觉不到logback-classic；
- 非常充分的文档：官方网站有两百多页的文档；
- 自动重新加载配置文件：当配置文件修改了，Logback-classic能自动重新加载配置文件。扫描过程快且安全，它并不需要另外创建一个扫描线程；
- Filters（过滤器）：在log4j，只有降低日志级别，不过这样会打出大量的日志，会影响应用性能。在Logback，你可以继续保持那个日志级别；
- 自动压缩已经打出来的log：RollingFileAppender在产生新文件的时候，会自动异步压缩已经打出来的日志文件。
- 自动去除旧的日志文件：设置TimeBasedRollingPolicy或者SizeAndTimeBasedFNATP的maxHistory属性，你可以控制已经产生日志文件的最大数量；

### logback.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
	scan: 当此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true。
	scanPeriod: 设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。当scan为true时，此属性生效。默认的时间间隔为1分钟。
	debug: 当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。
-->
<configuration debug="false" scan="true" scanPeriod="60 seconds">
	<!-- 上下文名称，用于区分不同应用程序的记录 默认default -->
	<contextName>default</contextName>

	<!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径 -->
	<property name="LOG_HOME" value="./log" />
	<!-- 定义日志布局的转换样式 -->
	<!--格式化输出：%d表示日期，%p优先级，%c类全限定名，%M方法名，%L行号，%m日志信息，%n换行 -->
	<property name="layout_pattern" value="%d{yyyy-MM-dd HH:mm:ss}[%p][%c][%M][%L] -> %m%n" />
	<property name="layout_pattern2" value="%date [%thread] %-5level %logger{36} [%line] - %msg%n"/>
	<!-- 定义要保存的最大归档文件数量，异步删除旧文件 （需要结合滚动pattern具体分析） -->
	<property name="logFile_period" value="30" />
	<!-- 定义单个日志文件的最大为10M 超出此大小则生成新文件 -->
	<property name="logFile_maxSize" value="10MB" />

	<!-- 邮件参数设置 -->
	<!-- 定义邮件服务器地址 -->
	<property name="smtpHost" value="smtp.163.com" />
	<!-- smtp端口 ，默认值25 -->
	<property name="smtpPort" value="25" />
	<!-- 发件人用户名 -->
	<property name="username" value="jmzc_top" />
	<!-- 发件人授权码 -->
	<property name="password" value="shouquanma" />
	<!-- 安全连接 默认false -->
	<property name="SSL" value="false" />
	<!-- 收件人邮箱 -->
	<property name="email_to" value="jmzc_top@aliyun.com" />
	<!-- 发件人邮箱 -->
	<property name="email_from" value="jmzc_top@163.com" />
	<!-- 主题 默认%logger{20} - %m -->
	<property name="email_subject" value="【Error】: %logger" />

	<!-- 输出日志到控制台 -->
	<appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<pattern>${layout_pattern}</pattern>
			<charset>UTF-8</charset>
		</encoder>
		<!-- 过滤器，记录>=DEBUG级别的日志 -->
		<filter class="ch.qos.logback.classic.filter.ThresholdFilter">
			<level>DEBUG</level>
		</filter>
	</appender>
	<!-- 输入日志到文件 DEBUG级别 -->
	<appender name="FileDebug" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
			<FileNamePattern>${LOG_HOME}/debug.%d{yyyyMMdd}.zip</FileNamePattern>
			<MaxHistory>${logFile_period}</MaxHistory>
		</rollingPolicy>
		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<pattern>${layout_pattern}</pattern>
		</encoder>
		<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
			<MaxFileSize>${logFile_maxSize}</MaxFileSize>
		</triggeringPolicy>
		<filter class="ch.qos.logback.classic.filter.ThresholdFilter">
			<level>DEBUG</level>
		</filter>
		<!-- 过滤器，只记录DEBUG级别的日志 -->
		<!-- <filter class="ch.qos.logback.classic.filter.LevelFilter">
			<level>DEBUG</level>
			<onMatch>ACCEPT</onMatch>
			<onMismatch>DENY</onMismatch>
			</filter> -->
	</appender>

	<!-- 输入日志到文件 ERROR级别 -->
	<appender name="FileError" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
			<!-- 设置文件滚动的样式 %d{yyyyMMdd}表示每天滚动 或者达到最大文件大小后滚动 .gz | .zip | .log | .txt -->
			<FileNamePattern>${LOG_HOME}/error.%d{yyyyMMdd}.zip</FileNamePattern>
			<MaxHistory>${logFile_period}</MaxHistory>
		</rollingPolicy>
		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<pattern>${layout_pattern}</pattern>
		</encoder>
		<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
			<MaxFileSize>${logFile_maxSize}</MaxFileSize>
		</triggeringPolicy>
		<filter class="ch.qos.logback.classic.filter.ThresholdFilter">
			<level>ERROR</level>
		</filter>
	</appender>

	<!-- 输出日志到邮件 janino.jar和mail.jar -->
	<appender name="Email" class="ch.qos.logback.classic.net.SMTPAppender">
		<!-- 基于标记和日志等级发送邮件 -->
		<evaluator class="ch.qos.logback.classic.boolex.JaninoEventEvaluator">
			<expression>
				(level > WARN &amp;&amp; null != throwable)||(marker != null &amp;&amp; marker.contains("DEVELOP"))
			</expression>
		</evaluator>
		<!-- 一个邮件只包含一个日志条目 -->
		<cyclicBufferTracker class="ch.qos.logback.core.spi.CyclicBufferTracker">
			<bufferSize>1</bufferSize>
		</cyclicBufferTracker>
		<!-- 设置同步发送，异步不好用？？？ -->
		<asynchronousSending>false</asynchronousSending>
		<smtpHost>${smtpHost}</smtpHost>
		<smtpPort>${smtpPort}</smtpPort>
		<SSL>${SSL}</SSL>
		<username>${username}</username>
		<password>${password}</password>
		<to>${email_to}</to>
		<from>${email_from}</from>
		<subject>${email_subject}</subject>
		<layout class="ch.qos.logback.classic.PatternLayout">
			<Pattern>${layout_pattern}</Pattern>
		</layout>
	</appender>

	<!-- root是所有logger的祖先 -->
	<!-- 日志输出级别 TRACE < DEBUG < INFO < WARN < ERROR -->
	<root level="INFO">
		<appender-ref ref="Console" />
		<!--
			<appender-ref ref="FileDebug" />
			<appender-ref ref="FileError" />
			<appender-ref ref="Email" />
		-->
	</root>
	
	<!-- show parameters for hibernate sql 专为 Hibernate 定制 -->
	<logger name="org.hibernate.type.descriptor.sql.BasicBinder" level="TRACE" />
	<logger name="org.hibernate.type.descriptor.sql.BasicExtractor" level="DEBUG" />
	<logger name="org.hibernate.SQL" level="DEBUG" />
	<logger name="org.hibernate.engine.QueryParameters" level="DEBUG" />
	<logger name="org.hibernate.engine.query.HQLQueryPlan" level="DEBUG" />
	 
	<!--myibatis log configure-->
	<logger name="com.apache.ibatis" level="TRACE"/>
	<logger name="java.sql.Connection" level="DEBUG"/>
	<logger name="java.sql.Statement" level="DEBUG"/>
	<logger name="java.sql.PreparedStatement" level="DEBUG"/>
</configuration>
```
### 附件
{% asset_link logback.xml 配置文件 %}