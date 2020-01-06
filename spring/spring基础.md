@Configuration 代替 xml配置文件
一个spring程序 IOC容器， DI（依赖注入）
在@Configuration类文件中，定义的@Bean 类似 xml中定义<bean>节点

@ComponentScan 是注解在Configuration类上的



**IoC的主要职责**：业务对象的构建管理， 业务对象间的依赖绑定



### 统一错误处理
**@ControllerAdvice**  作用在类上 ，抛到controller层的异常都会被捕捉到
**@ExceptionHandler**(Exception.class) 作用在错误处理的方法上，指定的异常类型会被处理



### 动态代理
**aop为例**
在方法被调用时，根据**classPattern，methodPattern**判断方法是否被逻辑增强
为逻辑增强的方法动态生成代理类，

```java
Proxy.newProxyInstance(classLoader, interfaces, invacationHandler);
```
动态代理-只能代理接口，不能代理类

**代理对象是真正去执行动作的对象**
由于代理对象能否替被代理对象执行动作，所以两者应该**实现同样的接口！！！**
接口中的方法就是被代理的一些动作

？？？如何获取方法执行时间，但不能改变这个类的源码
继承，聚合，


