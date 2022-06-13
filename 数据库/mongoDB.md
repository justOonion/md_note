### mongoDB 安装
安装目录 **/usr/local/mongodb/**

#### 下载
```shell
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz
```

#### 解压安装
```shell
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz
cd /usr/local/mongodb/
mkdir -p data/db
mkdir logs
cd bin
# 创建配置文件
touch mc.conf
echo "dbpath=/usr/local/mongodb/data/db
logpath=/usr/local/mongodb/logs/mongodb.log
port=27017
fork=true
journal=false
storageEngine=mmapv1" >> mc.conf
./mongod -f mc.conf #启动mongodb, mc.conf为配置文件
```

 ```shell
 export PATH=/usr/local/mongodb/bin/:$PATH  #添加到环境变量，可直接用命启动
 ```

 2. mongo (启动客户端)

 3. 在mongodb客户端执行

```shell use xdo-import
db.cloneDatabase("192.168.12.233:27800")   #从远处拷贝同名数据库所有数据到当前数据库
db.createUser({user:"xdo",pwd:"dcjet",roles:[{role:"readWrite",db:"xdo-import"}],mechanisms : ["SCRAM-SHA-1"]}) #创建用户
use xdo-import #创建数据库
db.createUser(
   {
     user: "xdo",
     pwd: "dcjet",
     roles:
       [
         { role: "readWrite", db: "xdo-import" }
       ]
   }
);#创建用户
```

​     

 附加 添加开机启动mongodb :
 echo "/usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/db –logpath=/usr/local/mongodb/logs –logappend  --auth -–port=27017" >> /etc/rc.local

