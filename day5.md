# day5

​																																														时间：2022年4月10日22:22:13

## 目录

- SpringMvc中如何解决post请求中文乱码问题，get又如何处理呢
- 简单谈一下SpringMvc的工作流程
- Mybatis中表的字段名和类中的属性名不一样怎么办

## 一、SpringMvc中如何解决post请求中文乱码问题，get又如何处理呢

### 1.post

<img src="D:\桌面\面试题\pic\day5\1649601092769.png" alt="1649601092769" style="zoom: 67%;" />

#### 解决（在web.xml中配置过滤器）

<img src="D:\桌面\面试题\pic\day5\1649601211408.png" alt="1649601211408" style="zoom:67%;" />

<img src="D:\桌面\面试题\pic\day5\1649601353205.png" alt="1649601353205" style="zoom:67%;" />



### 2.get(在server.xml的Connector标签中添加URIEncoding="utf-8")

<img src="D:\桌面\面试题\pic\day5\1649601177027.png" alt="1649601177027" style="zoom:67%;" />



## 二、简单谈一下SpringMvc的工作流程

### 1.处理方式一：设置方法返回值是ModelAndView



### 2.处理方式二：形参中添加Map对象，返回值是String

```
最终还是会转换成ModelAndView
```

### 3.流程

<img src="D:\桌面\面试题\pic\day5\1649601796843.png" alt="1649601796843" style="zoom:67%;" />

## 三、Mybatis中表的字段名和类中的属性名不一样怎么办

## 1.起别名

```sql
select id Id, name sname from student
```

## 2.修改Mybatis的核心配置文件

```xml
<settings>
	<setting name="mapUderscoreToCamelCase" value="true"/>
</settings>
```

以上就可以将下划线命名的字段名映射成类的驼峰命名的属性名

## 3.在核心配置文件中使用resultMap

```xml
<resultMap type="返回值类型" id="map的键">
	<id column="表中主键名" property="类中主键名"/>
    <result column="表中其他字段" property="类中其他字段"/>
    ...
</resultMap>
<select id="getObj" resultMap="map的键">
	select * from user;
</select>
```

