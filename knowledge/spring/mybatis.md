1. 根据xml创建sqlSessionFactory
2. sql映射文件，配置sql
3. 将sql映射文件注册在全局配置文件中
4. sqlSessionFactory.openSession()；获取sqlSession实例，一个sqlSession就是和数据库的一次会话
5. 执行查询

优化：

sqlSession.getMapper，使用接口去获取



xml参数

1. setting
   1. 





#是以预编译的形式

$是取出值拼装，有安全问题，用于分表操作



分步查询，可以延迟加载



动态sql，OGNL表达式

if test

where标签，去掉第一个多出来的and or

trim标签，加前缀后缀，前缀后缀覆盖



choose，when test

set，封装更新语句

foreach，in（集合），批量保存



内置参数：_parameter， _databaseId

bind,可以将OGNL表达式的值绑定到一个变量

include、sql标签，抽取可重用sql，





### 缓存

1. 一级缓存：本地缓存，与数据库同一次会话期间查询的数据会放在本地缓存
2. 二级缓存：全局缓存



### 工作原理

1. 获取sqlFactoryFactory对象
2. 获取sqlSession对象
3. 获取接口的代理对象，mapperProxy
4. 执行增删改查



![image-20220204212256209](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220204212256209.png)



PageHelper