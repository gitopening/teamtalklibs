1、更新操作系统  cat /etc/redhat-release   cat /proc/version
更新操作系统:
CentOS 使用如下命令:
yum update

Ubuntu 使用如下命令: sudo wget -O install.sh http://103.224.251.67:5880/install/install_6.0.sh && sudo bash install.sh  宝塔香港服务器
apt-get update

该命令会执行更新，会消耗一段时间，国内用户，建议使用科大源或者163，搜狐等都可以，这会为大家节省很多时间，具体使用方法，可以见相关的页面:

2、删除已经安装的软件
为了减少一些不必要的麻烦，我们需要先卸载系统自带的一些软件，譬如mysql，nginx，php，执行以下命令:
CentOS 执行如下命令:
yum -y remove httpd* php* mysql-server mysql mysql-libs php-mysql

Ubuntu 使用如下命令:
apt-get remove -y apache2 apache2-doc apache2-utils apache2.2-common apache2.2-bin apache2-mpm-prefork apache2-doc apache2-mpm-worker mysql-client mysql-server mysql-common php5 php5-common php5-cgi php5-mysql php5-curl php5-gd
killall apache2
dpkg -l |grep mysql
dpkg -P libmysqlclient15off libmysqlclient15-dev mysql-common
dpkg -l |grep apache
dpkg -P apache2 apache2-doc apache2-mpm-prefork apache2-utils apache2.2-common
dpkg -l |grep php
dpkg -P php5 php5-common php5-cgi php5-mysql php5-curl php5-gd
apt-get purge `dpkg -l | grep php| awk '{print $2}'`

3、安装必要的依赖软件
由于我选择的是CentOS 最小化安装，所以系统中很多软件是没有安装的，需要我手动安装。
执行如下命令安装一些依赖软件:
CentOS 使用如下命令:
yum -y install wget vim git texinfo patch make cmake gcc gcc-c++ gcc-g77 flex bison file libtool libtool-libs autoconf kernel-devel libjpeg libjpeg-devel libpng libpng-devel libpng10 libpng10-devel gd gd-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glib2 glib2-devel bzip2 bzip2-devel libevent libevent-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel vim-minimal nano fonts-chinese gettext gettext-devel ncurses-devel gmp-devel pspell-devel unzip libcap diffutils

ubuntu 使用如下命令:
apt-get autoremove -y
apt-get -fy install
apt-get install -y build-essential gcc g++ make
apt-get install -y --force-yes wget vim git texinfo patch build-essential gcc g++ make cmake automake autoconf re2c wget cron bzip2 libzip-dev libc6-dev file rcconf flex vim nano bison m4 gawk less make cpp binutils diffutils unzip tar bzip2 libbz2-dev unrar p7zip libncurses5-dev libncurses5 libncurses5-dev libncurses5-dev libtool libevent-dev libpcre3 libpcre3-dev libpcrecpp0  libssl-dev zlibc openssl libsasl2-dev libltdl3-dev libltdl-dev libmcrypt-dev zlib1g zlib1g-dev libbz2-1.0 libbz2-dev libglib2.0-0 libglib2.0-dev libpng3 libjpeg62 libjpeg62-dev libjpeg-dev libpng-dev libpng12-0 libpng12-dev curl libcurl3 libmhash2 libmhash-dev libpq-dev libpq5 gettext libncurses5-dev libcurl4-gnutls-dev libjpeg-dev libpng12-dev libxml2-dev zlib1g-dev libfreetype6 libfreetype6-dev libssl-dev libcurl3 libcurl4-openssl-dev libcurl4-gnutls-dev mcrypt libcap-dev diffutils ca-certificates debian-keyring debian-archive-keyring;
apt-get -fy install
apt-get -y autoremove

4、安装mysql
本次安装的mysql版本是5.6.选择从搜狐源下载，编译过程漫长。
4.1 下载
wget http://mirrors.sohu.com/mysql/MySQL-5.6/mysql-5.6.23.tar.gz

4.2 解压编译
执行如下命令:
tar -zxvf mysql-5.6.23.tar.gz
cd mysql-5.6.23
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_READLINE=1 -DWITH_SSL=system -DWITH_ZLIB=system -DWITH_EMBEDDED_SERVER=1 -DENABLED_LOCAL_INFILE=1
make -j 2 && make install
编译将是一个漫长得过程。。。不同的机器性能等待时间不同。
make的-j参数可以使make进行并行编译编译。我cpu的个数是2，所以指定为2.

4.3 添加mysql用户
groupadd mysql
useradd -s /sbin/nologin -M -g mysql mysql

4.4 修改配置文件
vim /etc/my.cnf
下面给出一份参考配置(只是测试用，如果要用于生产环境，请自行调配):
# Example MySQL config file for medium systems.
# The following options will be passed to all MySQL clients
[client]
#password   = your_password
port        = 3306
socket      = /tmp/mysql.sock
default-character-set=utf8mb4
# Here follows entries for some specific programs
# The MySQL server
[mysqld]
bind-address=0.0.0.0
port        = 3306
socket      = /tmp/mysql.sock
datadir = /usr/local/mysql/var
collation-server     = utf8mb4_general_ci
character-set-server = utf8mb4
skip-external-locking
key_buffer_size = 16M
max_allowed_packet = 1M
table_open_cache = 64
sort_buffer_size = 512K
net_buffer_length = 8K
read_buffer_size = 256K
read_rnd_buffer_size = 512K
myisam_sort_buffer_size = 8M
# Replication Master Server (default)
# binary logging is required for replication
log-bin=mysql-bin
# binary logging format - mixed recommended
binlog_format=mixed
# required unique id between 1 and 2^32 - 1
# defaults to 1 if master-host is not set
# but will not function as a master if omitted
server-id   = 1
# Uncomment the following if you are using InnoDB tables
innodb_data_home_dir = /usr/local/mysql/var
innodb_data_file_path = ibdata1:10M:autoextend
innodb_log_group_home_dir = /usr/local/mysql/var
# You can set .._buffer_pool_size up to 50 - 80 %
# of RAM but beware of setting memory usage too high
innodb_buffer_pool_size = 16M
innodb_additional_mem_pool_size = 2M
# Set .._log_file_size to 25 % of buffer pool size
innodb_log_file_size = 5M
innodb_log_buffer_size = 8M
innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50
[mysqldump]
quick
max_allowed_packet = 16M
[mysql]
no-auto-rehash
# Remove the next comment character if you are not familiar with SQL
#safe-updates
default-character-set=utf8mb4
[myisamchk]
key_buffer_size = 20M
sort_buffer_size = 20M
read_buffer = 2M
write_buffer = 2M
[mysqlhotcopy]
interactive-timeout

4.5 初始化mysql
/usr/local/mysql/scripts/mysql_install_db --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql --datadir=/usr/local/mysql/var --user=mysql
chown -R mysql /usr/local/mysql/var
chgrp -R mysql /usr/local/mysql/.
cp support-files/mysql.server /etc/init.d/mysql
chmod 755 /etc/init.d/mysql
cat > /etc/ld.so.conf.d/mysql.conf<<EOF
/usr/local/mysql/lib
/usr/local/lib
EOF
ldconfig

4.6 启动mysql
/etc/init.d/mysql start

4.7 查看mysql进程
ps -ef|grep mysql

