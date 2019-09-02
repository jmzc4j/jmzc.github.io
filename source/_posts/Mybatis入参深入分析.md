---
title: Mybatis入参深入分析
date: 2019-09-02 00:18:11
tags: mybatis
categories: 学习笔记
---

### Mybatis参数处理过程解析
#### 第一次参数处理
- 我们知道在Mybatis通过MapperProxy执行Mapper接口的方法时，
	- 实际调用的是MapperMethod类的execute方法，该方法中不同的sql命令执行不同的CRUD方法；
	- 但是每次执行CRUD方法时都会先进行一次入参的处理，即convertArgsToSqlCommandParam(args)的调用；
	- 而这个参数处理方法调用的是ParamNameResolver类的getNamedParams(args)方法；
##### 第一次参数处理部分源码
- 下面对第一次入参处理进行源码分析：  
```
//private final SortedMap<Integer, String> names; 用来保存入参的Map
//构造方法中初始化了names，截取部分代码解释
public ParamNameResolver(Configuration config, Method method) {
	final SortedMap<Integer, String> map = new TreeMap<>();
	String name = null;

	//如果使用了@Param注解，那么最终name的值就是该注解的value值
	if (annotation instanceof Param) {
	  hasParamAnnotation = true;
	  name = ((Param) annotation).value();
	  break;
	}
	//没有使用@Param注解，那么就会去Configuration中取看看有没有开启useActualParamName这个开关
	if (name == null) {
	  //这个开关对应着mybatis-config.xml中settings标签中的配置
	  //如果发现此开关启用，那么就使用Mapper接口方法的形参名称为name赋值
	  if (config.isUseActualParamName()) {
		name = getActualParamName(method, paramIndex);
	  }
	  //如果既没有使用@param注解，也没有开启useActualParamName开关
	  //那么久使用map集合的元素数目作为name的值
	  if (name == null) {
		name = String.valueOf(map.size());
	  }
	}
	//将name的值进行保存,此时
	//key的取值为0 1 2 ...
	//value的取值可能是:
	//	@Param注解的值
	//  形参本身的值
	//  形参在形参列表中的索引值（即0 1 2 ...）
	map.put(paramIndex, name); 、、

	//最终完成了成员变量names的初始化
	names = Collections.unmodifiableSortedMap(map); 
}
//第一次参数处理
public Object getNamedParams(Object[] args) {
	final int paramCount = names.size();
	//这里，看出传入的参数是null或者根本没有传参参数时
	//始终返回null
	if (args == null || paramCount == 0) {
	  return null;
	//这里，看出，没有使用@Param注解并且形参的个数只有一个时
	//就会直接从names集合中取出，此时与具体的参数名称是什么根本没有关系
	} else if (!hasParamAnnotation && paramCount == 1) {
	  return args[names.firstKey()];
	//除了上面的2种情况，都会将形参列表封装为一个Map，并且是以形参的为值的一个Map
	//但是对于同一个值可能会有1个或者2个键来指向
	} else {
	  final Map<String, Object> param = new ParamMap<>();
	  int i = 0;
	  for (Map.Entry<Integer, String> entry : names.entrySet()) {
		//以names集合的value值作为key，也就是说，这个key可能是
		//@Param的值，其次是形参本身，也可能是形参的位置索引，只能是这三种情况的一种
		param.put(entry.getValue(), args[entry.getKey()]);
		final String genericParamName = GENERIC_NAME_PREFIX + String.valueOf(i + 1);
		if (!names.containsValue(genericParamName)) {
		  //这里是再次为参数值定义新key
		  //当Map中不存在这种key时才出现这种定义，这个key就是param1 param2 ...
		  param.put(genericParamName, args[entry.getKey()]);
		}
		i++;
	  }
	  return param;
	}
}
```

