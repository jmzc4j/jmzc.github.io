---
title: Mybatis源码分析
date: 2019-08-30 23:15:41
tags: mybatis
categories: 学习笔记
---
### Mybatis架构
- 轻量级的半自动ORM框架，大致可分四层：
	1. 引导层：基本配置，通过Xml配置方式或者是Java API方式；
	2. 框架层：环境管理，通过配置文件对各种环境初始化，如连接池管理、事务管理、sql语句管理、缓存管理；
	3. 接口层：通过接口调用的方式实现对数据的增删改查，如Mapper接口、配置维护接口、数据增加接口、删除接口、添加接口、查询接口；
	4. 数据处理层： 完成sql的解析、sql的执行、参数的映射、返回结果处理和映射，分别对应SqlSource、Executor、ParameterHandler和ResultSethandler；
- 不多说，来一张架构图
{% asset_img mybatis架构.png %}

### Mybatis参数封装
1. 对于单个参数：
	- 可以不进行任何处理
	- 使用Param注解
	- useActuaParamName=true
	- 对于Collection、List、Array类型使用collection、list、array作为key
2. 对于多个参数：
	- 封装成一个TO
	- 封装成一个Map
	- 使用多个Param注解
	- 取值时使用param1、param2...或者0、1...作为key
	- useActuaParamName=true时直接使用形参作为key
3. 对于返回值为Map时，需要添加MapKey注解；
4. 来一个简单的源码分析
{% asset_img source05.png %}

5. 若看不清图，请看planUML源码
	```
	@startuml
	scale 2/3
	autonumber
	title 输入参数包装过程

	MapperMethod->MapperMethod: convertArgsToSqlCommandParam(Object[] args)
	MapperMethod->ParamNameResolver: paramNameResolver.getNamedParams(args)
	ParamNameResolver->ParamNameResolver: new ParamNameResolver(config, method)
	ParamNameResolver->ParamNameResolver: 为SortedMap<Integer, String> names初始化
	note right
		初始化过程：
			1. SortedMap<Integer, String> map = new TreeMap<>();
			2. 是否有@Param注解，有就用注解指定的值；
			3. 是否在全局配置中配置了<setting name="useActualParamName" value="true"/>
				配置了就直接使用形参的值（需要JDK8以上，因为使用了Stream API和方法引用）
			4. 如果上述两种都不存在，就使用Map集合的size()作为值
			总结： key的取值：从0开始；value的值可能是注解的值、形参的值或者是size(）的值;
	end note

	ParamNameResolver->ParamNameResolver: getNamedParams(Object[] args)
	note right
		参数封装过程：
			1. args==null或者names集合中没有值，返回null；
			2. 没有@Param注解并且只有一个形参时，直接从names集合中取值；
			3. 其他情况都会将形参列表封装到一个Map中，将names集合的key作为args的索引，然后将每个形参作为值添加到Map中
			那么Map的key有这几种情况：
				1. 以names集合的value作为Map的key
					- 存在@Param：key就是注解的值
					- 开启useActualParamName：key就是形参的值
					- 其他情况，使用形参的索引作为key
				2. 以param作为前置，后面从1开始递增，即param1、param2...
				
	end note

	note left
		在DefaultSqlSession中存在  Object wrapCollection(final Object object)也会对特殊形参包装
		1. 对于Colection、List和Array类型的形参同样是会被封装到一个Map中；
			- Collection类型的key： collection
			- List类型的key: list
			- Array类型的key：array
		2. 其他类型不进行处理；
		
		当我们的查询返回的是一个Map的时候，需要使用@MapKey注解来指定使用哪个属性来作为key
	end note

	center footer 即 MapperMethod.convertArgsToSqlCommandParam(Object[] args)分析
	@enduml
	```

### Mybatis获取SqlSessionFactory
1. 使用Resources工具加载配置资源文件到文件流中
2. 使用SqlSessionFactoryBuilder创建一个DefaultSqlSessionFactory；
3. 在2的过程中会使用XmlConifgBuilder和XmlMapperBuilder分别解析资源文件，从而进行Configuration的初始化；
4. 用一张图来看下具体流程
{% asset_img source01.png %}

