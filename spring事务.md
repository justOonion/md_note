### spring 体系太大 但是模块清晰

学习源码的作用？
面试，工作效率， 降低bug

1.事务注解的实现
		框架--目的--提供JDK没有的功能？❎ --封装功能实现， 提高开发效率？✅
2.spring设计是如何提供多种框架集成的
3.事务坑
4.spring学习指南

```java
@Autowired
DataSourde dataSource；
//数据库连接
Connection connection = dataSource.getConnection();
connection.setAutoCommit(false);

Statement statement = connection.createStatement();
try {
		String sql1 = "insert ...";
		String sql2 = "delete ...";
		statement.execute(sql1);
		statement.execute(sql2);
		
		connection.commit();
} catch (Exception e) {
 		connection.rollback();
} finally {
  	connection.close();
}


//通过AopContext.currentProxy()方法获取代理  可以避过事务的坑
    @Override
    public void openWithMultiTx(String aname, double money) {
((IStockProcessService)AopContext.currentProxy()).openAccount(aname,money);

((IStockProcessService)AopContext.currentProxy()).openStockInAnotherDb(aname, 11);
    }


```
数据库是如何区别哪些提交的sql是属于同一个事物的 是通过连接

真正让注解产生作用。并不是注解本身
AOP才是赋予注解灵魂的背后角色

--事物注解-- 本身只是标记这段代码需要事务控制





