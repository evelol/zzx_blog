profile.d是一个目录
profile是个文件,开头为-, 其部分内容

	1. for i in /etc/profile.d/*.sh ; do
	2. if [ -r "$i" ]; then
	3. if [ "${-#*i}" != "$-" ]; then
	4. . "$i"
	5. else
	6. . "$i" >/dev/null 2>&1
	7. fi
	8. fi
	9. done
	10. unset i
	11. unset -f pathmunge
	12. export JAVA_HOME=/usr/local/java
	13. export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	14. export PATH=$JAVA_HOME/bin:$PATH
	15. export PATH=$PATH:/usr/local/erlang/bin/
	16. ulimit -n 65535
	17. export HDC_HOME=/home/sa/hdc_home


1. 两个文件都是设置环境变量文件的，/etc/profile是永久性的环境变量,是全局变量，/etc/profile.d/设置所有用户生效
2. /etc/profile.d/比/etc/profile好维护，不想要什么变量直接删除/etc/profile.d/下对应的shell脚本即可，不用像/etc/profile需要改动此文件
用法：
CentOS 中每个用户都要指定各自的环境变量，其中包括可执行的 path路径，这些路径决定了每个用户在执行时的命令工具。  
一般情况下，可以再每个用户的环境变量里设定各自的 path变量值，然后再执行export PATH使其生效，但如果用户比较多，安装命令工具也原来越多，且出来本身用户可以执行这些工具，root用户或其他用户也可以执行命令，这时在每个用户环境变量里添加就比较复杂了。所以可以用另外一种方法： 
可以再 /etc/profile.d/ 目录下创建一个 path.sh 脚本，脚本内容如下： 
例子：
用代码格式表示内容如下：
# cat /etc/profile.d/path.sh
export PATH="/bin:/usr/bin:/sbin:/usr/sbin:/usr/local/sbin:/usr/local/nginx/sbin:/usr/local/php/bin:/usr/local/php/sbin:/usr/local/mysql/bin:/usr/local/redis/bin:/usr/local/daemontools/bin:/usr/local/daemontools/sbin:/usr/local/percona-xtrabackup/bin"
可以将需要各个用户需要执行的命令路径都写在该命令中，这样每次操作系统其中后，会自动执行path.sh脚本，将所有的环境变量生效，让各个用户都可以直接执行各自的命令。以后安装了新软件后，只需要将软件相关路径加入 /etc/profile.d/path.sh 脚本中，不需要再多个地方重复添加，使脚本生效后，各个用户都可以使用了。
Linux中profile文件简介(2011-09-05 09:52:33)
转载▼
标签： 
杂谈
 分类： 编程学习
1、Linux是一个多用户的操作系统。每个用户登录系统后，都会有一个专用的运行环境。通常每个用户默认的环境都是相同的，这个默认环境实际上就是一组环境变量的定义。用户可以对自己的运行环境进行定制，其方法就是修改相应的系统环境变量。
2、常在/etc/profile文件中修改环境变量，在这里修改的内容是对所有用户起作用的。使用修改.bashrc文件进行环境变量的编辑，只对当前用户有用。使用修改 /etc/profile 文件进行环境变量的编辑，是对所有用户有用。
以下主要操作将在该文件中进行
3、如何添加环境变量。
　例如添加”NAME=liheng“ 。在profile文件的最后添加如下内容export NAME=liheng变量值liheng可以加引号也可以不加，效果一样。
4、在profile文件添加或修改的内容需要注销系统才能生效。
5、重复定义变量怎样理解。
经常出现的是对PATH变量的定义。
例如：在peofile文件默认对PATH变量都有设置PATH=￥￥￥￥￥￥￥（记不住了），在以后可能在对PATH设置，一般都加在 profile 文件的最后PATH=······（打个比方）。而系统之中认定的PATH=·······￥￥￥￥￥￥￥￥￥，也就是说相同名字的环境变量，后写入的先起作用（通俗地讲）。
6、特殊字符介绍。
例如在profile中有如下内容，通过以下内容说明特殊符号的用法。
export A=/q/jing:aaa/cc/ld
export B=.:/liheng/wang export A=/cd/cdr:$A
大家注意红色的符号:
: 表示并列含义，例如A变量值有多个，用：符号进行分离。
. 表示你操作的当前目录。例如pap命令会查找B环境变量。
7、在LInux下profile的拥有着：

如是/etc下的，是root。 如是/home下的，属于/home下的这个用户。


 


8、使用env命令显示所有的环境变量 。在命令提示符下键入env就行了。
　　set命令显示所有本地定义的Shell变量。
9、常见的环境变量 PATH：决定了shell将到哪些目录中寻找命令或程序；
    HOME：当前用户主目录；
   MAIL：是指当前用户的邮件存放目录；
SHELL：是指当前用户用的是哪种Shell。
HISTSIZE：是指保存历史命令记录的条数
LOGNAME：是指当前用户的登录名。
HOSTNAME：是指主机的名称，许多应用程序如果要用到主机名的话，通常是从这个环境变量中来取得的。
LANG/LANGUGE：是和语言相关的环境变量，使用多种语言的用户可以修改此环境变量。

来源： http://blog.sina.com.cn/s/blog_4aecb6260100vyz3.html
