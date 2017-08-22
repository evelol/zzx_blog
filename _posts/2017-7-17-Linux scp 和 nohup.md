scp 用法
和cp一样的吧。用 -r 选项就OK了。
scp -r user@host:dir ...
1、获取远程服务器上的文件
 
scp -P 2222 root@www.vpser.net:/root/lnmp0.4.tar.gz /home/lnmp0.4.tar.gz

上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。 root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4.tar.gz 表示远程服务器上的文件，最后面的/home/lnmp0.4.tar.gz表示保存在本地上的路径和文件名。

2、获取远程服务器上的目录
 
scp -P 2222 -r root@www.vpser.net:/root/lnmp0.4/ /home/lnmp0.4/
3、将本地文件上传到服务器上
 
scp -P 2222 /home/lnmp0.4.tar.gz root@www.vpser.net:/root/lnmp0.4.tar.gz
 
上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。 /home/lnmp0.4.tar.gz表示本地上准备上传文件的路径和文件名。root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4.tar.gz 表示保存在远程服务器上目录和文件名。
 
4、将本地目录上传到服务器上
 
scp -P 2222 -r /home/lnmp0.4/ root@www.vpser.net:/root/lnmp0.4/
 
上 端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。-r 参数表示递归复制（即复制该目录下面的文件和目录）；/home/lnmp0.4/表示准备要上传的目录，root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4/ 表示保存在远程服务器上的目录位置。


nohup 继续执行


secureCRT断掉后想继续运行程序。也就是程序会在后台一直执行到结束。
secureCRT通过ssh联接到client端运行程序，如果关掉窗口或者断开连接时，任务会被kill。
 
为了程序能够后台执行到结束：
1.可以用&：
比如 python test.py 改成 python test.py &
来调用执行。
2.另外，还有一种方式：通过ctrl+z、bg等一系列的命令，将已经在前台运行的作业放到后台执行：
如果一个作业已经在前台执行，可以通过ctrl+z将该作业放到后台并挂起。然后通过jobs命令查看在后台执行的作业并找到对应的作业ID，执行bg %n(n为通过jobs查到的作业ID)唤醒该作业继续执行。
该方式也存在结果会输出到终端上的情况，同样可以用重定向的方法解决
相关命令：
jobs------------查看在后台执行的进程
fg %n----------将后台执行进程n调到前台执行，n表示jobnumber（通过jobs查看的进程编号，而非pid）
ctrl+z----------将在前台执行的进程，放到后台并挂起
bg %n---------将在后台挂起的进程，继续执行
ctrl+c----------前台进程终止
kill %n---------杀掉后台运行的进程，n表示jobnumber（通过jobs查看的进程编号，而非pid）
 
但上面来两种方法只能解决disconnect时的问题。
如果关闭客户端，程序一样无法执行。当用户注销或者网络中断时，终端后收到SIGHUP信号，从而关闭其所有子进程。
这就要使用nohup执行
 
3.nohup命令：nohup + command
如 nohup python test.py
nohup就是不挂起的意思，nohup命令会忽略SIGHUP信号，从而终端退出时不会影响到后台作业。后台运行把终端输出存放在当前目录下的nohup.out文件中。客户端关机后重新登陆服务器可以直接查看。
 
4.此外还有screen命令方式也可以实现，不介绍了。
 
 
程序运行后用
ps -ef  | grep '进行id'
或者ps -aux 来查看后来多少进行正在运行，grep自己的用户名，可以看到对应程序
 
 
原理如下：
在Linux/Unix中，有这样几个概念：
进程组（process group）：一个或多个进程的集合，每一个进程组有唯一一个进程组ID，即进程组长进程的ID。
会话期（session）：一个或多个进程组的集合，有唯一一个会话期首进程（session leader）。会话期ID为首进程的ID。
会话期可以有一个单独的控制终端（controlling terminal）。与控制终端连接的会话期首进程叫做控制进程（controlling process）。当前与终端交互的进程称为前台进程组。其余进程组称为后台进程组。
根据POSIX.1定义：
挂断信号（SIGHUP）默认的动作是终止程序。
当终端接口检测到网络连接断开，将挂断信号发送给控制进程（会话期首进程）。
如果会话期首进程终止，则该信号发送到该会话期前台进程组。
一个进程退出导致一个孤儿进程组中产生时，如果任意一个孤儿进程组进程处于STOP状态，发送SIGHUP和SIGCONT信号到该进程组中所有进程。
因此当网络断开或终端窗口关闭后，控制进程收到SIGHUP信号退出，会导致该会话期内其他进程退出。

securecrt 自动断开问题
Options->Session Options->Terminal->Anti-idle->勾选Send protocol NO-OP
(中文版：选项->会话选项->终端->反空闲->发送协议NO-OP)



