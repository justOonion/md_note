### ssr服务器 centOS 一键安装

``` shell
ssh root@104.243.24.201 -p 27931

yum -y install wget

Apt-get -y install wget

wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh

chmod +x shadowsocks-all.sh

./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log



```

### 接下来会有几个参数需要选择，依次为：

- 提示选择哪个版本安装，我们输入2后按回车，即选择SSR安装。

- 然后会提示设置SSR密码，输入自定义密码后按回车，建议不要使用默认密码。

- 接下来选择SSR要使用的服务器端口，随便输入一个，也可以默认回车。

- 然后选择加密方式，如果选择chacha20的话，就输入对应序号12，按回车继续。

- 接下来选择协议，建议选择自auth_aes128_md5开始以下的几种，输入对应序号按回车。

- 然后选择混淆方式，如下图所示，选择好后按回车。

- 以上参数选择完成后，按任意键开始安装。

- 安装完成后，会有如下图安装成功的提示，记住刚才设置的各项参数，在客户端连接时需要用到。

- 经过以上几个简单的参数选择后，SSR服务器端已经自动安装成功了。保险起见，输入reboot重启VPS服务器，SSR会自动随系统重启。

  

  

  

  SR服务端安装成功后，就可以在电脑、手机、路由器等设备上的SSR客户端上，按照以上设置的各项参数进行连接了。

  ![image-20210525093939700](/Users/chencheng/Library/Application Support/typora-user-images/image-20210525093939700.png)







