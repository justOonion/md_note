### netstat
```netstat -ntlp``` 显示所有运行中的进程 端口号、PID
**macOS**  lsof -i （list open file）



###Crontab -e 添加定时任务 （定时执行命令/脚本）
crontab -e
拼写定时脚本
**　　**　　**　　**　　**　　command 
分　  时　   日　  月　   周　   命令 
解 释: 
	分：分钟1～59 每分钟用或者 /1表示 
	时：小时1～23（0表示0点） 
	日：日期1～31 
	月：月份1～12 
	周：星期0～6（0表示星期天） 
	命令：要运行的命令



###awk
awk -F: '/pattern/{print $1 $2 " " NR, NF, $NF}'



###Terminal
ctrl + a : 跳到行首
ctrl + e : 跳到行尾
ctrl + l  : 清屏（clear）
ctrl+u : 删除**光标 至 行首**的内容
ctrl+k : 删除**光标 至 行尾**的内容



###VIM 模式切换 
[go to...](https://www.cnblogs.com/zzqcn/p/4619012.html)
普通模式、编辑模式、命令模式、可视模式



## extglob模式
extglob模式开启之后Shell可以另外识别出5个模式匹配操作符，能使文件匹配更加方便. 不然不识别！
```shell
#开启命令：
shopt -s extglob

#关闭命令：
shopt -u extglob
```

### 5个模式匹配操作符

1. **?(pattern1|pattern2|...)** - 所给模式匹配0次或1次；

2. ***(pattern1|pattern2|...)** - 所给模式匹配0次以上包括0次；

3. **+(pattern1|pattern2|...)** - 所给模式匹配1次以上包括1次；

4. **@(pattern1|pattern2|...)** - 所给模式仅仅匹配1次；

5. **!(pattern1|pattern2|...)** - 不匹配括号内的所给模式。

### 使用案例
```shell
#反选删除文件：
#（打开extglob模式）
shopt -s extglob     
rm -fr !(file1)

#多个要排除的：
rm -rf !(file1|file2)
```

   

### 通过PID 寻找程序路径
```jps``` 查找java编写的程序运行情况  (pid, processName)
```pwdx pid```  指定PID进程 对应的路径
```top``` 查询系统资源
```netstat -ntlp``` 进程列表 包含PID



### brew update更新慢问题解决
使用中科大的镜像
替换默认源
第一步，替换brew.git

```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```
第二步：替换homebrew-core.git
```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```
最后使用
```bash
brew update
```
进行更新，发现速度变的很快。替换镜像完成。



### wc 命令 （用于计算字数）
-c 或 --bytes或--chars 只显示Bytes数。
-l 或 --lines 只显示行数。
-w 或 --words 只显示字数。
--help 在线帮助。
-- version 显示版本信息。

### pkill 
kill 对应的是 PID
pkill 对应的是COMMAND，例如：pkill sshd



### passwd 修改用户密码
```passwd root``` 修改root用户密码



### #!/bin/bash和#!/bin/sh 区别

都只能放在 **第一行**，否则会被当作注释。如果不放，会使用系统默认的shell程序解释执行
bash支持功能多，慢，执行错误bash之后 **继续** 执行
sh 支持功能少，快，执行错误之后sh **不继续** 执行

### tail 监视文件末尾 tail [-F | -f | -r] [-q] [-b # | -c # | -n #] [file ...]
**tail -f** log.log 监视log.log末尾（默认显示10行）
**tail -n 20** log.log 显示20行
**tailf**        等同于tail -f -n 10


### nl   显示行号 (默认空行不加行号)
**nl -b a** log.log  空行加行号
**nl -b a -n rz** log.log  让行号前面自动补上0,统一输出格式（默认6位）
**nl -b a -n rz -w 3** log2014.log  行号-w 3 （3位）

### diff 比较两个文件
``` shell
diff txt1.txt txt2.txt -y -W 50 //-y side by side, -W:width
```