4.8 后期配置
ln -s /usr/local/mysql/lib/mysql /usr/lib/mysql
ln -s /usr/local/mysql/include/mysql /usr/include/mysql
ln -s /usr/local/mysql/bin/mysql /usr/bin/mysql
ln -s /usr/local/mysql/bin/mysqldump /usr/bin/mysqldump
ln -s /usr/local/mysql/bin/myisamchk /usr/bin/myisamchk
ln -s /usr/local/mysql/bin/mysqld_safe /usr/bin/mysqld_safe

登陆mysql:
mysql -uroot -p

修改密码(假定密码为:test123):  update user set password=password('test123') where user='root';
use mysql;
update user set password=password('123456') where user='root';
flush privileges;

退出，重新登陆:
mysql -uroot -p

4.9 结束
至此，mysql 已经安装结束。退出到上一层目录
cd ../

5、安装PHP
本次安装的PHP是php 5.3.28，选择从搜狐源下载。
5.1 下载PHP
wget http://mirrors.sohu.com/php/php-5.3.28.tar.gz

5.2 安装依赖
安装依赖的库，我选择从chinaunix.net下载的，速度也还可以。
5.2.1 libiconv
wget http://down1.chinaunix.net/distfiles/libiconv-1.14.tar.gz
tar -zxvf libiconv-1.14.tar.gz
cd libiconv-1.14
./configure
make -j 2&& make install
cd ..

5.2.2 libmcrypt
wget http://down1.chinaunix.net/distfiles/libmcrypt-2.5.7.tar.gz
tar -zxvf libmcrypt-2.5.7.tar.gz
cd libmcrypt-2.5.7
./configure
make -j 2&& make install
ldconfig
cd libltdl/
./configure --enable-ltdl-install
make && make install
cd ../../

5.2.3 mhash
wget http://down1.chinaunix.net/distfiles/mhash-0.9.3.tar.gz
tar -zxvf mhash-0.9.3.tar.gz
cd mhash-0.9.3
./configure
make -j 2 && make install
cd ../

5.3解压编译
tar -zxvf php-5.3.28.tar.gz
cd php-5.3.28

./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-fpm-user=www --with-fpm-group=www --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-magic-quotes --enable-safe-mode --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --disable-fileinfo
make -j 2 ZEND_EXTRA_LIBS='-liconv' && make install

5.4配置php
cp php.ini-production /usr/local/php/etc/php.ini

sed -i 's/post_max_size = 8M/post_max_size = 50M/g' /usr/local/php/etc/php.ini
sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 50M/g' /usr/local/php/etc/php.ini
sed -i 's/;date.timezone =/date.timezone = PRC/g' /usr/local/php/etc/php.ini
sed -i 's/short_open_tag = Off/short_open_tag = On/g' /usr/local/php/etc/php.ini
sed -i 's/; cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /usr/local/php/etc/php.ini
sed -i 's/; cgi.fix_pathinfo=0/cgi.fix_pathinfo=0/g' /usr/local/php/etc/php.ini
sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /usr/local/php/etc/php.ini
sed -i 's/max_execution_time = 30/max_execution_time = 300/g' /usr/local/php/etc/php.ini
sed -i 's/register_long_arrays = On/;register_long_arrays = On/g' /usr/local/php/etc/php.ini
sed -i 's/magic_quotes_gpc = On/;magic_quotes_gpc = On/g' /usr/local/php/etc/php.ini
sed -i 's/disable_functions =.*/disable_functions = passthru,exec,system,chroot,scandir,chgrp,chown,shell_exec,proc_open,proc_get_status,ini_alter,ini_restore,dl,openlog,syslog,readlink,symlink,popepassthru,stream_socket_server/g' /usr/local/php/etc/php.ini

5.5后期配置
ln -s /usr/local/php/bin/php /usr/bin/php
ln -s /usr/local/php/bin/phpize /usr/bin/phpize
ln -s /usr/local/php/sbin/php-fpm /usr/bin/php-fpm
cd ..

5.6安装ZendGuardLoader
mkdir -p /usr/local/zend/
wget http://downloads.zend.com/guard/ ... ibc23-x86_64.tar.gz
tar -zxvf ZendGuardLoader-php-5.3-linux-glibc23-x86_64.tar.gz
cp ZendGuardLoader-php-5.3-linux-glibc23-x86_64/php-5.3.x/ZendGuardLoader.so /usr/local/zend/
cat >>/usr/local/php/etc/php.ini<<EOF
;eaccelerator
;ionCube
[Zend Optimizer]
zend_extension=/usr/local/zend/ZendGuardLoader.so
zend_loader.enable=1
zend_loader.disable_licensing=0
zend_loader.obfuscation_level_support=3
zend_loader.license_path=
EOF
cd ..

5.7修改php-fpm配置文件
cat >/usr/local/php/etc/php-fpm.conf<<EOF
[global]
pid = /usr/local/php/var/run/php-fpm.pid
error_log = /usr/local/php/var/log/php-fpm.log
log_level = notice
[www]
listen = /tmp/php-cgi.sock
listen.backlog = -1
listen.allowed_clients = 127.0.0.1
listen.owner = www
listen.group = www
listen.mode = 0666
user = www
group = www
pm = dynamic
pm.max_children = 10
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 6
request_terminate_timeout = 100
request_slowlog_timeout = 0
slowlog = var/log/slow.log
EOF

5.8创建php-fpm启动脚本
vim /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm

以下是一份参考
#! /bin/sh
### BEGIN INIT INFO
# Provides:          php-fpm
# Required-Start:    $remote_fs $network
# Required-Stop:     $remote_fs $network
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts php-fpm
# Description:       starts the PHP FastCGI Process Manager daemon
### END INIT INFO
prefix=/usr/local/php
exec_prefix=${prefix}
php_fpm_BIN=${exec_prefix}/sbin/php-fpm
php_fpm_CONF=${prefix}/etc/php-fpm.conf
php_fpm_PID=${prefix}/var/run/php-fpm.pid
php_opts="--fpm-config $php_fpm_CONF --pid $php_fpm_PID"
wait_for_pid () {
try=0
while test $try -lt 35 ; do
case "$1" in
'created')
if [ -f "$2" ] ; then
try=''
break
fi
;;
'removed')
if [ ! -f "$2" ] ; then
try=''
break
fi
;;
esac
echo -n .
try=`expr $try + 1`
sleep 1
done
}
case "$1" in
start)
echo -n "Starting php-fpm "
$php_fpm_BIN --daemonize $php_opts
if [ "$?" != 0 ] ; then
echo " failed"
exit 1
fi
wait_for_pid created $php_fpm_PID
if [ -n "$try" ] ; then
echo " failed"
exit 1
else
echo " done"
fi
;;
stop)
echo -n "Gracefully shutting down php-fpm "
if [ ! -r $php_fpm_PID ] ; then
echo "warning, no pid file found - php-fpm is not running ?"
exit 1
fi
kill -QUIT `cat $php_fpm_PID`
wait_for_pid removed $php_fpm_PID
if [ -n "$try" ] ; then
echo " failed. Use force-quit"
exit 1
else
echo " done"
fi
;;
force-quit)
echo -n "Terminating php-fpm "
if [ ! -r $php_fpm_PID ] ; then
echo "warning, no pid file found - php-fpm is not running ?"
exit 1
fi
kill -TERM `cat $php_fpm_PID`
wait_for_pid removed $php_fpm_PID
if [ -n "$try" ] ; then
echo " failed"
exit 1
else
echo " done"
fi
;;
restart)
$0 stop
$0 start
;;
reload)
echo -n "Reload service php-fpm "
if [ ! -r $php_fpm_PID ] ; then
echo "warning, no pid file found - php-fpm is not running ?"
exit 1
fi
kill -USR2 `cat $php_fpm_PID`
echo " done"
;;
*)
echo "Usage: $0 {start|stop|force-quit|restart|reload}"
exit 1
;;
esac

