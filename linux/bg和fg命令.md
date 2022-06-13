[bg和fg命令](https://www.cnblogs.com/chjbbs/p/6307333.html)

linux提供的fg和bg命令，可以让我们轻松调度正在运行的任务

 假如你发现前天运行的一个程序需要很长的时间，但是需要干前天的事情，你就可以用ctrl-z挂起这个程序，然后可以看到系统的提示:
```sh
[1]+ Stopped /root/bin/rsync.sh
```

然后我们可以吧程序调度到后台执行：（bg 作业号）

```sh
bg 1

[1]+ /root/bin/rsync.sh &
```

用jobs命令查看任务

```sh
jobs

[1]+ Running /root/bin/rsync.sh &
```



把它调回到控制台运行

```sh
fg 1

/root/bin/rsync.sh
```



这样，你这控制台上就只有等待这个任务完成了。

fg、bg、jobs、&、 ctrl+z都是跟系统任务有关的，学会了相当的实用

一、&最经常被用到

这个用在一个命令的最后，可以把这个命令放到后台执行

二、ctrl + z

可以将一个正在前台执行的命令放到后台，并且暂停

三、jobs

查看当前session下有多少在后台运行的命令

四、fg

将后台中的命令调至前台继续运行

如果后台有多个命令，可以用fg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号（不是pid）

五、bg

将一个在后台暂停的命令，变成继续执行

如果后台有多个命令，可以用bg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号（不是pid）