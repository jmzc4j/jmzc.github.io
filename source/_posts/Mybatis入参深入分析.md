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
//第一次对入参进行处理
org.apache.ibatis.reflection.ParamNameResolver{
	private final SortedMap<Integer, String> names; //初次用来保存入参的Map
	 
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
	- 在Executor（默认是SimpleExecutor）执行方法时再一次对入参进行了包装,即调用DefaultSqlSession类wrapCollection(parameter)方法
##### 第二次参数处理部分源码
- - 下面对第一次入参处理进行源码分析：
```
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