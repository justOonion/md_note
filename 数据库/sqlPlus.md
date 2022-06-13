### 安装
[下载](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html)

```sh
[root@node1 ~]# ls
oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm  
oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm    
oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm 
```

安装

```sh
[root@node1 ~]# rpm -ivh oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm 
Preparing...                          ################################# [100%]
Updating / installing...
   1:oracle-instantclient11.2-basic-11################################# [100%]
[root@node1 ~]# rpm -ivh oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm 
Preparing...                          ################################# [100%]
Updating / installing...
   1:oracle-instantclient11.2-sqlplus-################################# [100%]
[root@node1 ~]# rpm -ivh oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm 
Preparing...                          ################################# [100%]
Updating / installing...
   1:oracle-instantclient11.2-devel-11################################# [100%]
[root@node1 ~]#
```

配置

```sh
[root@node1 ~]# vi ~/.bashrc
export  ORACLE_HOME=/usr/lib/oracle/11.2/client64
export  TNS_ADMIN=$ORACLE_HOME/network/admin
export  LD_LIBRARY_PATH=$ORACLE_HOME/lib 
export  PATH=$ORACLE_HOME/bin:$PATH
export  NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
```



### 连接运行

``` shell
sqlplus userName/passwd@//host:port/serviceName
```



### 执行sql语句
SQL> **@/root/bom.sql**