##### 第一次参数处理结果总结
1. 没有参数，或者参数为null时，返回null；
2. 有且只有一个参数时，并且没有使用Param注解，返回names中的惟一值，此时其实与参数名是什么就没有关系；
3. 当使用Param注解且只有一个参数，或者无论是否使用Param注解但是有多个参数时，都将封装为一个Map，最后返回此Map；
	- 此时key的值存在以下情况：
	1. 使用注解后Param注解的指定的值
	2. 开启useActualParamName开关后形参本身的值
	3. 1和2情况不成立，就使用形参在参数列表的位置索引（即0、1、2...）
	4. 不论如何，只要Map中不存在param1、param2、param3... 这样的key，那么就可以使用这种标记作为key；
- 经过这次处理，我们得出：继续向下传递的参数可能是：
	基本类型，基本类型包装类，String，POJO，POJO的包装类型、List、Collection、Array、Map等，最终传递的都是一个Object类型对象；
	
#### 第二次参数处理
- 我们知道MapperMethod类的execute方法实际调用DefaultSqlSession的增删改查方法：
	- 对于select，无论是selectOne、selectMap还是selectList方法，最终调用的都是SqlSession的selectList方法；
	- 对于insert、delete、update，最终调用的时SqlSession的update方法；
	- 而执行selectList或者update方法实际执行的Executor实现类的query或者update方法；
	- 在Executor执行方法时再一次对入参进行了包装,即调用DefaultSqlSession类wrapCollection(parameter)方法
##### 第二次参数处理部分源码
- 下面对第一次入参处理进行源码分析：      
```
//第二次参数处理
private Object wrapCollection(final Object object) { // 这里object就代表要传入的参数
	//如果传入的是Collection类型,那么就封装到一个Map中
	if (object instanceof Collection) {
	  StrictMap<Object> map = new StrictMap<>();
	 //当是Collection或者Set类型时，key为collection
	  map.put("collection", object);
	  //当是List类型时，key就是list
	  if (object instanceof List) {
		map.put("list", object);
	  }
	  return map;
	  //当传入的是非null并且是Array类型是，也会封装到Map中
	} else if (object != null && object.getClass().isArray()) {
	  StrictMap<Object> map = new StrictMap<>();
	  //此时的key就是array
	  map.put("array", object);
	  return map;
	}
	//其他情况不做处理
	return object;
}
```
  
##### 第二次参数处理结果总结
1. 没有参数，或者参数为null，返回null；
2. 有参数且参数为Map、Pojo、原始类型等不做处理，直接返回；
3. 有参数但参数是Collection、List、Array类型时封装为一个Map，然后返回Map；
- 经过这次处理后，参数类型只剩下基本类型及其包装类、String、pojo及其包装类、Map等（干掉了第一次中Collection和Array）；

