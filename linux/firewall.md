

## linux 防火墙


```shell
firewall-cmd --state #查看防火墙状态
systemctl stop firewalld.service #停止
systemctl start firewalld.service #启动

systemctl enable firewalld #firewall开机启动
systemctl disable firewalld.service #禁止firewall开机启动

firewall-cmd --zone=public --add-port=80/tcp --permanent  #添加80端口
firewall-cmd --zone=public --query-port=80/tcp --permanent  #查看80端口
firewall-cmd --zone=public --remove-port=80/tcp --permanent #移除80端口
firewall-cmd --reload # 重载防火墙设置，使设置生效

#批量设置端口 100-500全部开放 （remove同理）
firewall-cmd --zone=public --add-port=100-500/tcp --permanent

firewall-cmd --zone=public --list-ports #查看所有开放端口

```

