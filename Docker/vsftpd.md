## docker 安装vsftpd

### 拉取镜像
```sh
docker pull fauria/vsftpd
```
### 启动容器

**参数说明：**

- **/Users/chencheng/ftpFile:/home/vsftpd**：映射 **docker** 容器 **ftp** 文件根目录（冒号前面是宿主机的目录）
- **-p**：映射 **docker** 端口（冒号前面是宿主机的端口）
- **-e FTP_USER=test -e FTP_PASS=test** ：设置默认的用户名密码（都为 **test**）
- **PASV_ADDRESS**：宿主机 **ip**，当需要使用被动模式时必须设置。
- **PASV_MIN_PORT~ PASV_MAX_PORT**：给客服端提供下载服务随机端口号范围，默认 **21100-21110**，与前面的 **docker** 端口映射设置成一样。

```sh
docker run -d -v /Users/chencheng/ftpFile:/home/vsftpd \
-p 20:20 -p 21:21 -p  21100-21110:21100-21110 \
-e FTP_USER=cc -e FTP_PASS=123456 \
-e PASV_ADDRESS=192.168.1.104 \
-e PASV_MIN_PORT=21100 -e PASV_MAX_PORT=21110 \
--name vsftpd --restart=always fauria/vsftpd
```



### 访问服务
```
ftp://test:test@192.168.1.104:21
```