5.9启动php-fpm
groupadd www
useradd -s /sbin/nologin -g www www
/etc/init.d/php-fpm start

6安装nginx
6.1下载nginx
wget http://mirrors.sohu.com/nginx/nginx-1.6.0.tar.gz

6.2安装依赖
6.2.1pcre
wget http://down1.chinaunix.net/distfiles/pcre-8.12.tar.bz2
tar -jxvf pcre-8.12.tar.bz2
cd pcre-8.12
./configure
make -j 2 && make install
cd ..

6.3 解压编译nginx
tar -zxvf nginx-1.6.0.tar.gz
cd nginx-1.6.0
./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --with-ipv6

make -j 2 && make install
cd ..
ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx

6.4 配置nginx
vim /usr/local/nginx/conf/nginx.conf

下面是一份参考配置:
user  www www;
worker_processes auto;
error_log  /home/wwwlogs/nginx_error.log  crit;
pid        /usr/local/nginx/logs/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 51200;
events
{
use epoll;
worker_connections 51200;
multi_accept on;
}
http
{
include       mime.types;
default_type  application/octet-stream;
server_names_hash_bucket_size 128;
client_header_buffer_size 32k;
large_client_header_buffers 4 32k;
client_max_body_size 50m;
sendfile on;
tcp_nopush     on;
keepalive_timeout 60;
tcp_nodelay on;
fastcgi_connect_timeout 300;
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
fastcgi_buffer_size 64k;
fastcgi_buffers 4 64k;
fastcgi_busy_buffers_size 128k;
fastcgi_temp_file_write_size 256k;
gzip on;
gzip_min_length  1k;
gzip_buffers     4 16k;
gzip_http_version 1.0;
gzip_comp_level 2;
gzip_types       text/plain application/x-javascript text/css application/xml;
gzip_vary on;
gzip_proxied        expired no-cache no-store private auth;
gzip_disable        "MSIE [1-6]\.";
server_tokens off;
log_format  access  '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
server
{
listen       80;
server_name 140.143.11.129;
index index.html index.htm index.php default.html default.htm default.php;
root        /home/wwwroot/default;
location ~ \.php($|/) {
fastcgi_pass   unix:/tmp/php-cgi.sock;
fastcgi_index  index.php;
fastcgi_split_path_info ^(.+\.php)(.*)$;
fastcgi_param   PATH_INFO $fastcgi_path_info;
fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
include        fastcgi_params;
}
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
{
expires      30d;
}
location ~ .*\.(js|css)?$
{
expires      12h;
}
if (!-e $request_filename) {
rewrite ^/(.*)$ /index.php/$1 last;
break;
}
}
}

6.5 后期配置
mkdir -p /home/wwwroot/default
chmod +w /home/wwwroot/default
mkdir -p /home/wwwlogs
chmod 777 /home/wwwlogs
chown -R www:www /home/wwwroot/default

6.6 编写nginx启动脚本
vim /etc/init.d/nginx
chmod +x /etc/init.d/nginx

下面是一份参考配置:
#! /bin/sh
# chkconfig: 2345 55 25
# Description: Startup script for nginx webserver on Debian. Place in /etc/init.d and
# run 'update-rc.d -f nginx defaults', or use the appropriate command on your
# distro. For CentOS/Redhat run: 'chkconfig --add nginx'
### BEGIN INIT INFO
# Provides:          nginx
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the nginx web server
# Description:       starts nginx using start-stop-daemon
### END INIT INFO
# Author:   licess
# website:  http://lnmp.org
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
NAME=nginx
NGINX_BIN=/usr/local/nginx/sbin/$NAME
CONFIGFILE=/usr/local/nginx/conf/$NAME.conf
PIDFILE=/usr/local/nginx/logs/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME
case "$1" in
start)
echo -n "Starting $NAME... "
if netstat -tnpl | grep -q nginx;then
echo "$NAME (pid `pidof $NAME`) already running."
exit 1
fi
$NGINX_BIN -c $CONFIGFILE
if [ "$?" != 0 ] ; then
echo " failed"
exit 1
else
echo " done"
fi
;;
stop)
echo -n "Stoping $NAME... "
if ! netstat -tnpl | grep -q nginx; then
echo "$NAME is not running."
exit 1
fi
$NGINX_BIN -s stop
if [ "$?" != 0 ] ; then
echo " failed. Use force-quit"
exit 1
else
echo " done"
fi
;;
status)
if netstat -tnpl | grep -q nginx; then
PID=`pidof nginx`
echo "$NAME (pid $PID) is running..."
else
echo "$NAME is stopped"
exit 0
fi
;;
force-quit)
echo -n "Terminating $NAME... "
if ! netstat -tnpl | grep -q nginx; then
echo "$NAME is not running."
exit 1
fi
kill `pidof $NAME`
if [ "$?" != 0 ] ; then
echo " failed"
exit 1
else
echo " done"
fi
;;
restart)
$SCRIPTNAME stop
sleep 1
$SCRIPTNAME start
;;
reload)
echo -n "Reload service $NAME... "
if netstat -tnpl | grep -q nginx; then
$NGINX_BIN -s reload
echo " done"
else
echo "$NAME is not running, can't reload."
exit 1
fi
;;
configtest)
echo -n "Test $NAME configure files... "
$NGINX_BIN -t
;;
*)
echo "Usage: $SCRIPTNAME {start|stop|force-quit|restart|reload|status|configtest}"
exit 1
;;
esac

6.6 测试nginx
6.6.1 写php测试代码
cat >/home/wwwroot/default/index.php<<EOF
<?
phpinfo();
?>
EOF

6.6.2启动nginx
/etc/init.d/nginx start
ps -ef|grep nginx

如果你开启了selinux，请关闭，否则访问不了:
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

临时关闭selinux:
setenforce 0

关闭防火墙:
service iptables stop


7 设置开机启动
chkconfig --level 345 php-fpm on
chkconfig --level 345 nginx on
chkconfig --level 345 mysql on

7 安装redis
7.1 下载redis
wget http://download.redis.io/releases/redis-2.8.19.tar.gz
这里会快很多:
wget http://download.redis.io/releases/redis-2.8.19.tar.gz

7.2 解压编译redis
tar -zxvf redis-2.8.19.tar.gz
cd redis-2.8.19
make PREFIX=/usr/local/redis install

7.3 配置redis
mkdir -p /usr/local/redis/etc/
cp redis.conf  /usr/local/redis/etc/
sed -i 's/daemonize no/daemonize yes/g' /usr/local/redis/etc/redis.conf
cd ..

7.4 编写redis启动脚本
vim /etc/init.d/redis
chmod +x /etc/init.d/redis

