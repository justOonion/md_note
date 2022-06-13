### 安装

[点击进入 gradle 下载页](https://services.gradle.org/distributions/)

```shell
# 安装目录
mkdir /opt/gradle

# 解压缩
unzip -d /opt/gradle grade-5.6-bin.zip

# 编辑/etc/profile  设置环境变量
export GRADLE_HOME=/opt/gradle/gradle-5.6
export PATH=$GRADLE_HOME/bin:$PATH

# 刷新配置文件
source /etc/profile

# 查看gradle 版本
gradle -version
```

###Gradle
build.gradle文件常用任务解析
```apply plugin: 'war'``` 指定web项目，项目编译（在项目提示符下执行：```gradle build```）时生成项目的war包
```apply plugin：'java'```  生成jar包

**Grovvy:**

```sh
#普通变量
def val = '123'
Pringtln val

#数组
def list = ["a", "b"]
list << "c"
println list.get(2) // c

#map
def map = ["key1": "val1", "key2": "val2"]
map.key3 = "val3"
println map.get("key3") // val3

#闭包演示
def b1 = {
    println "hello b1"
}
def method1(Closure closure) {
    closure()
}
method1(b1) // hello b1

#带参数的闭包
def b2 {
    v -> println "hello $(v)"
}
def method2(Closure closure) {
    closure("xiaoma")
}
method2(b2) // hello xiaoma
```

配置GRADLE_USER_HOME 环境变量 指定 maven 仓库