#### 第三次参数处理
- 上面提到查询执行到Executor时，执行的是执行器的query方法；
- 而这个方法中第一条语句就是BoundSql boundSql = ms.getBoundSql(parameterObject);
- MapperedStatment的这个方法实际又是调用sqlSource.getBoundSql(parameterObject);
- SqlSource就是用来将xml中的sql脚本转变成一个真正JDBC可用的sql的对象；
- SqlSource在XmlMapper解析过程中就已经通过XMLScriptBuilder类的parseScriptNode方法创建了；（通常情况创建的都是DynamicSqlSource）
- DynamicSqlSource类的getBoundSql源码分析：  
```
public BoundSql getBoundSql(Object parameterObject) {
	//创建了一个代表动态环境的上下文，在这个构造中对输入参数进行了第三次处理
	DynamicContext context = new DynamicContext(configuration, parameterObject);
	rootSqlNode.apply(context);
	SqlSourceBuilder sqlSourceParser = new SqlSourceBuilder(configuration);
	Class<?> parameterType = parameterObject == null ? Object.class : parameterObject.getClass();
	SqlSource sqlSource = sqlSourceParser.parse(context.getSql(), parameterType, context.getBindings());
	BoundSql boundSql = sqlSource.getBoundSql(parameterObject);
	context.getBindings().forEach(boundSql::setAdditionalParameter);
	return boundSql;
}

//DynamicContext类中部分属性如下：
//public static final String PARAMETER_OBJECT_KEY = "_parameter"; 内置参数之一
//public static final String DATABASE_ID_KEY = "_databaseId"; 内置参数之二
// private final ContextMap bindings; 一个严格的HashMap

//关键的静态代码块：OGNL处理的基础
//将参数保存到ContextMap中，然后在OGNL环境中进行了注册
static {
	OgnlRuntime.setPropertyAccessor(ContextMap.class, new ContextAccessor());
}

//构造，也是对输入参数的再处理 
public DynamicContext(Configuration configuration, Object parameterObject) {
	//判断：如果不是null也不是Map类型，就对参数进行包装（使用MetaObject包装器）
	if (parameterObject != null && !(parameterObject instanceof Map)) {
	  MetaObject metaObject = configuration.newMetaObject(parameterObject);
	  //断定参数类型是否在TypeHandlerRegistry中进行了注册
	  boolean existsTypeHandler = configuration.getTypeHandlerRegistry().hasTypeHandler(parameterObject.getClass());
	  //将包装后的对象赋值给了ContextMap的属性
	  //注册过值为true，否则为false
	  bindings = new ContextMap(metaObject, existsTypeHandler);
	} else {
	  //对于null或者Map类型没做特殊处理
	  bindings = new ContextMap(null, false);
	}
	//为参数设置一个统一的key，可以在动态sql中直接使用
	bindings.put(PARAMETER_OBJECT_KEY, parameterObject);
	//为databaseId设置一个统一的key，可以在动态sql中直接使用
	bindings.put(DATABASE_ID_KEY, configuration.getDatabaseId());
}

//上面这一步做完，那么如果是通过bindings取值：
@Override
public Object get(Object key) {
  //先从Map中取，Map类型也没问题
  String strKey = (String) key;
  if (super.containsKey(strKey)) {
	return super.get(strKey);
  }
	
  //当参数是null时返回null
  if (parameterMetaObject == null) {
	return null;
  }

  //当存在该类型的类型处理器，但是没有被MetaObject包装，那就是原始类型，直接返回
  if (fallbackParameterObject && !parameterMetaObject.hasGetter(strKey)) {
	return parameterMetaObject.getOriginalObject();
  } else {
	// issue #61 do not modify the context when reading
	return parameterMetaObject.getValue(strKey);
  }
}

//parameterMetaObject.getValue(strKey)
//PropertyTokenizer属性解析器，去拆解链式中的'.'或'['
//不断递归，直到没有这样的符号，然后最终使用objectWrapper.get(prop)取得属性值；
public Object getValue(String name) {
	PropertyTokenizer prop = new PropertyTokenizer(name);
	if (prop.hasNext()) {
	  MetaObject metaValue = metaObjectForProperty(prop.getIndexedName());
	  if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
		return null;
	  } else {
		return metaValue.getValue(prop.getChildren());
	  }
	} else {
	  return objectWrapper.get(prop);
	}
}
```

##### 第三次参数处理结果
1. 非null非Map类型，经过MetaObject包装后再次封装到ContextMap中；
2. null或者Map类型没做特殊处理；
3. 为所有的参数设置了一个统一的key，就是_parameter；
- 注意：非动态sql应该没有上述处理
- 但是，无论如何都会经过MetaObject处理；
```
private MetaObject(Object object, ObjectFactory objectFactory, ObjectWrapperFactory objectWrapperFactory, ReflectorFactory reflectorFactory) {
	this.originalObject = object;
	this.objectFactory = objectFactory;
	this.objectWrapperFactory = objectWrapperFactory;
	this.reflectorFactory = reflectorFactory;

	if (object instanceof ObjectWrapper) {
	  this.objectWrapper = (ObjectWrapper) object;
	} else if (objectWrapperFactory.hasWrapperFor(object)) {
	  this.objectWrapper = objectWrapperFactory.getWrapperFor(this, object);
	} else if (object instanceof Map) {
	  this.objectWrapper = new MapWrapper(this, (Map) object);
	} else if (object instanceof Collection) {
	  this.objectWrapper = new CollectionWrapper(this, (Collection) object);
	} else {
	  this.objectWrapper = new BeanWrapper(this, object);
	}
}
```

