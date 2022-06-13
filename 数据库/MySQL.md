```sh
# docker 启动mysql容器
# 
docker run --name mysql3308 -p 3308:3306 --privileged=true -ti -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=enjoy -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -v /home/mysql/docker-data/3308/conf:/etc/mysql/conf.d -v /home/mysql/docker-data/3308/data/:/var/lib/mysql -v /home/mysql/docker-data/3308/logs/:/var/log/mysql -d mysql:5.7
```

##### 注意my.cnf 配置文件格式问题

```shell
# 进入mysql
mysql -uroot -p123456 -h 192.168.0.81 -P 3307

# 新增一个 master 复制用户
GRANT REPLICATION SLAVE,FILE,REPLICATION CLIENT ON *.* TO 'repluser '@'%' IDENTIFIED BY '123456';
# 刷新权限
FLUSH PRIVILEGES;

# 在master上查看 bin-log文件 和 position
show master status;
```

```shell
change master to master_host='192.168.0.81',master_port=3307,master_user='repluser',master_password='123456',master_log_file='mysql-bin.000005',master_log_pos=154;

# 显示slave状态
show slave status \G
# 开启slave
start/stop slave;
# 查看 slave 状态， master中也一样
show processlist;
```



###主从半同步设置

##### 安装插件
1.主库 install plugin rpl_semi_sync_master soname 'semisync_master.so';
2.从库 install plugin rpl_semi_sync_slave soname 'semisync_slave.so';
可以主库从库把这两个插件都装上，因为会有主从切换的情景

##### 查看插件状态
	show plugins; 

##### 启用半同步

1. 先启用从库上的参数，租后启用主库上的参数
	从库：
		set global rpl_semi_sync_slave_enabled=1; 1: 启用  0:停用
	主库：
		set global rpl_semi_sync_master_enabled=1;
		set global rpl_semi_sync_master_timeout=10000; #单位ms
		

	show global variables like '%sync%';
	
	// 等待点（半同步）5.7默认
	set semi_sync_master_wait_point='AFTER_SYNC';
	
	


