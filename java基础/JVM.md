### 方法区

### -XX:MetaspaceSize  -XX:MaxMetaspaceSize

-	MetaspaceSize 永久代的大小（超过这个值就发生fullGC，要避免fullGC）
-	MaxMetaspaceSize 

```sh
jstat -gcutil pid #查看使用率M的值 （M = MU/MC）MU：使用量。MC：容量
jstat -gc pid 2s 3 #查看MC， MU

#查看JVM参数默认值
java -XX:+PrintFlagsFinal -version |grep MetaspaceSize
```

**建议MetaspaceSize和MaxMetaspaceSize设置一样大；**

class常量池（静态常量池）--- 存放符号引用（class， 方法等信息）

运行时常量池 --- 虚拟机规范中的概念 --- 存放直接引用（jdk1.7以后，实现在堆中）

字符串常量池 --- jvm为了高效使用string，而划出的一块区域
String
private final char value[];
说明只要创建了 就不可被修改！！！
why？
1.安全
2.hash唯一， hashMap - key
3.字符串常量池





JVM对象的访问方式：

1. 句柄
2. 直接指针


判断对象的存活
	手动释放内存容易出现的问题：
    1. 忘记回收 （内存泄漏）
    2. 多次回收会造成什么问题？
        《多次回收会释放掉 已经被别的线程占用的内存空间》
        
	什么是垃圾！！！
		没有任何引用指向的一个对象 或者多个对象（循环引用）
	如何判断？
		**引用计数法**：在对象中添加一个引用计数器，每当有一个地方引用它，计数器+1，当引用失效，计数器-1； （python在用引用计数法，但是解决不了“循环引用”的问题）
		**可达性分析**：（根可达GC roots）
			1. 虚拟机栈（栈帧中的本地变量表）中引用的对象
			2. 方法区中类静态属性引用的对象
			3. 方法区中常量引用的对象
			4. 本地方法栈中JNI（一般是native方法）引用的对象
			5. JVM 的内部引用（class 对象、异常对象 NullPointException、OutofMemoryError，系统类加载器）。（非重点）
			6. 所有被同步锁(synchronized 关键)持有的对象。（非重点）
			7. JVM 内部的 JMXBean、JVMTI 中注册的回调、本地代码缓存等（非重点）
			8. JVM 实现中的“临时性”对象，跨代引用的对象（在使用分代模型回收只回收部分代的对象，这个后续会细讲，先大致了解概念）（非重点）


###各种引用
	强引用 
		Object obj = new Object(),垃圾回收器永远不会回收这类对象
	
	软引用
		内存不足时 会被回收
		SoftReference<User> userSoft = new SoftReference<User>(u);
		应用：缓存
		
	弱引用
		WeakReference 只要发生了GC，就会被回收
		应用：缓存
		实际运用（WeakHashMap、ThreadLocal）
		
	虚引用
		PhantomReference 随时会被回收掉
		垃圾回收的时候收到一个通知，就是为了监控垃圾回收器是否正常工作。


### 对象的分配策略（
	所有的对象 优先分配在eden区
	JVM优化 栈上分配（逃逸分析，发现对象出不去，外面调不到，就会直接在栈中创建）
	大对象直接放入老年代（大小可设置）-XX:PretenureSizeThreshold=4m
	长期存活的对象放入老年代（主流jvm 15次gc后进入老年代）
	动态对象年龄判定
	空间分配担保
	


		







