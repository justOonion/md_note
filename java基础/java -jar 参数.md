### 方式一

-DpropName=propValue的形式携带，要放在-jar参数前面，亲测，放在它后面好像取不到值
``` sh
java -DfileName=JOURNAL_TREENODE_DATA-20190404174502.txt -DprocessType=1 -jar dataProcess.jar
```
System.getProperty("propName")用来取值

 

### 方式二

参数直接跟在命令后面，多个参数之间用空格隔开
```sh
java -jar dataProcess.jar JOURNAL_TREENODE_DATA-20190404174502.txt processType=1
```
这种方式参数就是jar包里主启动类中main方法的args参数，按顺序来

 

### 方式三

使用springboot的方式，--propName=propValue方式
```sh
java -jar dataProcess.jar --hdfsFileName=trx_20190407.txt --processType=2
```
可以使用spring的@value("${hdfsFilename}")取值
