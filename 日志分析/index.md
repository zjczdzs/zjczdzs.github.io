# 日志分析


<!--more-->

@[toc]
# 1.流量与日志分析

日志，是作为记录系统与服务最直接有效的方法。在日志中，可 以发现访问记录以及发现攻击线索。日志分析也是最常用的分析安全 事件所采用的途径。系统日志和 web 日志分别记录了不同内容，为分析 攻击提供了有效证据。网络流量分析，也是作为排查安全事件所能获 得的有效证据，通过学习，学员可以了解系统和服务的主要日志，并能够通过分析获取攻击线索。

## 1.1系统日志分析

### 1.1.1window系统日志与分析方法

前提:开启审核策略，若日后系统出现故障、安全事故则可以查看系统的日志文件、排除故障，追查入侵者的信息等。



![c150661356f04eb1ff85c7f2892cfea6.png](https://img-blog.csdnimg.cn/img_convert/c150661356f04eb1ff85c7f2892cfea6.png)

![406339bdcc999151b8093097d1906d92.png](https://img-blog.csdnimg.cn/img_convert/406339bdcc999151b8093097d1906d92.png)

![02cee60d0ee037f6c84c0219e83ddfbe.png](https://img-blog.csdnimg.cn/img_convert/02cee60d0ee037f6c84c0219e83ddfbe.png)

快捷        Win+R打开运行 → 输入 gpedit.msc 回车 → 计算机配置 → Windows 设置 → 安全设置 → 本地策略 → 审核策略。
![(C:\Users\ZHAI\AppData\Roaming\Typora\typora-user-images\image-20220721194234173.png)\]](https://img-blog.csdnimg.cn/3d144faeedfc418e81df439d0077be7f.png)


查看日志

Win+R打开运行，输入“eventvwr.msc”，回车运行，打开“事件查看器”。

	![在这里插入图片描述](https://img-blog.csdnimg.cn/7646d596b6c444a5a9ec5af32dfebcb3.png)

筛选查看

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200218160244385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDg5NTcw,size_16,color_FFFFFF,t_70)

**系统日志**：记录操作系统组件产生的事件，主要包括驱动程序、系统组件和应用软件的崩溃以及数据丢失错误等。默认存放路径：%SystemRoot%\System32\Winevt\Logs\System.evtx

**安全日志**：记录系统的安全审计事件，包含各种类型的登录日志、对象访问日志、进程追踪日志、特权使用、帐号管理、策略变更、系统事件。这个日志一般是安全工程师重点关注对象。

默认存放路径：%SystemRoot%\System32\Winevt\Logs\Security.evtx

**应用程序日志**：

包含由应用程序或系统程序记录的事件，主要记录程序运行方面的事件，
默认存放路径：%SystemRoot%\System32\Winevt\Logs\Application.evtx。

对于Windows事件日志分析，不同的EVENT ID代表了不同的意义，摘录一些常见的安全事件的说明

```
4624  --登录成功   
4625  --登录失败  
4634 -- 注销成功
4647 -- 用户启动的注销   
4672 -- 使用超级用户（如管理员）进行登录
```

*分析工具*

Log Parser（是微软公司出品的日志分析工具，它功能强大，使用简单，可以分析基于文本的日志文件、XML 文件、CSV（逗号分隔符）文件，以及操作系统的事件日志、注册表、文件系统、Active Directory。它可以像使用 SQL 语句一样查询分析这些数据，甚至可以把分析结果以各种图表的形式展现出来。

Log Parser 2.2下载地址：https://www.microsoft.com/en-us/download/details.aspx?id=24659

Log Parser 使用示例：https://mlichtenberg.wordpress.com/2011/02/03/log-parser-rocks-more-than-50-examples/查看日志的重点

**分析重点**：

①查看登录日志中暴力破解痕迹；

②查看账号管理日志中账号的新增、修改痕迹；

③查看远程桌面登录日志中的登录痕迹。

**暴力破解账密日志**

攻击者通过暴力破解的方式入侵系统，不论是否成功，在日志中会留下入侵痕迹，所以事件id为4624和4625的事件是首当其冲的关注点。需要留意日志中的SubjectUserNameIpAddress。

**入侵事件**

发现连续三条日志，由登录失败到成功，WorkstationName均来自名为kali的主机，并且最终记录下kali的IP地址为192.168.74.129。这个过程可以判断攻击者通过192.168.74.129的主机暴力破解成功administrator的密码，此处即为暴破留下的痕迹

**账号管理日志**

Windows中日志中与账号创建有关的事件ID：4720, 4722, 4724,4738。攻击者攻陷一台Windows主机后，可能会创建后门账号、隐藏账号

**远程桌面登录日志**

上述的安全日志很可能被覆盖掉，为尽量不遗漏入侵痕迹，可进一步查看远程桌面的登录日志。攻击者建立后门账号后会通过远程桌面连接到失陷主机上，此时的登录行为会记录到远程桌面日志。

远程连接日志（应用程序和服务日志->Microsoft->Windows->-TerminalServices->RemoteConnectionManager->Operational），重要事件 ID 和含义：

1149：用户认证成功

21：远程桌面服务：会话登录成功

24：远程桌面服务：会话已断开连接

25：远程桌面服务：会话重新连接成功

因此我们可以看看应用程序日志里事件id为1149：

### 1.1.2linux 系统日志与分析方法

Linux系统拥有非常灵活和强大的日志功能，可以保存几乎所有的操作记录，并可以从中检索出我们需要的信息。

大部分Linux发行版默认的日志守护进程为 syslog，位于 /etc/syslog 或 /etc/syslogd 或/etc/rsyslog.d，默认配置文件为 /etc/syslog.conf 或 rsyslog.conf，任何希望生成日志的程序都可以向 syslog 发送信息。

Linux系统内核和许多程序会产生各种错误信息、警告信息和其他的提示信息，这些信息对管理员了解系统的运行状态是非常有用的，所以应该把它们写到日志文件中去。完成这个过程的程序就是syslog。syslog可以根据日志的类别和优先级将日志保存到不同的文件中。

*默认配置下，日志文件通常都保存在“/var/log”目录下。*

常见的日志类型，但并不是所有的Linux发行版都包含这些类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/64f037eb94dd452caf7ebbece5b0795d.png)


*常见的日志优先级:*
![在这里插入图片描述](https://img-blog.csdnimg.cn/3196edbe18fa4cb99186c291bf830852.png)


系统日志是由一个名为syslog的服务管理的，如以下日志文件都是由syslog日志服务驱动的：

```
/var/log/boot.log：录了系统在引导过程中发生的事件，就是Linux系统开机自检过程显示的信息

/var/log/lastlog ：记录最后一次用户成功登陆的时间、登陆IP等信息

/var/log/messages ：记录Linux操作系统常见的系统和服务错误信息

/var/log/secure ：Linux系统安全日志，记录用户和工作组变坏情况、用户登陆认证情况

/var/log/btmp ：记录Linux登陆失败的用户、时间以及远程IP地址

/var/log/syslog：只记录警告信息，常常是系统出问题的信息，使用lastlog查看

/var/log/wtmp：该日志文件永久记录每个用户登录、注销及系统的启动、停机的事件，使用last命令查看

/var/run/utmp：该日志文件记录有关当前登录的每个用户的信息。如 who、w、users、finger等就需要访问这个文件

/var/log/syslog 或 /var/log/messages 存储所有的全局系统活动数据，包括开机信息。基于 Debian 的系统如 Ubuntu 在/var/log/syslog 中存储它们，而基于 RedHat 的系统如 RHEL 或 CentOS 则在 /var/log/messages 中存储它们。

/var/log/auth.log 或 /var/log/secure 存储来自可插拔认证模块(PAM)的日志，包括成功的登录，失败的登录尝试和认证方式。Ubuntu 和 Debian 在 /var/log/auth.log 中存储认证信息，而 RedHat 和 CentOS 则在 /var/log/secure 中存储该信息。
```

## 1.2 web日志分析

Web访问日志记录了Web服务器接收处理请求及运行时错误等各种原始信息。通过对WEB日志进行的安全分析，不仅可 以帮助我们定位攻击者，还可以帮助我们还原攻击路径，找到网站存在的安全漏洞并进行修复。

### iis 日志分析方法

与开发阶段不同的，运维阶段不可能让你去调试程序，发现各类问题， 我们只能通过各种系统日志来分析网站的运行状况， 对于部署在IIS上的网站来说，IIS日志提供了最有价值的信息，我们可以通过它来分析网站的响应情况，来判断网站是否有性能问题， 或者存在哪些需要改进的地方

![在这里插入图片描述](https://img-blog.csdnimg.cn/f8255ee783a54589b12af7229950ea47.png)


这里面记录了：

1. 请求发生在什么时刻，
2. 哪个客户端IP访问了服务端IP的哪个端口，
3. 客户端工具是什么类型，什么版本，
4. 请求的URL以及查询字符串参数是什么，
5. 请求的方式是GET还是POST，
6. 请求的处理结果是什么样的：HTTP状态码，以及操作系统底层的状态码，
7. 请求过程中，客户端上传了多少数据，服务端发送了多少数据，
8. 请求总共占用服务器多长时间、等等。

有个叫 [**Log Parser**](http://www.microsoft.com/en-us/download/details.aspx?id=24659) 的工具就可以专门解析IIS日志，我们可以用它来查看日志中的信息。

建议选择输出格式为 SQL 。
 注意：这里的SQL并不是指SQLSERVER，而是指所有提供ODBC访问接口的数据库。
 我可以使用下面的命令将IIS日志导入到SQLSERVER中（说明：为了不影响页面宽度我将命令文本换行了）：

```sql
"C:\Program Files\Log Parser 2.2\logparser.exe"  
"SELECT  *  FROM  'D:\Temp\u_ex130615.log'  to MyMVC_WebLog" -i:IISW3C -o:SQL 
-oConnString:"Driver={SQL Server};server=localhost\sqlexpress;database=MyTestDb;Integrated Security=SSPI" 
-createtable:ON
```

导入完成后，我们就可以用熟悉的SQLSERVER来做各种查询和统计分析了，例如下面的查询：

```sql
SELECT cip,csmethod,sport,csuristem,scstatus,scwin32status,scbytes,csbytes,timetaken 
FROM dbo.MyMVC_WebLog
```

	![](https://img-blog.csdnimg.cn/421c547a12b34e7a9f68bd1290ae460d.png)


注意：
IIS日志在将结果导出到SQLSERVER时，字段名中不符合标识符规范的字符将会删除。
  例如：c-ip 会变成 cip， s-port 会变成 sport 。
IIS日志中记录的时间是UTC时间，而且把日期和时间分开了，导出到SQLSERVER时，会生成二个字段：

对于一个ASP.NET程序来说，如果抛出一个未捕获**异常**，会记录到IIS日志中（500），

本文所说的异常可分为四个部分：

1.（ASP.NET）程序抛出的未捕获异常，导致服务器产生500的响应输出。

2.404之类的请求资源不存在错误。

3.大于500的服务器错误，例如：502，503

4.系统错误或网络传输错误。 

前三类异常可以用下面的查询获得：

```sql
select scStatus, count(*) AS count, sum(timetaken * 1.0) /1000.0 AS sum_timetaken_second
from MyMVC_WebLog with(nolock)
group by scStatus
order by 3 desc
```

### apache日志分析

如果apache的安装时采用默认的配置,那么在/logs目录下就会生成两个文件,分别是access_log和error_log

#### **access_log**

CustomLog "| /usr/sbin/rotatelogs /var/log/apache2/%Y_%m_%d_other_vhosts_access.log 86400 480" vhost_combined

通过CustomLog指令,每天一天生成一个独立的日志文件,同时也写了定时器将一周前的日志文件全部清除,这样可以显得更清晰,既可以分离每一天的日志又可以清除一定时间以前的日志通过制,LogFormat定义日志的记录格式

下面是一条经典的访问记录

```log
101.226.168.195 - - [17/Oct/2014:16:46:11 +0800] "GET /actkaijiang/3dinfo.html HTTP/1.1" 200 9678 "http://www.yicp.com/actkaijiang/3dinfo.html" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.89 Safari/537.1; 360Spider"
```

一共是有9项,将他们一一拆开

```
101.226.168.195
-
-
[17/Oct/2014:16:46:11 +0800]
"GET /actkaijiang/3dinfo.html HTTP/1.1"
200
9678
"http://www.yicp.com/actkaijiang/3dinfo.html"
"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.1 (KHTML, likeGecko)Chrome/21.0.1180.89Safari/537.1;360Spider"
```

(1) 101.226.168.195 这是一个请求到apache服务器的客户端ip,默认的情况下,第一项信息只是远程主机的ip地址,但我们如果需要apache查出主机的名字,可以将 HostnameLookups设置为on,但这种做法是不推荐使用,因为它大大的减缓了服务器.另外这里的ip地址不一定就是客户主机的ip地址,如果 客户端使用了代理服务器,那么这里的ip就是代理服务器的地址,而不是原机.

 (2) **-** 这一项是空白,使用"-"来代替,这个位置是用于标注访问者的标示,这个信息是由identd的客户端存在,除非IdentityCheck为on,非则apache是不会去获取该部分的信息(

(3) **-** 这一项又是为空白,不过这项是用户记录用户HTTP的身份验证,如果某些网站要求用户进行身份雁阵,那么这一项就是记录用户的身份信息

(4） [17/Oct/2014:16:46:11 +0800] 第四项是记录请求的时间,格式为[day/month/year:hour:minute:second zone],最后的+0800表示服务器所处的时区为东八区

(5)"GET /actkaijiang/3dinfo.html HTTP/1.1" 这一项整个记录中最有用的信息,首先,它告诉我们的服务器收到的是一个GET请求,其次,是客户端请求的资源路径,第三,客户端使用的协议时HTTP/1.1,整个格式为"%m %U%q %H",即"请求方法/访问路径/协议"

(6) 200 这是一个状态码,由服务器端发送回客户端,它告诉我们客户端的请求是否成功,或者是重定向,或者是碰到了什么样的错误,这项值为200，表示服务器已经成 功的响应了客户端的请求,一般来说,这项值以2开头的表示请求成功,以3开头的表示重定向,以4开头的标示客户端存在某些的错误,以5开头的标示服务器端 存在某些错误,
(9)**9678**这项表示服务器向客户端发送了多少的字节,在日志分析统计的时侯,把这些字节加起来就可以得知服务器在某点时间内总的发送数据量是多少

(10) **-** **http://www.yicp.com/actkaijiang/3dinfo.html** 表示请求来源

(11)**"*****\*Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.89 Safari/537.1; 360Spider\******"** 这项主要记录客户端的浏览器信息

#### **error_log**

error_log为错误日志,记录下任何错误的处理请求,它的位置和内容由ErrorLog指令控制,通常服务器出现什么错误,首先对它进行查阅,是一个最重要的日志文件

tail error_log,随意摘取一个记录

```
[Fri Dec 10 15:03:59 2010] [error] [client 218.19.140.242] File does not exist: /home/htmlfile/tradedata/favicon.ico
```

同样也是分为几个项

```
[Fri Dec 10 15:03:59 2010]
[error]
[client 218.19.140.242]
File does not exist: /home/htmlfile/tradedata/favicon.ico
```

   1.[Fri Dec 10 15:03:59 2010] 记录错误发生的时间,注意,它跟我们上面access_log记录的时间格式是不同的

2) [error] 这一项为错误的级别,根据LogLevel指令来控制错误的类别,上面的404是属于error级别

3) [client 218.19.140.242] 记录客户端的ip地址

4) File does not exist: /home/htmlfile/tradedata/favicon.ico 这一项首先对错误进行了描述,例如客户端访问一个不存在或路径错误的文件,就会给出404的提示错误

了解日志的各种定义后,这里分享一下从网上淘来的一些对日志分析的脚本

```
1.查看apache的进程数
ps -aux | grep httpd | wc -l

2.分析日志查看当天的ip连接数
cat default-access_log | grep "10/Dec/2010" | awk '{print $2}' | sort | uniq -c | sort -nr

3.查看指定的ip在当天究竟访问了什么url
cat default-access_log | grep "10/Dec/2010" | grep "218.19.140.242" | awk '{print $7}' | sort | uniq -c | sort -nr

4.查看当天访问排行前10的url
cat default-access_log | grep "10/Dec/2010" | awk '{print $7}' | sort | uniq -c | sort -nr | head -n 10

5.看到指定的ip究竟干了什么
cat default-access_log | grep 218.19.140.242 | awk '{print $1"\t"$8}' | sort | uniq -c | sort -nr | less

6.查看访问次数最多的几个分钟(找到热点)
awk '{print $4}' default-access_log |cut -c 14-18|sort|uniq -c|sort -nr|head
```

### nginx日志分析

Nginx是一个高性能的HTTP和反向代理服务器。Nginx access日志记录了web应用的访问记录。大致记录了访问方式（POST/GET）、客户端IP、远程用户、请求时间、请求状态码、访问host地址、请求页面大小、reffer信息、x_forwarded_for地址等等。nginx access日志的格式不是一成不变的，是可以自定义的。Nginx access具体日志格式与在服务器的存储位置可以查看nginx.conf配置文件。Nginx详细记录了每一次web请求。

```
log_format  combined  '$remote_addr - $remote_user  [$time_local]  '

' "$request"  $status  $body_bytes_sent  '

' "$http_referer"  "$http_user_agent" ';
```

如果nginx位于负载均衡器，squid，nginx反向代理之后，web服务器无法直接获取到客户端真实的IP地址了。  $remote_addr获取反向代理的IP地址。反向代理服务器在转发请求的http头信息中，可以增加X-Forwarded-For信息，用来记录客户端IP地址和客户端请求的服务器地址。
下面是修改后，生产环境下代理服务器用的日志格式。可以根据需要添加对应的日志参数

```
log_format  main  '$remote_addr - $remote_user [$time_local] requesthost:"$http_host"; "$request" requesttime:"$request_time"; '

'$status $body_bytes_sent "$http_referer" - $request_body'

'"$http_user_agent" "$http_x_forwarded_for"';
```

二：Nginx日志参数详解

参数注释如下：

```
$remote_addr   #与$http_x_forwarded_for 用以记录客户端的ip地址

$http_x_forwarded_for   #当前端有代理服务器时，设置web节点记录客户端地址的配置，此参数生效的前提是代理服务器也要进行相关的http_x_forwarded_for设置

$remote_user   #记录客户端用户名称,一般默认为空

$time_local   #记录访问时间

$request  #记录请求的URL和HTTP协议

$status   #记录请求状态

$body_bytes_sent   #记录发送给客户端文件内容大小

$http_referer  #记录从哪个页面链接访问过来的

$http_user_agent   #记录客户端浏览器相关信息

$request_time   #处理完请求所花时间，以秒为单位

$http_host   #请求地址，即浏览器中你输入的地址(IP或域名)

$request_body   #记录POST数据

$request_length   #客户端请求的长度

$upstream_status  #upstream状态，成功是200

$upstream_addr #后台upstream的地址，即真正提供服务的主机地址

$upstream_response_time    #请求过程中，upstream响应时间
```

Nginx日志常用分析命令示范(注：日志的格式不同，awk取的项不同。下面命令针对上面日志格式执行)

```
1)总请求数

wc -l  access.log |awk '{print $1}'

2)独立IP数

awk '{print $1}' access.log|sort |uniq |wc -l

3)每秒客户端请求数 TOP5

awk '{print $6}' access.log|sort|uniq -c|sort -rn|head -5

4)访问最频繁IP Top5

awk '{print $1}' access.log|sort |uniq -c |sort -nr |head -5

5)访问最频繁的URL TOP5

awk '{print $7}' access.log|sort |uniq -c |sort -nr |head -5

6)响应大于5秒的URL TOP5

awk '{if ($7 > 5){print $6}}' access.log|sort|uniq -c|sort -rn |head -5

7)HTTP状态码(非200)统计 Top5

awk '{if ($11 != 200){print $11}}' access.log|sort|uniq -c|sort -rn|head -5

8)分析请求数大于50000的源IP

cat access.log|awk '{print $NF}'|sort |uniq -c |sort -nr|awk '{if ($1 >50000){print $2}}'
```

### tomcat 日志分析

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200523145319655.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVzbWFu,size_16,color_FFFFFF,t_70#pic_center)

这些日志文件的产生是在tomcat/conf/logging.[properties](https://so.csdn.net/so/search?q=properties&spm=1001.2101.3001.7020)中配置的

Tomcat 日志信息分 为 两 类 ：

一是运行中的日志，它主要 记录 运行的一些信息，尤其是一些异常 错误 日志信息 。
二是 访问 日志信息，它 记录 的 访问 的 时间 ， IP ， 访问 的 资 料等相 关 信息。

### 主流日志分析工具使用