5. 若看不清图，请看planUML源码
	```
	@startuml
	scale 2/3
	autonumber
	title DefaultSqlSessionFactory初始化过程

	SqlSessionFactoryBuilder->SqlSessionFactoryBuilder: new SqlSessionFactoryBuilder()
	SqlSessionFactoryBuilder->XMLConfigBuilder: build(reader)
	SqlSessionFactoryBuilder->XMLConfigBuilder: build(reader,  environment, properties)
	XMLConfigBuilder->XMLConfigBuilder: new XMLConfigBuilder(reader, environment, properties)
	XMLConfigBuilder->XMLConfigBuilder: parse()开始解析配置文件
	XMLConfigBuilder->Configuration: 最终通过parseConfiguration(parser.evalNode("/configuration"))初始化Configuration中全局配置
	XMLConfigBuilder->XMLMapperBuilder: mapperElement(root.evalNode("mappers")
	XMLMapperBuilder->XMLMapperBuilder: new XMLMapperBuilder()
	XMLMapperBuilder->XMLMapperBuilder: parse()开始解析映射文件
	XMLMapperBuilder->Configuration: 最终通过configurationElement(parser.evalNode("/mapper"))初始化Configuration映射配置
	Configuration-->XMLConfigBuilder: 返回configuration
	XMLConfigBuilder->DefaultSqlSessionFactory: build(configuration)创建了DefaultSqlSessionFactory
	DefaultSqlSessionFactory-->SqlSessionFactoryBuilder: 返回DefaultSqlSessionFactory

	center footer 需要Configuration对象的参与，dom解析使用的是xPath
	@enduml
	```

### Mybatis获取SqlSession
1. 使用SqlSessionFactory创建一个DefaultSqlSession；
2. 在1的过程中会通过TransactionFactory创建tx，然后将tx作为参数创建Executor（默认情况下是SimpleExecutor）；
3. 具体过程如下图：
{% asset_img source02.png %}

4. 若看不清图，请看planUML源码
	```
	@startuml
	scale 2/3
	autonumber
	title DefaultSqlSession初始化过程

	DefaultSqlSessionFactory->Configuration: open()
	DefaultSqlSessionFactory->Configuration: openSessionFromDataSource(execType, level, autoCommit)
	Configuration->TransactionFactory: getTransactionFactoryFromEnvironment(environment)
	TransactionFactory->Transaction: newTransaction(dataSource, level, autoCommit)
	Transaction-->TransactionFactory: 返回Transaction
	TransactionFactory->Executor: newExecutor(tx, execType)创建默认的SimpleExecutor
	TransactionFactory->Executor: 如果开启二级缓存创建CachingExecutor
	Executor->DefaultSqlSession: new DefaultSqlSession(configuration, executor, autoCommit)
	DefaultSqlSession-->DefaultSqlSessionFactory: 返回一个关闭自动提交的DefaultSqlSession

	center footer 需要Configuration和Executor对象的参与
	@enduml
	```

### Mybatis获取MapperProxy
1. 使用SqlSession的getMapper方法，最终是调用MapperProxyFactory的newInstance方式完成MapperProxy的创建；
2. 具体过程如下图：
{% asset_img source03.png %}

3. 若看不清图，请看planUML源码
	```
	@startuml
	scale 2/3
	autonumber
	title MapperProxy初始化过程

	DefaultSqlSession->Configuration: getMapper(type)
	Configuration->MapperRegistry: getMapper(type, sqlSession)
	MapperRegistry->MapperRegistry: getMapper(type, sqlSession)
	MapperRegistry->MapperProxyFactory: 根据接口类型创建MapperProxyFactory
	MapperProxyFactory->MapperProxy: newInstance(sqlSession)
	MapperProxy->MapperProxy: new MapperProxy<>(sqlSession, mapperInterface, methodCache)
	MapperProxy-->DefaultSqlSession: 返回MapperProxy

	center footer 需要Configuration和DefaultSqlSession的参与
	@enduml
	```

### Mybatis查询流程
1. 执行MapperProxyinvoke方法
2. 执行DefaultSqlSession的selectList方法
3. 执行Executor的quary方法
4. 通过ParameterHandler处理输入参数，最终调用的TypeHandler的setParameter方法
5. 通过StatmentHandler创建Statment，执行execute方法
6. 通过ResultSetHandler处理返回结果，最终调用TypeHandler的getParameter方法
7. 给出具体流程图：
{% asset_img source04.png %}

