Connect 一次连接
Statement  sql语句
//注册mysql驱动   （加载驱动）
Class.forname("com.mysql.jdbc.Driver");

**parameterMap** 已被废弃
**resultMap** 用于手动映射Java对象与数据库对象

<sql id=""></sql>
**sql** 标签只有一个id属性 可以任意引用，表示一个sql片段
引用用法：<include refid="sqlid"></include>



**sqlSessionFactoryBuilder** 读取配置文件 创建sqlSessionFactory之后 会被回收掉
**sqlSessionFactory** 整个程序的生命周期 维护连接池

**sqlSession** 表示一次连接 线程不安全的 方法级别的生命周期
**sqlMapper** 方法级别的生命周期



