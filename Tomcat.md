### Tomcat原理分析

Tomcat中最顶层的容器是**Server**，代表着整个服务器，一个Server可以包含多个Service，用于具体提供服务。Service主要包含两个部分：
1. Connector用于处理连接相关的事情， 并提供Socket与Request和Response相关的转换；
2. Container用于封装和管理Servlet，以及具体处理Request请求；



