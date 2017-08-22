注意，这两个 程序的安装路径要没有空格
不安装oracle客户端，用plsql连接oracle
常用的Oracle开发的工具有SQL Developer和PL/SQL Developer，个人感觉前者虽然跨平台性优于后者，但比较大（大于300M）占用资源，而且用户体验也一般，而后者相对就小很多（小于30M），用起来也比较顺手。
用PL/SQL连接oracle数据库，不管是本地的还是远程的，一般都需要安装oracle客户端（500M左右）比较大，而且在各个系统上安装也有些讲究，需要修改相应配置文件，有点麻烦。
如何达到不安装oracle客户端，又能使用PL/SQL工具？本人做了如下测试并达到目的。
测试环境：win7系统，未安装过oracle包括客户端，本机虚拟机linux中装有oracle，并有一个可用的orcl实例；
1. oracle官网提供了instantclient，下载instantclient-basic包，将包解压存放到本地，本测试为
    D:\STUDY\instantclient_10_2；
2. 在这个目录下新建sqlnet.ora和tnsnames.ora两个文件
sqlnet.ora文件内容
---------------------
SQLNET.AUTHENTICATION_SERVICES= (NTS)   
NAMES.DIRECTORY_PATH= (TNSNAMES, EZCONNECT)
tnsnames.ora文件内容
-------------------------
orcl =   
(DESCRIPTION =      
  (ADDRESS_LIST =         
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.146.143 )(PORT = 1521))      
  (CONNECT_DATA =        
    (SERVICE_NAME = orcl )      
注：斜体部分需要按需配置，如果端口是自定义的也需要相应变更。
3. 配置环境变量
变量名                       变量值
TNS_ADMIN            D:\STUDY\instantclient_10_2
注：这里变量值即为instantclient在本地的主目录；如果不配置该变量，
       连接时会报错ORA-12154: TNS:could not resolve the connect identifier specified。
4. 安装PL/SQL Developer，并配置oracle的连接参数：
    打开PL/SQL，Tools-->Preferences，配置Connection的Oracle Home和OCI library，如下图所示
 重新启动PL/SQL，登录测试，OK！
如果出现乱码问题，请点击：http://gang-chen.iteye.com/blog/2005546
另附下载地址：
PL/SQL Developer：http://www.allroundautomations.com/bodyplsqldevreg.html
instantclient-basic-win32：http://pan.baidu.com/share/link?shareid=514153&uk=3255414737
也可以去oracle官网下载instantclient其他版本，只是不太稳定，有时会下载失败。 
参考资料：

http://wenku.baidu.com/link?url=HdNSsEe21lyBVzoE7Ih2-BUaw-2HM7pOkQU23i6LkZad-I3nepjPrCtzTlYo1Egvm4NfCVDRA7DQq9FwwTvL2yjP06B4AqZVim9TjMkO9lW
