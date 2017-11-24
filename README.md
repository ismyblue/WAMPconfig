# Windows配置php最新版开发环境（php+Apache+mysql）


## 1.所需工具/安装包
- windows: 64位
- apache: [httpd-2.4.29-o102m-x64-vc14.zip](https://www.apachehaus.com/cgi-bin/download.plx?dli=AZVFDNaVVQz8EVrtWVuB3MKVlUGR1UwRHVIp1a)
- php: [php-7.1.11-Win32-VC14-x64.zip](http://windows.php.net/downloads/releases/php-7.1.11-Win32-VC14-x64.zip)
- mysql: 
自动安装版： [mysql-installer-web-community-5.7.20.0.msi](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-web-community-5.7.20.0.msi) 
手动配置版： [mysql-5.7.20-winx64.zip](https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.20-winx64.zip)

必装插件dll：

- [VC_redist.x64.exe 2017Apache所需](https://go.microsoft.com/fwlink/?LinkId=746572)
- [vcredist_x64.exe 2013MySQL所需](https://download.microsoft.com/download/F/3/5/F3500770-8A08-488E-94B6-17A1E1DD526F/vcredist_x64.exe)

以上链接都是官网copy下来的链接，直接使用便是。如果不习惯，就自己去各个官网下载。

## 2.安装步骤

### 2.1.安装php7

解压php-7.1.11-Win32-VC14-x64.zip到D:\WAMP\

#### php配置
- 用编辑器打开D:\WAMP\php-7.1.11-Win32-VC14-x64目录下的php.ini-development复制为php.ini文件
将 ;extension_dir = "ext" 修改为 extension_dir = "E:/Php7/ext"   (注意要把前面的分号去掉)
打开(也就是把前面的分号去掉)用于连接 数据库（Mysql）的模块
extension=php_mysql.dll


### 2.2.安装Apache24

安装vc14 VC_redist.x64.exe

解压httpd-2.4.29-o102m-x64-vc14.zip到D:\WAMP\

**编辑配置文件**
进入D:\WAMP\httpd-2.4.29-o102m-x64-vc14\Apache24\conf,编辑httpd.conf
修改以下内容：

```
Define SRVROOT D:\WAMP\httpd-2.4.29-o102m-x64-vc14\Apache24"
ServerRoot "${SRVROOT}"

ServerAdmin ismyblue@163.com

//首先在apache24下新建/www

DocumentRoot "${SRVROOT}/www"
<Directory "${SRVROOT}/www">

<IfModule dir_module>
    DirectoryIndex index.php index.html index.htm
</IfModule>

/加载php模块

在#LoadModule xml2enc_module modules/mod_xml2enc.so下面添加
LOadModule php7_module "E:/Php7/php7apache2_4.dll"
AddHandler application/x-httpd-php .php
 #指定php.ini的位置（注意：需要把E:/Php7下的php.ini-development 重命名为 php.ini）
PHPIniDir "E:/Php7"

```
>打开Apache安装目录下的“bin”文件夹中的“httpd.exe”测试服务器，如果一闪而过，则说明你的配置文件有问题。如果打开一个空的命令提示符，那么恭喜你。不要关闭这个命令提示符，打开浏览器，在地址栏输入“localhost”，回车，如果显示“It works!”则说明你的服务器配置成功。

>注意：如果没有安装vc14此处会报错！

>下一步要做的就是让系统服务中增加apache的服务。
关闭上一步的命令窗口，开始----运行-----cmd，进入apapche的bin目录下，加入服务的命令为：httpd.exe -k install -n
例如：httpd.exe -k install -n "Apache24"
“服务”也会有localhost这个服务项。
卸载命令：httpd.exe -k uninstall -n "Apache24"

>启动apache有两种方式：
1、在系统服务中启动，启动后在浏览器输入localhost就会出现it works！
2、可以通过Apache24\bin文件夹下的ApacheMonitor.exe来控制这个Apache。任务栏也会有显示这个软件的很方便的管理Apache的运行。
至此，apache安装配置启动完毕。
    

### 2.3.安装Mysql

解压到D:\WAMP\mysql-5.7.20-winx64
**新建一个ini配置文件mysql.ini**
填入
<pre>
[client]
port=3306
default-character-set=utf8

[mysqld] 
# 设置为自己MYSQL的安装目录 
basedir=D:\\WAMP\mysql-5.7.20-winx64
# 设置为MYSQL的数据目录 
datadir=D:\\WAMP\\mysql-5.7.20-winx64\\data
port=3306
character_set_server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER
#开启查询缓存
explicit_defaults_for_timestamp=true
skip-grant-tables
</pre>

**自动生成data目录，进入mysql的bin目录：**
<pre>
```
	windows:
	bin\mysqld --initialize
	mysqld --initialize-insecure
	linux:
	mysqld --initialize-insecure --user=mysql 
```
</pre>
进入mysql的bin目录：输入mysqld -install
出现ervice successfully installed.就成功了

注意：运行net start mysql命令（启动MYSQL命令）
net stop mysql命令（停止MYSQL命令） 