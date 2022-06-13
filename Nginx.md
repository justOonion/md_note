**nginx**：打开 nginx
**nginx -t** ：测试配置文件是否有语法错误
**nginx -s reopen**：重启Nginx
**nginx -s reload**：重新加载Nginx配置文件，然后以优雅的方式重启Nginx
**nginx -s stop**：强制停止Nginx服务
**nginx -s quit**：优雅地停止Nginx服务（即处理完所有请求后再停止服务）



###  location 匹配顺序

location = /uri 　　 # **=**   表示精确匹配，只有完全匹配上才能生效。
location ^~ /uri 　  # **^~** 对URL路径进行前缀匹配，并且在正则之前。
location ~ pattern   # **~**  表示 **区分大小写** 的正则匹配。
location ~* pattern  # **~*** 表示 **不区分大小写** 的正则匹配。
location /uri 　　　 # **按字符串长短进行匹配，所以location顺序是无关紧要的**。
location / 　　　　 # 通用匹配，任何未匹配到其它location的请求都会匹配到



### location 匹配细节
 **最长前缀匹配，是location匹配的核心原则。**

![img](https://www.taohui.org.cn/images/nginx/location%E7%9A%84%E5%8C%B9%E9%85%8D%E6%B5%81%E7%A8%8B.png)

- 先对前缀location执行最长前缀匹配

- 若最长前缀location前，携带有=或者^~，那么使用此location配置块处理请求；

- 按server{ }中正则表达式的出现顺序，依次匹配。成功后就选中此location；

- 若所有正则表达式皆未匹配上，则使用第1步中检索出的最长前缀location处理请求。

  


### nginx 故障转移

	集群中有一台服务器挂了或者请求超时了，nginx会将已经发送至该服务器的请求 重新发送到另一台服务器

### nginx 雪崩
	故障转移是个很好的功能，但有个坑。
	在并发很大的时候，服务器太忙导致处理用户请求出现了超时，因为故障转移机制，nginx会将这些超时的请求交给集群中的其他服务器去处理。
	但是同样一个集群，一般来说所有的服务器所承载的压力都是同样大小的，这样的话，其他服务器因为要处理多出来的请求，很有可能也会因为压力过大导致奔溃。
	这样的连锁反应，就会导致整个集群变得不可用。这种现象就是nginx雪崩。

### 雪崩的解决方案
-	限制重试次数  即若一个请求超时到一定次数后就不再重试了 
	配置：proxy_next_upstream_tries 3
-	限制重试超时时间 即一个请求在集群中的重试总时间超过了规定的时间后就放弃重试
	配置： proxy_next_upstream_timeout 60


###nginx 熔断
	当一台被代理的服务器处理请求，出现一定次数（熔断次数 max_fails）的错误的情况下，nginx在一定时间内，将不会吧请求分配给这台服务器，这个时间就是熔断时间 (fail_timeout)。过了熔断时间后，nginx会在次尝试分配一次请求给服务器处理，如果还失败，则继续熔断
```config
upstream tomcat servers{
	server 192.168.0.81:8080 weight=3 max_fails=3 fail_timeout=10s;
	server 192.168.0.81:8081 weight=2 max_fails=3 fail_timeout=10s;
}
```