8. 若看不清图，请看planUML源码
	```
	@startuml
	scale 2/3
	autonumber
	title 根据主键查询过程

	MapperProxy->MapperMethod: invoke(proxy, method, args)
	MapperMethod->MapperMethod: execute(sqlSession, args)
	MapperMethod->MapperMethod: sqlCommand.getType()== SELECT
	MapperMethod->MapperMethod: 根据返回类型做不同处理（这里返回单个对象）
	MapperMethod->MapperMethod: convertArgsToSqlCommandParam(args)入参包装
	MapperMethod->DefaultSqlSession: sqlSession.selectOne(command.getName(), param)
	DefaultSqlSession->DefaultSqlSession: selectOne(statement, parameter)
	DefaultSqlSession->DefaultSqlSession: selectList(statement, parameter)，返回list.get(0)
	DefaultSqlSession->DefaultSqlSession: selectList(statement, parameter, rowBounds)
	DefaultSqlSession->Configuration: 获取MappedStatement
	Configuration->Configuration: getMappedStatement(id, true)
	Configuration-->DefaultSqlSession: mappedStatements.get(id)
	DefaultSqlSession->CachingExecutor: executor.query(ms, parameter, rowBounds, resultHandler)
	CachingExecutor->CachingExecutor: query(ms, parameterObject, rowBounds, resultHandler)
	CachingExecutor->CachingExecutor: 获取BoundSql,创建缓存key
	CachingExecutor->CachingExecutor: query(ms, parameterObject, rowBounds, resultHandler, key, boundSql)
	CachingExecutor->BaseExecutor: executor.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql)
	BaseExecutor->BaseExecutor: query(ms, parameter, rowBounds, resultHandler, key, boundSql)
	BaseExecutor->BaseExecutor: localCache.getObject(key)
	BaseExecutor->BaseExecutor: resultHandler为null偶从localCache中取
	BaseExecutor->BaseExecutor: 否则，queryFromDatabase(ms, parameter, rowBounds, resultHandler, key, boundSql)
	BaseExecutor->SimpleExecutor: baseExcutor.doQuery(ms, parameter, rowBounds, resultHandler, boundSql)
	SimpleExecutor->SimpleExecutor: doQuery(ms, parameter, rowBounds, resultHandler, boundSql)
	SimpleExecutor->Configuration: ms.getConfiguration()
	Configuration->RoutingStatementHandler: newStatementHandler(wrapper, ms, parameter, rowBounds, resultHandler, boundSql)
	RoutingStatementHandler->RoutingStatementHandler: new RoutingStatementHandler(executor, mappedStatement, parameterObject, rowBounds, resultHandler, boundSql)
	RoutingStatementHandler->PreparedStatementHandler: ms.getStatementType()根据类型判断获取StatementHandler
	PreparedStatementHandler->PreparedStatementHandler: new PreparedStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql)
	PreparedStatementHandler->PreparedStatementHandler: 在构建PreparedStatementHandler过程中还创建了ParameterHandler和ResultSetHandler
	PreparedStatementHandler-->SimpleExecutor: 返回PreparedStatementHandler
	SimpleExecutor-->SimpleExecutor: prepareStatement(handler, statementLog)
	SimpleExecutor-->SimpleExecutor: PreparedStatementHandler创建了PrepareStatement
	SimpleExecutor-->SimpleExecutor: DefaultParameterHandler处理sql的输入参数
	SimpleExecutor-->SimpleExecutor: 最终使用TypeHandler的setParameter(ps, i, parameter, jdbcType)
	SimpleExecutor-->PreparedStatementHandler: statmentHandler.query(stmt, resultHandler)
	PreparedStatementHandler-->PreparedStatementHandler: query(statement, resultHandler)
	PreparedStatementHandler-->PreparedStatementHandler: 通过PreparedStatement执行ps.execute()
	PreparedStatementHandler-->PreparedStatementHandler: 通过DefaultResultSetHandler出来结果集
	PreparedStatementHandler-->PreparedStatementHandler: 最终使用TypeHandler的getResult(rs, column)

	center footer 需要MapperedStatment、Executor、Statmenthandler、ParameterHandler、ResultSetHandler、TypeHandler参与
	@enduml
	```