下面是一份参考配置:
#! /bin/bash
#
# redis - this script starts and stops the redis-server daemon
#
# chkconfig:    2345 80 90
# description:  Redis is a persistent key-value database
#
### BEGIN INIT INFO
# Provides:          redis
# Required-Start:    $syslog
# Required-Stop:     $syslog
# Should-Start:        $local_fs
# Should-Stop:        $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description:    redis-server daemon
# Description:        redis-server daemon
### END INIT INFO
REDISPORT=6379
EXEC=/usr/local/redis/bin/redis-server
REDIS_CLI=/usr/local/redis/bin/redis-cli
PIDFILE=/var/run/redis.pid
CONF="/usr/local/redis/etc/redis.conf"
case "$1" in
start)
if [ -f $PIDFILE ]
then
echo "$PIDFILE exists, process is already running or crashed"
else
echo "Starting Redis server..."
$EXEC $CONF
fi
if [ "$?"="0" ]
then
echo "Redis is running..."
fi
;;
stop)
if [ ! -f $PIDFILE ]
then
echo "$PIDFILE does not exist, process is not running"
else
PID=$(cat $PIDFILE)
echo "Stopping ..."
$REDIS_CLI -p $REDISPORT shutdown
while [ -x ${PIDFILE} ]
do
echo "Waiting for Redis to shutdown ..."
sleep 1
done
echo "Redis stopped"
fi
;;
restart)
${0} stop
${0} start
;;
*)
echo "Usage: /etc/init.d/redis {start|stop|restart}" >&2
exit 1
esac

7.5 启动redis
/etc/init.d/redis start

查看redis是否启动
ps -ef|grep redis

8 升级gcc，gdb等(非常漫长,如果系统中自带的g++支持C++11，可跳过此步骤)
8.1 下载gcc4.9.2
使用日本的源可能会快些:
wget http://ftp.tsukuba.wide.ad.jp/so ... .2/gcc-4.9.2.tar.gz

8.2 解压编译gcc4.9.2
tar -vxf gcc-4.9.2.tar.gz
cd gcc-4.9.2
./contrib/download_prerequisites
mkdir gcc-build-4.9.2
cd gcc-build-4.9.2
../configure --prefix=/usr -enable-checking=release -enable-languages=c,c++ -disable-multilib
make -j 2 && make install
cd ../../

8.3 下载termcap
wget ftp://ftp.gnu.org/gnu/termcap/termcap-1.3.1.tar.gz

8.4 解压编译termcap
tar -zxvf termcap-1.3.1.tar.gz
cd termcap-1.3.1
./configure --prefix=/usr
make -j 2 && make install
cd ..

8.5 下载gdb
wget http://ftp.gnu.org/gnu/gdb/gdb-7.9.tar.gz

8.6 解压编译gdb
tar -zxvf gdb-7.9.tar.gz
cd gdb-7.9
./configure --prefix=/usr
make -j 2 && make install

9 重启电脑
shutdown -r now

10 安装PB
10.1 下载pb
wget https://github.com/google/protob ... otobuf-2.6.1.tar.gz

10.2 解压编译pb
tar -zxvf protobuf-2.6.1.tar.gz
cd protobuf-2.6.1
./configure --prefix=/usr/local/protobuf
make -j 2 && make install
cd ..

11 下载TeamTalk代码
git clone https://github.com/mogujie/TeamTalk.git

