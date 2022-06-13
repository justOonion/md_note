### 基于POJO的 轻量级 和 最小侵入性编程
**对象的依赖关系** 由 系统中负责协调各对象的第三方组件（上下文 Application Context） 在创建对象的时候 进行设定

如果一个对象只通过接口（而不是具体实现或初始化过程）来表明依赖关系，
那么这种依赖就能够在对象本身毫不知情的情况下，用不同的具体实现进行替换

Spring上下文全权负责对象的创建和组装。Spring自带了多种应用上下文的实现，他们之间主要区别只是在于如何加载配置
ClassPathXmlApplicationContext ...



### 使用 ApplicationContext


### bean 的生命周期
- Spring 对bean进行实例化
- Spring 将值 和 bean的引用注入到 bean对应的属性中
- 如果bean实现了 BeanNameAware接口，Spring将bean的ID 传给setBeanName() 方法
- 如果bean实现了 BeanFactoryAware接口，Spring将调用setBeanFactory() 方法，将BeanFactory容器实例传入
- 如果bean实现了 ApplicationContextAware接口，Spring将调用 setApplicationContext() 方法，将bean所在的应用上下文的引用传入进来
- 如果bean实现了 BeanPostProcessor 接口，Spring将调用它们的 postProcessBefore Initialization() 方法
- 如果bean实现了 InitializingBean 接口，Spring将调用它们的 afterPropertiesSet() 方法，类似地，如果bean使用init-method 声明了初始化方法，该方法也会被调用
- 如果bean实现了 BeanPostProcessor 接口，Spring将调用它们的 postProcessAfterInitialization() 方法

- 此时，bean已经准备就绪，可以被应用程序使用了，它们将一直驻留在应用上下文中，直到该上下文被销毁
- 如果bean实现了 DisposableBean 接口，Spring将调用它的 destory() 接口方法。同样，如果bean使用destory_method声明了销毁方法，该方法也会被调用




### spring
Spring 提供了与其他第三方框架和类库的集成点，这样你就不需要自己编写这样的代码了


### 如何选择 ApplicationContext
我的建议是尽可能地使用**自动配置**的机制。显式配置越少越好。

当你必须要显式配置bean的时候（比如，有些源码不是由你来维护的，而当你需要为这些代码配置bean的时候），我推荐使用类型安全并且比XML更加强大的JavaConfig。

最后，只有当你想要使用便利的XML命名空间，并且在JavaConfig中没有同样的实现时，才应该使用XML。

@Component 注解表明该类作为组件类，并告知Spring要为这个类创建Bean。
但是组件扫描 默认是不启用的。 我们还需要显式配置一下 @ComponentScan， 如果不指定参数，Spring会自动扫描该配置类相同的包，以及这个包下所有的子包



方法上添加了@Bean注解，Spring将会拦截所有对它的调用，并确保直接返回该方法所有创建的bean，而不是每次都对其进行实际的调用



## 高级装配
### 环境 profile



在默认情况下，Spring应用上下文中所有Bean都是作为单例的形式创建的。也就是说，不管给定的一个bean被注入到其他bean多少次，每次所注入的都是同一个实例


在大多数情况下， 单例bean是很理想的方案。
但是Spring 还定义了其他几种作用域

- 单例： 在整个应用中，只创建一个实例
- 原型： 每次注入或者通过Spring应用上下文获取的时候， 都会创建一个新的Bean实例
- 会话： 在Web应用中， 为每个会话创建一个bean实例
- 请求： 在Web应用中，为每个请求创建一个bean实例


















