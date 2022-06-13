```java
// 静态内部类实现的单例模式
public class Singleton {
	private Singleton() {}
	
	private static class SingletonHolder{
		public static final Singleton instance = new Singleton();
	}
	
	public static Singleton getInstance() {
		return SingletonHodler.instance;
	}
}
/**
- 静态变量是在类加载的时候初始化的，而类是在用到它的时候才加载的
- 外部类的加载并不会导致内部类的加载
- 类的加载过程是线程安全的
**/
```

上面代码实现的单例模式 即支持**延迟加载**， 也是**线程安全**的

先说明下final 

​	final 可以修饰类、方法、变量

​	final 修饰的类 不可以被继承

​	final 修饰的方法 不可以被子类重写，（早期jvm将final修饰的方法判定为内联函数， 编译器会将该函数的代码直接插入到调用处，以减少函数的调用，优化性能）。 
子类可以将父类中的非final方法重写为final方法

​	final 修饰的变量只能被赋值一次，之后就不能再修改了
**final修饰的成员变量**有两种赋值方式，一种是在变量声明时，另一种是在构造函数中（毕竟构造函数只会被调用一次，所以对象一旦被创建，final成员变量变不会在被修改

```java
// 方法一
public class Demo_1 {
	private final int fl = 8;
}

// 方法二
public class Demo_2 {
	private final int fl;
	
	public Demo_2 () {
		this.fl = 8;
	}
}
```
**final 修饰的局部变量**，赋值方式也有两种，一种是声明时赋值，另一种是使用前赋值一次，之后就不能再赋值了

```java
// 方法一
public double caculateArea(double r) {
	final double pi = 3.14; //声明时赋值
	double area = pi * r * r;
	return area;
}
// 方法二
public double caculateArea(double r) {
	final double pi;
	pi = 3.14; //使用前赋值
	double area = pi * r * r;
	return area;
}
```

**final 修饰的方法参数** 不可重新赋值，引用类型变量可以修改内部成员变量，但不可重新赋值新的对象



### 如何设计一个不可变类

1. **将类设置为final类**，否则通过继承可以通过子类改变该类的数据
2. **将类中所有属性都设置为final**，在创建对象时设置，之后不再允许修改
3. **通过方法返回的属性，如果是引用类型的，需要返回属性的副本**



static 可以修饰变量、方法、代码块、嵌套类

- static 变量

static 只能修饰成员变量（final能修饰成员变量、局部变量、方法参数）
static修饰的成员变量也叫静态变量，可以通过类来访问，也可以通过对象来访问
```java
public class Obj {
	public static int objCnt = 0;
	
	public Obj() {
		objCnt++;
	}
}

public class Demo_3 {
	public static void main(Stirng[] args) {
		Obj d1 = new Obj();
		Obj d2 = new Obj();
		System.out.println(Obj.objCnt); // 2
		System.out.println(d1.objCnt); // 2
	}
}
```

static final 修饰的变量 我们叫 **静态常量**


- static 方法


- static 代码块
静态代码块在类加载时执行， 如果类中有多个静态代码块，执行顺序和书写顺序一致
作用：如果某些静态成员变量的初始化操作无法通过一个简单的赋值语句来完成，就可以把初始化逻辑放到静态代码块中


- static 嵌套类
static只能修饰嵌套类，嵌套类也叫内部类，内部类有三种：普通内部类、静态内部类、匿名内部类

普通内部类
private 修饰的普通内部类，在外部类之外是不可见的！ 
如果想在外部类之外的代码中使用内部类，要么使用public修饰内部类，要么让内部类实现一个外部接口，外部代码使用接口来访问内部类代码
```java
public interface I {}

public class A {
	private class B{}
	private class C implements I {}
	public class D {}
	
	public B getB() {
		return new B();
	}
	
	public I getC() {
		return new C();
	}
	
	public D getD() {
		return new D();
	}
}

public class Demo {
	public static void main(String[] args) {
		A a = new A();
		A.B b = a.getB(); // 编译报错
		I c = a.getC();
		A.D d1 = a.getD();
		A.D d2 = a.new D();
	}
}
```

- 静态内部类
	与普通内部类的区别
	- 访问权限上
	- 静态
	- 如果要创建普通内部类的对象，需要先创建外部类的对象，而静态内部类对象可以独立于外部类单独创建
``` java
public class A {
	public class D {}
	public static class E {}
}

public class Demo {
	public static void main(String[] args) {
		A a = new A();
		A.D d = a.new D();
		
		A.E e = new A.E();
	}
}
```

三个关键知识：
- 静态变量是在类加载的时候初始化的，而类是在用到它的时候才加载的
- 外部类的加载并不会导致内部类的加载
- 类的加载过程是线程安全的