#### 创建SqlSource的过程（Sql解析过程）
- 无论是创建哪一种SqlSource都是通过SqlSourceBuilder的parse方法实现的；
- 下面给出SqlSourceBuilder的parse方法的源码分析：     
```
public SqlSource parse(String originalSql, Class<?> parameterType, Map<String, Object> additionalParameters) {
	//1这就是一个符号处理器，用来初始化以下属性
	//本类2个：Class<?> parameterType;MetaObject metaParameters;
	//超类3个：Configuration configuration;TypeAliasRegistry typeAliasRegistry;TypeHandlerRegistry typeHandlerRegistry;
	ParameterMappingTokenHandler handler = new ParameterMappingTokenHandler(configuration, parameterType, additionalParameters);
	//2这是用来解析SQL的解析器，专门用来解析#{}这种参数表达式的
	GenericTokenParser parser = new GenericTokenParser("#{", "}", handler);
	//3重新拼装sql，返回一个带有占位符的可以通过JDBC向数据库发出的sql；
	String sql = parser.parse(originalSql);
	//最终返回的都是一个StaticSqlSource;
	return new StaticSqlSource(configuration, sql, handler.getParameterMappings());
}
```

- 上面第3步解析sql脚本的过程中，对于#{}的表达式进行了解析，
- 调用的是ParameterMappingTokenHandler类的handleToken方法，
- 下面给出源码分析：
```
public String handleToken(String content) {
	  //这里parameterMappings就是一个ArrayList<ParameterMapping>;
	  //就是向List中添加ParameterMapping
	  //ParameterMapping代表了一个封装了#{}中所相关的属性的对象（如，要传递的property、jdbcType、javaType、mode、typeHandler等）
      parameterMappings.add(buildParameterMapping(content));
	  
	  //参数解析完成后使用占位符替代OGNL
      return "?";
}
//详细代码请自行参看源码
private ParameterMapping buildParameterMapping(String content) {
	//先取出表达式的内容进行封装成Map
	Map<String, String> propertiesMap = parseParameterMapping(content);
	String property = propertiesMap.get("property");
	Class<?> propertyType;
	...
	//省略部分分析：
	//1. 判断MetaObject中是否有对该类型进行了包装，包装过则取出对应类型；
	//2. 判断该类型参数是否在TypeHandlerRegistry中进行了注册，存在则得到其类型；
	//3. 判断是JdbcType否是游标,是游标则取ResultSet类型；
	//4. 判断参数为null或者是Map兼容类型时，取Object类型；
	//5. 到反射该类型所有成员的Map中取检索，存在就获取对应类型，没有依然是Object类型；
	//拿到具体类型后，构建下面的静态内部类对象
	ParameterMapping.Builder builder = new ParameterMapping.Builder(configuration, property, propertyType);
	Class<?> javaType = propertyType;
	String typeHandlerAlias = null;
	//遍历集合向builder中的属性赋值
	for (Map.Entry<String, String> entry : propertiesMap.entrySet()) {
	String name = entry.getKey();
	String value = entry.getValue();
	...
	}
	//存在别名，获取别名真实类型
	if (typeHandlerAlias != null) {
	builder.typeHandler(resolveTypeHandler(javaType, typeHandlerAlias));
	}
	//验证后返回一个ParameterMapping对象，默认情况mode=in
	return builder.build();
} 
```

#### 再次简单回顾Myabtis的查询流程
1. 从MapperProxy开始，调用invoke方法
2. 执行MapperMethod的execute方法
3. 执行DefaultSqlSession的selectList方法
4. 执行SimpleExecutor的query方法，
5. 在4的执行过程中，通过Configuration创建PreparedStatementHandler、DefaultParameterHandler和DefaultResultSetHandler
6. PreparedStatementHandler创建PreparedStatment,并调用DefaultParameterHandler来设置参数
7. 执行PreparedStatementHandler的query方法
8. 最终执行的是PreparedStatement的execute方法,通过DefaultResultSetHandler处理返回结果
9. 在设置参数和封装结果集的过程中都会有TypeHandler和MetaObject类参与其中