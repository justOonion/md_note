### FactoryBean 和 BeanFactory

FactoryBean是一个工厂Bean，可以生成某一个类型的Bean实例，它的最大的一个作用就是：自定义Bean的创建。
BeanFactory是Spring容器中基本类，它负责创建和管理容器中的Bean。
```java
public interface FactoryBean<T> {

    //返回的对象实例
    T getObject() throws Exception;
    //Bean的类型
    Class<?> getObjectType();
    //true是单例，false是非单例  在Spring5.0中此方法利用了JDK1.8的新特性变成了default方法，返回true
    boolean isSingleton();
}
```
FactoryBean中定义了一个Spring Bean的很重要呢的三个特性：是否单例、Bean类型、Bean实例。

### spring 注解的唯一缺点就是侵入性变强！

spring容器只认识 BeanDefinition，所以不管是扫描注解 还是解析xml，最终都是将信息读取到BeanDefiniton中，传给spring容器，让spring容器完成bean的加载。
spring将类实例化只有一种途径：就是将类信息包装成beanDefiniton对象！



### 循环依赖问题
**A，B循环依赖的实例化过程**

		实例化A [getBean(A-beanName)]
			（先从缓存中获取，获取不到往下走，否则返回）
		在 beforeSingletonCreation 中
		将 A-beanName 加入到 singletonsCurrentlyInCreation 容器中
	
		触发A的无参构造函数，
		收集A上的注解信息
		A-》三级缓存中
	
		A-IOC依赖注入，
	
				B是A的Autowired属性，触发B的getBean操作，
				将B的beanName加入到 singletonsCurrentlyInCretion 容器中
				触发B的无参构造函数，
				收集B上的注解信息
				B-》三级缓存中
			
				B-IOC依赖注入，
					因为B是A的Autowired属性，
					触发A-getBean操作-从三级缓存中取并ObjectFactory对象 执行 getObject()操作
					返回 A的bean实例  并将A的bean实例加入 二级缓存中
					完成B的实例化，删除singletonsCurrentlyInCreation容器中 B-beanName，将B实例加入到一级缓存中
			
		返回B 实例，完成A的实例化,删除singletonsCurrentlyInCreation容器中 A-beanName，将A加入到一级缓存中
	
		下次实例化A、B的时候，直接从一级缓存中取就可以了