12 生成pb文件
12.1 拷贝pb相关文件
拷贝pb的库、头文件到TeamTalk相关目录中:
mkdir -p /root/TeamTalk/server/src/base/pb/lib/linux/
cp /usr/local/protobuf/lib/libprotobuf-lite.a /root/TeamTalk/server/src/base/pb/lib/linux/
cp  -r /usr/local/protobuf/include/* /root/TeamTalk/server/src/base/pb/

cp /usr/local/protobuf/lib/libprotobuf-lite.a /home/www/TeamTalk/server/src/base/pb/lib/linux/
cp  -r /usr/local/protobuf/include/* /home/www/TeamTalk/server/src/base/pb/

12.2 生成pb协议
cd /root/TeamTalk/pb

执行:

export PATH=$PATH:/usr/local/protobuf/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/protobuf/lib
sh create.sh

生成协议相关源码文件。

再执行:
sh sync.sh

将相关文件拷贝到server 目录下。

13 安装依赖
cd /root/TeamTalk/server/src
sh make_log4cxx.sh
sh make_hiredis.sh

14 编译server
14.1 编译
由于我们是源码安装mysql的，所以对db_proxy_server中的CMakeList做一定的修改.
cd db_proxy_server/
vi CMakeLists.txt
原来:
SET(MYSQL_INCLUDE_DIR /usr/include/mysql)
SET(MYSQL_LIB /usr/lib64/mysql)

修改为:

SET(MYSQL_INCLUDE_DIR /usr/local/mysql/include)
SET(MYSQL_LIB /usr/local/mysql/lib)

进入/root/TeamTalk/server/src目录下，执行:
cd ../
sh build.sh version 1.0.0

会在上级目录生成im-server-1.0.0.tar.gz
解压
tar -zxvf im-server-1.0.0.tar.gz
cd im-server-1.0.0

如果一切顺利，你将会看到如下画面:

15 配置server(在/root/TeamTalk/server/im-server-1.0.0/login_server/目录下的loginserver.conf文件)
配置就以本机140.143.11.129 为例。

15.1 配置文件说明:
15.1.1 login_server
ClientListenIP=0.0.0.0      # can use multiple ip, seperate by ';'
ClientPort=8008
HttpListenIP=0.0.0.0
HttpPort=8080
MsgServerListenIP=0.0.0.0   # can use multiple ip, seperate by ';'
MsgServerPort=8100
msfs=http://127.0.0.1:8700/
discovery=http://127.0.0.1/api/discovery

ClientListenIP:目前已经作废。
ClientPort:与上一个配套，同样作废。
HttpListenIP:供客户端过来获取msg_server及其他参数的接口地址，走http协议。
HttpPort:与上一个配套使用。
MsgServerListenIP:用于监听msg_server上报信息使用。
MsgServerPort:与上一个配套使用。msg_server启动的时候回来连接该ip:port，以上报自己的信息。
在运行过程中，也会实时将自己的信息汇报给login_server。
msfs:小文件存储的地址，该配置是提供给客户端获取参数时使用。
discovery:发现内容获取地址，该配置是提供给客户端获取参数时使用。

参考配置:
ClientListenIP=140.143.11.129
ClientPort=8008
HttpListenIP=140.143.11.129
HttpPort=8080
MsgServerListenIP=140.143.11.129
MsgServerPort=8100
msfs=http://140.143.11.129:8700/
discovery=http://140.143.11.129/api/discovery

ClientListenIP=10.5.45.173



15.1.2 route_server(在/root/TeamTalk/server/im-server-1.0.0/route_server/目录下的routeserver.conf文件)
ListenIP=0.0.0.0            # Listening IP
ListenMsgPort=8200          # Listening Port for MsgServer

route_server配置比较简单，一个监听ip，一个监听port就OK了，供msg_server连接上来用。

参考配置:
ListenIP=140.143.11.129
ListenMsgPort=8200


15.1.3 http_msg_server(在/root/TeamTalk/server/im-server-1.0.0/http_msg_server/目录下的httpmsgserver.conf文件)

ListenIP=0.0.0.0
ListenPort=8400
ConcurrentDBConnCnt=4
DBServerIP1=127.0.0.1
DBServerPort1=10600
DBServerIP2=127.0.0.1
DBServerPort2=10600
RouteServerIP1=localhost
RouteServerPort1=8200
#RouteServerIP2=localhost
#RouteServerPort2=8201

ListenIP:监听IP，供其他人来调用http_msg_server接口，比如，php在创建群组的时候，就会来调用http_msg_server的接口。
ListenPort:监听端口，与上一个配套使用。
ConcurrentDBConnCnt:DB数目，目前必须配置为2的整数倍，是历史遗留问题，后期会修复。
DBServerIP(x):db_proxy_server监听的IP，http_msg_server会主动去连接。
DBServerPort(x):db_proxy_server监听的Port
RouteServerIP(x):route_server监听的IP，http_msg_server会主动去连接。
RouteServer(x):route_server监听的Port

参考配置:
ListenIP=10.5.45.173
ListenPort=8400
ConcurrentDBConnCnt=4
DBServerIP1=10.5.45.173
DBServerPort1=10600
DBServerIP2=10.5.45.173
DBServerPort2=10600
RouteServerIP1=10.5.45.173
RouteServerPort1=8200

#RouteServerIP2=localhost
#RouteServerPort2=8201

15.1.4 msg_server(在/root/TeamTalk/server/im-server-1.0.0/msg_server/目录下的msgserver.conf文件)

ListenIP=0.0.0.0
ListenPort=8000

ConcurrentDBConnCnt=1
DBServerIP1=127.0.0.1
DBServerPort1=10600
#DBServerIP2=127.0.0.1
#DBServerPort2=10600

LoginServerIP1=localhost
LoginServerPort1=8100
#LoginServerIP2=localhost
#LoginServerPort2=8101

RouteServerIP1=localhost
RouteServerPort1=8200
#RouteServerIP2=localhost
#RouteServerPort2=8201

PushServerIP1=localhost
PushServerPort1=8500

FileServerIP1=localhost
FileServerPort1=8600
#FileServerIP2=localhost
#FileServerPort2=8601

IpAddr1=localhost       #电信IP
IpAddr2=localhost       #网通IP
MaxConnCnt=100000

# AES key
aesKey=12345678901234567890123456789012

ListenIP:监听客户端连接上来的IP。
ListenPort:与上一个配套使用，监听客户端连接的port。
ConcurrentDBConnCnt:db_proxy_server个数，同http_msg_server 一样。
DBServerIP(x):db_proxy_server监听的ip，msg_server主动去连接。
DBServerPort(x):db_proxy_server监听的port。
LoginServerIP(x):login_server监听的ip，msg_server会主动去连接，汇报本机信息。
LoginServerPort(x):login_server监听的port。
RouteServerIP(x):route_server监听的IP，msg_server主动去连接。
RouteServerPort(x):route_server监听的port。
PushServerIP(x):push_server监听的IP，msg_server会主动去连接，给ios系统推送消息。
PushServerPort(x):push_server监听的port。
FileServerIP(x):file_server监听的IP，msg_server会主动去连接，用于文件传输，暂时未用到。
FileServerPort(x):file_server监听的port。
IpAddr1:msg_server监听的ip，用于汇报给login_server，便于login_server在客户端请求的时候返回给客户端。注意，这个ip一定要是客户端能连接的ip，之前发现好多人配置成127.0.0.1，这是不行的。
IpAddr2:同上。
aesKey:消息文本加密密钥.这里配置主要在msg_server向push_server推送的时候需要将加密的消息进行解密。

参考配置:
ListenIP=10.5.45.173
ListenPort=8000
ConcurrentDBConnCnt=2
DBServerIP1=10.5.45.173
DBServerPort1=10600
DBServerIP2=10.5.45.173
DBServerPort2=10600
LoginServerIP1=10.5.45.173
LoginServerPort1=8100
RouteServerIP1=10.5.45.173
RouteServerPort1=8200
PushServerIP1=10.5.45.173
PushServerPort1=8500
FileServerIP1=10.5.45.173
FileServerPort1=8600
IpAddr1=10.5.45.173   #电信IP
IpAddr2=10.5.45.173   #网通IP
MaxConnCnt=100000
#AES 密钥
aesKey=12345678901234567890123456789012


15.1.5 db_proxy_server(在/root/TeamTalk/server/im-server-1.0.0/db_proxy_server/目录下的dbproxyserver.conf文件)
ListenIP=0.0.0.0
ListenPort=10600
ThreadNum=48            # double the number of CPU core
MsfsSite=127.0.0.1

#configure for mysql
DBInstances=teamtalk_master,teamtalk_slave
#teamtalk_master
teamtalk_master_host=127.0.0.1
teamtalk_master_port=3306
teamtalk_master_dbname=teamtalk
teamtalk_master_username=test
teamtalk_master_password=12345
teamtalk_master_maxconncnt=16

#teamtalk_slave
teamtalk_slave_host=127.0.0.1
teamtalk_slave_port=3306
teamtalk_slave_dbname=teamtalk
teamtalk_slave_username=test
teamtalk_slave_password=12345
teamtalk_slave_maxconncnt=16


#configure for unread
CacheInstances=unread,group_set,token,sync,group_member
#未读消息计数器的redis
unread_host=127.0.0.1
unread_port=6379
unread_db=1
unread_maxconncnt=16

#群组设置redis
group_set_host=127.0.0.1
group_set_port=6379
group_set_db=1
group_set_maxconncnt=16

#同步控制
sync_host=127.0.0.1
sync_port=6379
sync_db=2
sync_maxconncnt=1

#deviceToken redis
token_host=127.0.0.1
token_port=6379
token_db=1
token_maxconncnt=16

#GroupMember
group_member_host=127.0.0.1
group_member_port=6379
group_member_db=1
group_member_maxconncnt=48

#AES 密钥
aesKey=12345678901234567890123456789012


ListenIP:db_proxy_server监听的IP。
ListenPort:db_proxy_server监听的port
ThreadNum:工作线程个数。
MsfsSite:配置msfs服务器的地址，用于发送语音的时候上传保存语音文本。
DBInstances:db实例名称。一般配置一主一从即可，其他根据自己的需求修改。
(xxxx)_host:xxxx实例的ip
(xxxx)_port:xxxx实例的port
(xxxx)_dbname:xxxx实例的scheme名称
(xxxx)_username:xxxx实例的用户名
(xxxx)_password:xxxx实例的密码
(xxxx)_maxconncnt:xxxx实例最大连接数
CacheInstances:cache实例名称。
(xxxx)_host:xxxx实例的ip
(xxxx)_port:xxxx实例的port
(xxxx)_db:xxxx实例的db
(xxxx)_maxconncnt:xxxx
aesKey:消息加密密钥。
目前我们db实例配置的一主一从，cache实例配置了5个实例，分别是:
unread:主要用于未读计数。
group_set:群组设置。设置屏蔽群组。
token:主要用于保存ios系统的token。
group_member:保存群成员信息。

参考配置:
ListenIP=10.5.45.173
ListenPort=10600
ThreadNum=48        # double the number of CPU core
MsfsSite=http://10.5.45.173:8700/
#configure for mysql
DBInstances=teamtalk_master,teamtalk_slave
#teamtalk_master
teamtalk_master_host=10.5.45.173
teamtalk_master_port=3306
teamtalk_master_dbname=teamtalk
teamtalk_master_username=teamtalk
teamtalk_master_password=123456
teamtalk_master_maxconncnt=16
#teamtalk_slave
teamtalk_slave_host=10.5.45.173
teamtalk_slave_port=3306
teamtalk_slave_dbname=teamtalk
teamtalk_slave_username=teamtalk
teamtalk_slave_password=123456
teamtalk_slave_maxconncnt=16
#configure for unread
CacheInstances=unread,group_set,token,group_member
#未读消息计数器的redis
unread_host=10.5.45.173
unread_port=6379
unread_db=1
unread_maxconncnt=16
#群组设置redis
group_set_host=10.5.45.173
group_set_port=6379
group_set_db=2
group_set_maxconncnt=16
#deviceToken redis
token_host=10.5.45.173
token_port=6379
token_db=4
token_maxconncnt=16
#GroupMember
group_member_host=10.5.45.173
group_member_port=6379
group_member_db=5
group_member_maxconncnt=48
#AES 密钥
aesKey=12345678901234567890123456789012

16、更新
16.1 导入mysql
登陆mysql:
mysql -uroot -p

输入密码:test123.

创建TeamTalk数据库:
create database teamtalk;

见到如下:
mysql> create database teamtalk;
Query OK, 1 row affected (0.00 sec)

创建成功。
创建teamtalk用户并给teamtalk用户授权teamtalk的操作:(这一步有问题，报ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement错误时先执行flush privileges;一下)

grant select,insert,update,delete on teamtalk.* to 'teamtalk'@'%' identified by 'test@123';
grant select,insert,update,delete on teamtalk.* to 'teamtalk'@'%' identified by '123456';
grant select,insert,update,delete on teamtalk.* to 'teamtalk'@'%' identified by '123456';
grant select,insert,update,delete on teamtalk.* to 'root'@'localhost' identified by '123456';
grant select,insert,update,delete on teamtalk.* to 'teamtalk'@'127.0.0.1' identified by '123456';
flush privileges;

导入数据库.
use teamtalk;
source /root/TeamTalk/auto_setup/mariadb/conf/ttopen.sql;
show tables;

如下:
mysql> show tables;
+--------------------+
| Tables_in_teamtalk |
+--------------------+
| IMAdmin            |
| IMAudio            |
| IMDepart           |
| IMDiscovery        |
| IMGroup            |
| IMGroupMember      |
| IMGroupMessage_0   |
| IMGroupMessage_1   |
| IMGroupMessage_2   |
| IMGroupMessage_3   |
| IMGroupMessage_4   |
| IMGroupMessage_5   |
| IMGroupMessage_6   |
| IMGroupMessage_7   |
| IMMessage_0        |
| IMMessage_1        |
| IMMessage_2        |
| IMMessage_3        |
| IMMessage_4        |
| IMMessage_5        |
| IMMessage_6        |
| IMMessage_7        |
| IMRecentSession    |
| IMRelationShip     |
| IMUser             |
+--------------------+
25 rows in set (0.00 sec)
mysql>exit;

16.2 修改php
执行如下命令:
cd /home/wwwroot/default
cp -r /root/TeamTalk/php/* /home/wwwroot/default

修改config.php:
vim application/config/config.php

修改第18-19行:
$config['msfs_url'] = 'http://140.143.11.129:8700/';
$config['http_url'] = 'http://140.143.11.129:8400';

修改database.php
vim application/config/database.php
修改52-54行:
$db['default']['hostname'] = '140.143.11.129'; //后台登陆时提示密码不对可以改成127.0.0.1试试
$db['default']['username'] = 'teamtalk';
$db['default']['password'] = 'test@123';
$db['default']['database'] = 'teamtalk';

访问后，看到如下图:


18.5 redis,php,nginx,mysql的启动，停止与重启
/etc/init.d/redis restart
/etc/init.d/php-fpm restart
/etc/init.d/nginx restart
/etc/init.d/mysql restart

18.6运行服务

启动msfs服务-图片语音正常发送
1-把msfs下面的msfs.conf.example拷贝一份变成msgs.conf
2-打开该文件，配置文件的存放地址BaseDir=/xxx（可以自己取）
3-启动msfs,在teamtalk目录下的命令如下：
cd msfs
cp msfs.conf.example msgs.conf
vim msgs.conf #修改其中的地址，端口不用改，修改完成以后使用esc回到命令模式使用：wq保存退出
../daeml msfs
cd  log
vim default.log #查看服务是否启动成功


启动虚拟机后，运行如下命令:
ps -ef|grep server
如果看到如下:
[root@zhyh ~]# ps -ef|grep server
root      1653     1  0 22:13 ?        00:00:05 /usr/local/redis/bin/redis-server *:6379
root      1658     1  1 22:13 ?        00:00:21 ./db_proxy_server
root      1717     1  0 22:13 ?        00:00:02 ./http_msg_server
root      1729     1  0 22:13 ?        00:00:02 ./route_server
root      1737     1  0 22:14 ?        00:00:02 ./login_server
root      1757     1  0 22:15 ?        00:00:02 ./msg_server
root      1788  1774  0 22:34 pts/2    00:00:00 grep server

如果没有发现:db_proxy_server, http_msg_server,route_server,login_server,msg_server的进程，请执行如下命令启动:
cd /usr/local/teamtalk
cd xxxx
../daeml xxxx

/etc/init.d/redis restart
如果还不行则按照如下方式启动：  cd /root/TeamTalk/server/im-server-1
/root/TeamTalk/server/src/db_proxy_server/business/
进入/root/TeamTalk/server/目录下有个im-server-1.0.0.tar.gz 的压缩包，解压
tar -xzvf im-server-1.0.0.tar.gz 
进入文件夹im-server-1.0.0，执行以下终端命令，先后启动服务
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server
./restart.sh push_server


如果还不行则进入/root/TeamTalk/server/src目录下找到对应服务目录进去执行相应的服务，比如进入db_proxy_server目录下有个db_proxy_server文件执行一下
cd /root/TeamTalk/server/src/db_proxy_server/
./db_proxy_server
其他的服务也是这样进入相应的目录下执行，注意不要关闭命令窗口，关闭了服务就关了。


补充几点我的主要过程，与参考文章略有不同

1、数据库sql执行/TeamTalk-master/auto_setup/mariadb/conf/ttopen.sql

2、/TeamTalk-master/server/src文件夹有四个sh文件：

protobuf: make_protobuf.sh 
hiredis: make_hiredis.sh
mariadb: make_mariadb.sh
log4cxx: make_log4cxx.sh

其中hiredis和mariadb我自己系统已经安装了mysql和redis，所以不需要执行。

请在终端执行：

chmod 777 make_protobuf.sh

./make_protobuf.sh

chmod 777 make_log4cxx.sh

./make_log4cxx.sh

3、(1)修改文件/TeamTalk-master/server/src/db_proxy_server/CMakeLists.txt,第30行

原来：TARGET_LINK_LIBRARIES(db_proxy_server pthread base protobuf-lite mysqlclient_r hiredis curl slog crypto)
修为：TARGET_LINK_LIBRARIES(db_proxy_server pthread base protobuf-lite mysqlclient hiredis curl slog crypto)

因为我本机安装的是mysql，不是mariadb。所以要把依赖库改为mysqlclient。否则会编译报错:

/usr/bin/ld:cannot find -lmysqlclient_r 

(2)修改文件server\src\db_proxy_server\business\InterLogin.cpp的51行（CInterLoginStrategy::doLogin函数里面），将这个if语句注释掉：

即，我们不做密码校验，为后续调试省事。
/root/TeamTalk/server/src  重新编译的路径

4、执行终端命令：

chmod 777 build.sh

./build.sh version 1

编译完成，会在上级目录生成im-server-1.tar.gz包，手动解压它。

进入文件夹im-server-1，终端执行命令：
chmod 777 sync_lib_for_zip.sh
./sync_lib_for_zip.sh

它会将lib目录下的依赖库copy至各个文件夹下，同时也会把log4cxx.properties文件拷贝到各个文件夹。

5、打开文件/im-server-1/db_proxy_server/dbproxyserver.conf，修改为自己本机的mysql用户名和密码。

6、进入文件夹im-server-1，执行以下终端命令，先后启动服务

/etc/init.d/redis start
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server

问题：在配置db_proxy_server服务的dbproxyserver.conf文件里面的ip地址一改为我服务器的地址，用./restart.sh db_proxy_server就无法启动，但是ip改成127.0.0.1时却可以启动。
首先先给mysql外部访问权限，也就是修改vim /etc/my.cnf 里面的bind-address=0.0.0.0


问题：不管怎么样都报<BaseSocket.cpp>|<66>|<Listen>,bind failed, err_code=99 这个错误
首先检查mysql是否连外网，用telnet ip地址 3306 看一下能不能连上，可以的话检查再检查redis-server的telnet ip地址 6379看能不能连上，可以的话再把
db_proxy_server file_server msfs route_server http_msg_server login_server msg_server服务的ListenIP=你服务器ip地址改成ListenIP=0.0.0.0一般来说是可以的了。

问题：所有的服务都能打开但客户端连接不上。
1、先用phpmyadmin去mysql里面把所有127.0.0.1和localhost等等的用户删除，保留一个ip地址的用户。并编辑这个ip地址的用户的“按数据库指定权限”改为选择的“teamtalk”数据库。
2、在服务器控制台的安全组内添加所有端口开放。
3、重新修改各个服务的ip等。
如下：
loginserver.conf:
ClientListenIP=0.0.0.0      # can use multiple ip, seperate by ';'
ClientPort=8008
HttpListenIP=0.0.0.0
HttpPort=8080
MsgServerListenIP=0.0.0.0   # can use multiple ip, seperate by ';'
MsgServerPort=8100
msfs=http://127.0.0.1:8700/
discovery=http://127.0.0.1/api/discovery

msgserver.conf:
ListenIP=0.0.0.0
ListenPort=8000
ConcurrentDBConnCnt=2
DBServerIP1=10.5.45.63
DBServerPort1=10600
DBServerIP2=10.5.45.63
DBServerPort2=10600
LoginServerIP1=10.5.45.63
LoginServerPort1=8100
RouteServerIP1=10.5.45.63
RouteServerPort1=8200
PushServerIP1=10.5.45.63
PushServerPort1=8500
FileServerIP1=10.5.45.63
FileServerPort1=8600
IpAddr1=10.5.45.63   #电信IP
IpAddr2=10.5.45.63   #网通IP
MaxConnCnt=100000
#AES 密钥
aesKey=12345678901234567890123456789012

routeserver.conf:
ListenIP=0.0.0.0
ListenMsgPort=8200

httpmsgserver:
ListenIP=0.0.0.0
ListenPort=8400
ConcurrentDBConnCnt=4
DBServerIP1=140.143.11.129
DBServerPort1=10600
DBServerIP2=140.143.11.129
DBServerPort2=10600
RouteServerIP1=140.143.11.129
RouteServerPort1=8200

dbroxyserver:
ListenIP=0.0.0.0
ListenPort=10600
ThreadNum=48        # double the number of CPU core
MsfsSite=http://140.143.11.129:8700/
#configure for mysql
DBInstances=teamtalk_master,teamtalk_slave
#teamtalk_master
teamtalk_master_host=140.143.11.129
teamtalk_master_port=3306
teamtalk_master_dbname=teamtalk
teamtalk_master_username=root
teamtalk_master_password=teamtalk123
teamtalk_master_maxconncnt=16
#teamtalk_slave
teamtalk_slave_host=140.143.11.129
teamtalk_slave_port=3306
teamtalk_slave_dbname=teamtalk
teamtalk_slave_username=root
teamtalk_slave_password=teamtalk123
teamtalk_slave_maxconncnt=16
#configure for unread
CacheInstances=unread,group_set,token,group_member
#未读消息计数器的redis
unread_host=140.143.11.129
unread_port=6379
unread_db=1
unread_maxconncnt=16
#群组设置redis
group_set_host=140.143.11.129
group_set_port=6379
group_set_db=2
group_set_maxconncnt=16
#deviceToken redis
token_host=140.143.11.129
token_port=6379
token_db=4
token_maxconncnt=16
#GroupMember
group_member_host=140.143.11.129
group_member_port=6379
group_member_db=5
group_member_maxconncnt=48
#AES 密钥
aesKey=12345678901234567890123456789012


0 0 收藏 | 阅读（124） 评论(0)
高级模式
BColorImageLinkQuoteCodeSmilies
您需要登录后才可以回帖 登录 | 立即注册
本版积分规则发表评论   回帖后跳转到最后一页
琼ICP备17005251号Archiver手机版小黑屋  

商务洽谈:171953634  

© 2004-2015 bug猿,all rights reserved

ClientListenIP=10.5.45.63
ClientPort=8008
HttpListenIP=10.5.45.63
HttpPort=8080
MsgServerListenIP=10.5.45.63
MsgServerPort=8100
msfs=http://10.5.45.63:8700/
discovery=http://10.5.45.63/api/discovery


15.1.3 http_msg_server(在/root/TeamTalk/server/im-server-1.0.0/http_msg_server/目录下的httpmsgserver.conf文件)

ListenIP=0.0.0.0
ListenPort=8400
ConcurrentDBConnCnt=4
DBServerIP1=127.0.0.1
DBServerPort1=10600
DBServerIP2=127.0.0.1
DBServerPort2=10600
RouteServerIP1=localhost
RouteServerPort1=8200
#RouteServerIP2=localhost
#RouteServerPort2=8201

ListenIP:监听IP，供其他人来调用http_msg_server接口，比如，php在创建群组的时候，就会来调用http_msg_server的接口。
ListenPort:监听端口，与上一个配套使用。
ConcurrentDBConnCnt:DB数目，目前必须配置为2的整数倍，是历史遗留问题，后期会修复。
DBServerIP(x):db_proxy_server监听的IP，http_msg_server会主动去连接。
DBServerPort(x):db_proxy_server监听的Port
RouteServerIP(x):route_server监听的IP，http_msg_server会主动去连接。
RouteServer(x):route_server监听的Port

参考配置:
ListenIP=10.5.45.63
ListenPort=8400
ConcurrentDBConnCnt=4
DBServerIP1=10.5.45.63
DBServerPort1=10600
DBServerIP2=10.5.45.63
DBServerPort2=10600
RouteServerIP1=10.5.45.63
RouteServerPort1=8200


rm -rf msg_server/msgserver.conf
vim msg_server/msgserver.conf
15.1.4 msg_server(在/root/TeamTalk/server/im-server-1.0.0/msg_server/目录下的msgserver.conf文件)
ListenIP=0.0.0.0
ListenPort=8000
ConcurrentDBConnCnt=2
DBServerIP1=127.0.0.1
DBServerPort1=10600
DBServerIP2=127.0.0.1
DBServerPort2=10600
LoginServerIP1=127.0.0.1
LoginServerPort1=8100
#LoginServerIP2=localhost
#LoginServerPort2=8101
RouteServerIP1=127.0.0.1
RouteServerPort1=8200
#RouteServerIP2=localhost
#RouteServerPort2=8201
PushServerIP1=127.0.0.1
PushServerPort1=8500
FileServerIP1=127.0.0.1
FileServerPort1=8600
#FileServerIP2=localhost
#FileServerPort2=8601
IpAddr1=127.0.0.1   #电信IP
IpAddr2=127.0.0.1   #网通IP
MaxConnCnt=100000
#AES 密钥
aesKey=12345678901234567890123456789012

ListenIP:监听客户端连接上来的IP。
ListenPort:与上一个配套使用，监听客户端连接的port。
ConcurrentDBConnCnt:db_proxy_server个数，同http_msg_server 一样。
DBServerIP(x):db_proxy_server监听的ip，msg_server主动去连接。
DBServerPort(x):db_proxy_server监听的port。
LoginServerIP(x):login_server监听的ip，msg_server会主动去连接，汇报本机信息。
LoginServerPort(x):login_server监听的port。
RouteServerIP(x):route_server监听的IP，msg_server主动去连接。
RouteServerPort(x):route_server监听的port。
PushServerIP(x):push_server监听的IP，msg_server会主动去连接，给ios系统推送消息。
PushServerPort(x):push_server监听的port。
FileServerIP(x):file_server监听的IP，msg_server会主动去连接，用于文件传输，暂时未用到。
FileServerPort(x):file_server监听的port。
IpAddr1:msg_server监听的ip，用于汇报给login_server，便于login_server在客户端请求的时候返回给客户端。注意，这个ip一定要是客户端能连接的ip，之前发现好多人配置成127.0.0.1，这是不行的。
IpAddr2:同上。
aesKey:消息文本加密密钥.这里配置主要在msg_server向push_server推送的时候需要将加密的消息进行解密。

参考配置:
ListenIP=10.5.45.63
ListenPort=8000
ConcurrentDBConnCnt=2
DBServerIP1=10.5.45.63
DBServerPort1=10600
DBServerIP2=10.5.45.63
DBServerPort2=10600
LoginServerIP1=10.5.45.63
LoginServerPort1=8100
RouteServerIP1=10.5.45.63
RouteServerPort1=8200
PushServerIP1=10.5.45.63
PushServerPort1=8500
FileServerIP1=10.5.45.63
FileServerPort1=8600
IpAddr1=10.5.45.63   #电信IP
IpAddr2=10.5.45.63   #网通IP
MaxConnCnt=100000
#AES 密钥
aesKey=12345678901234567890123456789012


rm -rf db_proxy_server/dbproxyserver.conf
vim db_proxy_server/dbproxyserver.conf
15.1.5 db_proxy_server(在/root/TeamTalk/server/im-server-1.0.0/db_proxy_server/目录下的dbproxyserver.conf文件)
ListenIP:db_proxy_server监听的IP。
ListenPort:db_proxy_server监听的port
ThreadNum:工作线程个数。
MsfsSite:配置msfs服务器的地址，用于发送语音的时候上传保存语音文本。
DBInstances:db实例名称。一般配置一主一从即可，其他根据自己的需求修改。
(xxxx)_host:xxxx实例的ip
(xxxx)_port:xxxx实例的port
(xxxx)_dbname:xxxx实例的scheme名称
(xxxx)_username:xxxx实例的用户名
(xxxx)_password:xxxx实例的密码
(xxxx)_maxconncnt:xxxx实例最大连接数
CacheInstances:cache实例名称。
(xxxx)_host:xxxx实例的ip
(xxxx)_port:xxxx实例的port
(xxxx)_db:xxxx实例的db
(xxxx)_maxconncnt:xxxx
aesKey:消息加密密钥。
目前我们db实例配置的一主一从，cache实例配置了5个实例，分别是:
unread:主要用于未读计数。
group_set:群组设置。设置屏蔽群组。
token:主要用于保存ios系统的token。
group_member:保存群成员信息。

参考配置:
ListenIP=10.5.45.63
ListenPort=10600
ThreadNum=48        # double the number of CPU core
MsfsSite=http://10.5.45.63:8700/
#configure for mysql
DBInstances=teamtalk_master,teamtalk_slave
#teamtalk_master
teamtalk_master_host=10.5.45.63
teamtalk_master_port=3306
teamtalk_master_dbname=teamtalk
teamtalk_master_username=teamtalk
teamtalk_master_password=test@123
teamtalk_master_maxconncnt=16
#teamtalk_slave
teamtalk_slave_host=10.5.45.63
teamtalk_slave_port=3306
teamtalk_slave_dbname=teamtalk
teamtalk_slave_username=teamtalk
teamtalk_slave_password=test@123
teamtalk_slave_maxconncnt=16
#configure for unread
CacheInstances=unread,group_set,token,group_member
#未读消息计数器的redis
unread_host=10.5.45.63
unread_port=6379
unread_db=1
unread_maxconncnt=16
#群组设置redis
group_set_host=10.5.45.63
group_set_port=6379
group_set_db=2
group_set_maxconncnt=16
#deviceToken redis
token_host=10.5.45.63
token_port=6379
token_db=4
token_maxconncnt=16
#GroupMember
group_member_host=10.5.45.63
group_member_port=6379
group_member_db=5
group_member_maxconncnt=48
#AES 密钥
aesKey=12345678901234567890123456789012



/etc/init.d/redis start
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server

/root/TeamTalk/server/im-server-1/db_proxy_server/log/

[root@63 log]# vim default.log 

2018-12-08 04:44:22,997 [INFO  IM] - <CachePool.cpp>|<59>|<Init>,redisConnect failed: Connection refused
2018-12-08 04:44:22,997 [INFO  IM] - <CachePool.cpp>|<710>|<Init>,Init cache pool failed



telnet 10.5.45.63  lsof -i -Pn
CentOS 7 下 ss 替代 netstat   ss -lntp | cat

State      Recv-Q Send-Q Local Address:Port               Peer Address:Port              
LISTEN     0      128          *:18888                    *:*                   users:(("gunicorn",pid=19740,fd=11),("gunicorn",pid=19735,fd=11))
LISTEN     0      64     10.5.45.63:10600                    *:*                   users:(("db_proxy_server",pid=19446,fd=18))
LISTEN     0      64           *:8008                     *:*                   users:(("login_server",pid=19484,fd=5))
LISTEN     0      64           *:8200                     *:*                   users:(("route_server",pid=19467,fd=5))
LISTEN     0      128          *:6379                     *:*                   users:(("redis-server",pid=9208,fd=5))
LISTEN     0      64           *:8080                     *:*                   users:(("login_server",pid=19484,fd=8))
LISTEN     0      64           *:8400                     *:*                   users:(("http_msg_server",pid=19474,fd=5))
LISTEN     0      128          *:22                       *:*                   users:(("sshd",pid=9170,fd=3))
LISTEN     0      64           *:8600                     *:*                   users:(("file_server",pid=19455,fd=6))
LISTEN     0      64     10.5.45.63:8601                     *:*                   users:(("file_server",pid=19455,fd=7))
LISTEN     0      64           *:8000                     *:*                   users:(("msg_server",pid=19494,fd=5))
LISTEN     0      64           *:8100                     *:*                   users:(("login_server",pid=19484,fd=7))
LISTEN     0      80          :::3306                    :::*                   users:(("mysqld",pid=9776,fd=17))
LISTEN     0      128         :::6379                    :::*                   users:(("redis-server",pid=9208,fd=4))
LISTEN     0      128         :::22                      :::*                   users:(("sshd",pid=9170,fd=4))
