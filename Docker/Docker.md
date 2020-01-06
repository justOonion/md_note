### Docker
```chkconfig docker on``` 服务端deamon程序开机启动

```docker run``` 是基于镜像创建容器的命令，他有两个基本的命令行参数 -i、-t
	-i 标志着容器的STDIN是开启的，持久的标准输入是交互式shell的“半边天“
	-t 则标志着另外的"半边天"，他告诉docker要为刚创建的容器分配一个伪tty（teletypewriter）终端
有了这两个参数，新创建的容器才能提供一个交互式shell。
	--name 创建容器时，使用--name参数给容器命令
	-d （daemon）创建守护式容器
	--restart 自动重启容器 always、on_failures:5

```bash
docker run --restart=always 
```

```docker start``` 重新启动已停止的容器
```docker attach``` 附着到正在运行的容器中
```docker logs```  查看容器的日志
```docker top``` 查看容器内的进程

```docker exec``` 在容器内部运行进程
	-d 表明运行一个后台进程 

```bash
#后台进程
docker exec -d daemon_cc touch /etc/new_config_file
#交互式进程
docker exec -t -i daemon_cc /bin/bash
```

```docker stop```停止容器（会向容器发送 SIGTERM 信号）
```docker kill```快速停止容器（会向容器发送 SIGKILL 信号）
```ctrl + P + Q``` 退出容器 不退出容器

删除容器：
```docker rm -f containId``` 删除一个运行状态的容器
删除镜像
```docker rmi imgId```

**导出**镜像 (-o和>表示输出到文件)
```docker save （-o 或 >） img.tar imgNm:imgVer```
**导入**镜像
```docker load (-i 或 <) img.tar```

**导出**容器到压缩文件 container.tar
```docker export containerId > container.tar```
**导入**容器 container.tar
```cat container.tar | docker import - imgNm:imgVer```

**将容器提交到本地镜像**
```docker commit containId imgNm:imgVer```





### docker 安装mysql (3,4,5可省略)
1) **下载镜像**  
	docker pull mysql:5.7
2) **运行镜像**
	docker run --name mysqlserver -e MYSQL_ROOT_PASSWORD=123456 -d -i -p 3307:3306  mysql:5.7
3) **进入容器**
	docker exec -it mysqlContainerId /bin/bash
4) **设置局域网可访问**
	grant all privileges on *.* to root@"%" identified by "password" with grant option;
	flush privileges;
5) **修改密码**
	ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
	flush privileges;



### 卷映射
**-v** 本地目录:容器目录
```docker run -it -v /hostPath:/containerPath centOS:latest```