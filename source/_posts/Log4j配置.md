---
title: Log4j配置
date: 2019-08-28 18:49:24
tags: log4j
categories: 配置文件
---
### what is
- Log4j是Apache下的一款开源的日志框架;在项目中，我们会结合slf4j和log4j一起使用;
- Log4j提供了简单的API调用，强大的日志格式定义以及灵活的扩展性。我们可以自己定义Appender来满足我们对于日志输出的需求;
- Log4j有三个主要的组件：Loggers(记录器)，Appenders(输出源)和Layouts(布局);

### why use it
- 我们在系统中对于记录日志的需求并不单纯。首先，我们希望日志要能持久化到磁盘，最基本的就是要能够保存到文件中；其次，我们希望在开发和生产环境中记录的日志并不相同，明显开发环境的日志记录会更多方便调试，但放到生产环境下大量的日志很容易会撑爆服务器，因此在生产环境我们希望只记录重要信息。
- 基于不单纯的目的，System.out.println是直接不能满足我们的要求，因此抛弃它，选择功能更强的日志框架。而log4j是apache下一款著名的开源日志框架，log4j满足上面的一切需求。
- 记录日志的框架并不仅仅只有log4j，比较有名的还有logback等，现在比较火的SpringBoot默认集成的日志就是logback。不管哪种日志框架，一般都能够实现日志的持久化功能。

### 日志级别
Loggers组件在此系统中被分为五个级别：DEBUG、INFO、WARN、ERROR和FATAL。这五个级别是有顺序的，DEBUG < INFO < WARN < ERROR < FATAL，分别用来指定这条日志信息的重要程度，明白这一点很重要，Log4j有一个规则：只输出级别不低于设定级别的日志信息，假设Loggers级别设定为INFO，则INFO、WARN、ERROR和FATAL级别的日志信息都会输出，而级别比INFO低的DEBUG则不会输出。

### 输出格式说明：
```
%p：输出日志信息的优先级，即DEBUG，INFO，WARN，ERROR，FATAL。
%d：输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，如：%d{yyyy/MM/dd HH:mm:ss,SSS}。
%r：输出自应用程序启动到输出该log信息耗费的毫秒数。
%t：输出产生该日志事件的线程名。
%l：输出日志事件的发生位置，相当于%c.%M(%F:%L)的组合，包括类全名、方法、文件名以及在代码中的行数。例如：test.TestLog4j.main(TestLog4j.java:10)。
%c：输出日志信息所属的类目，通常就是所在类的全名。
%M：输出产生日志信息的方法名。
%F：输出日志消息产生时所在的文件名称。
%L:：输出代码中的行号。
%m:：输出代码中指定的具体日志信息。
%n：输出一个回车换行符，Windows平台为"rn"，Unix平台为"n"。
%x：输出和当前线程相关联的NDC(嵌套诊断环境)，尤其用到像java servlets这样的多客户多线程的应用中。
%%：输出一个"%"字符。
另外，还可以在%与格式字符之间加上修饰符来控制其最小长度、最大长度、和文本的对齐方式。如：
1) c：指定输出category的名称，最小的长度是20，如果category的名称长度小于20的话，默认的情况下右对齐。
2)%-20c："-"号表示左对齐。
3)%.30c：指定输出category的名称，最大的长度是30，如果category的名称长度大于30的话，就会将左边多出的字符截掉，但小于30的话也不会补空格。
```

### log4j.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration PUBLIC "-//log4j/log4j Configuration//EN" "log4j.dtd">
<log4j:configuration>
    <!--输出到控制台-->
    <appender name="consoleAppender" class="org.apache.log4j.ConsoleAppender">
        <param name="Threshold" value="DEBUG"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </layout>
    </appender>

    <!--输出到文件（info）-->
    <!--将生成“info.log.2014-06-11”这样的日志文件-->
    <appender name="fileAppenderInfo" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="File" value="${user.home}/logs/website/info.log" />
        <param name="DatePattern" value=".yyyy-MM-dd" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </layout>
        <filter class="org.apache.log4j.varia.LevelRangeFilter">
            <param name="LevelMin" value="INFO" />
            <param name="LevelMax" value="INFO" />
        </filter>
    </appender>

    <!--输出到文件（warn）-->
    <appender name="fileAppenderWarn" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="File" value="${user.home}/logs/website/warn.log" />
        <param name="DatePattern" value=".yyyy-MM-dd" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </layout>

        <filter class="org.apache.log4j.varia.LevelRangeFilter">
            <param name="LevelMin" value="WARN" />
            <param name="LevelMax" value="WARN" />
        </filter>
    </appender>

    <!--输出到文件（error）-->
    <appender name="fileAppenderError" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="File" value="${user.home}/logs/website/error.log" />
        <param name="DatePattern" value=".yyyy-MM-dd" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </layout>
        <filter class="org.apache.log4j.varia.LevelRangeFilter">
            <param name="LevelMin" value="ERROR" />
            <param name="LevelMax" value="ERROR" />
        </filter>
    </appender>

    <!--屏蔽所有org.springframework.*输出的Debug（及以下）信息-->
    <logger name="org.springframework">
        <level value="INFO"></level>
    </logger>

    <root>
        <level value="ALL"/>
        <appender-ref ref="consoleAppender" />
     <!--    <appender-ref ref="fileAppenderInfo" />
        <appender-ref ref="fileAppenderWarn" />
        <appender-ref ref="fileAppenderError" /> -->
    </root>
</log4j:configuration>
```

### log4j.properties
```
#默认输出路径
log4j.rootLogger=info,stdout,logfile,busi1
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=logs/cmsmgr.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %F %p %m%n

log4j.logger.com.ibatis=INFO
log4j.logger.com.ibatis.common.jdbc.SimpleDataSource=INFO
log4j.logger.com.ibatis.common.jdbc.ScriptRunner=INFO
log4j.logger.com.ibatis.sqlmap.engine.impl.SqlMapClientDelegate=INFO
log4j.logger.java.sql.Connection=INFO
log4j.logger.java.sql.Statement=INFO
log4j.logger.java.sql.PreparedStatement=INFO
#输出到控制台
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.Threshold=INFO
log4j.appender.stdout.ImmediateFlush=true
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout.ConversionPattern=[%-5p] %d(%r) --> [%t] %l: %m %x %n
#输出到busi1
log4j.logger.busi1= info, busi1
#每天产生一个日志文件
log4j.appender.busi1=org.apache.log4j.DailyRollingFileAppender
#日志文件格式
log4j.appender.busi1.DatePattern='.'yyyy-MM-dd-HH
#日志路径
log4j.appender.busi1.File=logs/busi1.log
#最低输出日志级别
log4j.appender.busi1.Threshold = INFO
#输出的布局样式
log4j.appender.busi1.layout=org.apache.log4j.PatternLayout
#自定义输出哪些信息
log4j.appender.busi1.layout.ConversionPattern=[%d{yyyy-MM-dd HH:mm:ss}] %l%t %m%n


#输出到busi
log4j.logger.busi= info, busi
log4j.appender.busi=org.apache.log4j.DailyRollingFileAppender
log4j.appender.busi.File=logs/busi.log
log4j.appender.busi.Threshold = INFO
log4j.appender.busi.DatePattern='.'yyyy-MM-dd-HH
log4j.appender.busi.layout=org.apache.log4j.PatternLayout
log4j.appender.busi.layout.ConversionPattern=[%d{yyyy-MM-dd HH:mm:ss}] %l%t %m%n
```
### 附件
{% asset_link log4j.properties properties版 %}
{% asset_link log4j.xml xml版 %}