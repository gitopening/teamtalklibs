 ln -s /usr/lib/java-1.8.0/jdk1.8.0_212/bin/java /usr/bin/java
java -version
springts
ln -s /root/user/soft/java/sts-4.3.2/SpringToolSuite4 /usr/local/bin/springts
java -version
rm -rf /usr/bin/java
java -version
ln -s /root/user/soft/php/PhpStorm/bin/phpstorm.sh  /usr/local/bin/phpstorm
phpstorm
wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-4.9.2/gcc-4.9.2.tar.gz
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
vi /etc/my.cnf
systemctl restart mysqld
ls
tar -zxvf gcc-4.9.2.tar.gz 
cd gcc-4.9.2/
./contrib/download_prerequisites
ping www.baidu.com
wget http://ftp.gnu.org/gnu/gdb/gdb-7.9.tar.gz、
wget http://ftp.gnu.org/gnu/gdb/gdb-7.9.tar.gz
wget ftp://ftp.gnu.org/gnu/termcap/termcap-1.3.1.tar.gz
wget https://github.com/google/protobuf/releases/download/v2.6.1/protobuf-2.6.1.tar.gz
git clone https://github.com/meili/TeamTalk.git
http://ftp.gnu.org/gnu/gcc/gcc-4.9.2/gcc-4.9.2.tar.gz
cg gcc-4.9.2
./contrib/download_prerequisites
cd gcc-4.9.2/
./contrib/download_prerequisites
 http://ftp.gnu.org/gnu/gcc/gcc-4.9.2/gcc-4.9.2.tar.gz
http://ftp.gnu.org/gnu/gcc/gcc-4.9.2/gcc-4.9.2.tar.gz
cd gcc-4.9.2/
./contrib/download_prerequisites
wget ftp://ftp.gnu.org/gnu/gmp/gmp-4.3.2.tar.bz2
wget http://www.mpfr.org/mpfr-2.4.2/mpfr-2.4.2.tar.bz2
wget http://www.multiprecision.org/mpc/download/mpc-0.8.1.tar.gz
wget https://zh.osdn.net/frs/g_redir.php?m=kent&f=d2718c%2Fmpc-0.8.1.tar.gz
./contrib/download_prerequisites
tar xzf cloog-0.18.1.tar.gz 
tar xjf isl-0.12.2
tar xjf isl-0.12.2.tar.bz2 
./contrib/download_prerequisites
yum -y install wget vim git texinfo patch make cmake gcc gcc-c++ gcc-g77 flex bison file libtool libtool-libs autoconf kernel-devel libjpeg libjpeg-devel libpng libpng-devel libpng10 libpng10-devel gd gd-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glib2 glib2-devel bzip2 bzip2-devel libevent libevent-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel vim-minimal nano fonts-chinese gettext gettext-devel ncurses-devel gmp-devel pspell-devel unzip libcap diffutils
cd /var/www/html/newtopup/simplewind/vendor/GatewayWorker/
cd /www/wwwroot/newtopup/simplewind/vendor/GatewayWorker
ls
php start.php start -d
 http://ftp.gnu.org/gnu/gcc/gcc-4.9.2/gcc-4.9.2.tar.gz
ftp://ftp.gnu.org/gnu/gmp/gmp-4.3.2.tar.bz2
wget http://ftp.gnu.org/gnu/gcc/gcc-4.9.2/gcc-4.9.2.tar.gz
cd ..
ls
tar -zxvf gcc-4.9.2.tar.gz 
cd gcc-4.9.2/
./contrib/download_prerequisites
gcc -version
gcc --version
./contrib/download_prerequisites
cd gcc-4.9.2/
./contrib/download_prerequisites
mkdir gcc-build-4.9.2
cd gcc-build-4.9.2/
../configure --prefix=/usr -enable-checking=release -enable-languages=c,c++ -disable-multilib
make -j 2 && make install
tar -zxvf termcap-1.3.1.tar.gz 
cd termcap-1.3.1/
./configure --prefix=/usr
make -j 2 && make install
cd ..
tar -zxvf gdb-7.9.tar.gz 
cd gdb-7.9/
./configure --prefix=/usr
make -j 2 && make install
cd ..
reboot
tar -zxvf protobuf-2.6.1.tar.gz 
cd protobuf-2.6.1/
./configure --prefix=/usr/local/protobuf
make -j 2 && make install
mkdir -p /root/TeamTalk/server/src/base/pb/lib/linux/
cp /usr/local/protobuf/lib/libprotobuf-lite.a /root/TeamTalk/server/src/base/pb/lib/linux/
cp  -r /usr/local/protobuf/include/* /root/TeamTalk/server/src/base/pb/
cd /root/TeamTalk/pb
export PATH=$PATH:/usr/local/protobuf/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/protobuf/lib
sh create.sh 
sh sync.sh 
cd /root/TeamTalk/server/src/
sh make_log4cxx.sh 
sh make_hiredis.sh 
cd db_proxy_server/
vi CMakeLists.txt 
cd ../
sh build.sh version 1.0.0
 yum install mysql-devel
sh build.sh version 1.0.0
cd /www/wwwroot/newtopup/simplewind/vendor/GatewayWorker
php start.php start -d
cd /root/TeamTalk/server/src
sh build.sh version 1.0.0
pwd
cd ../im-server-1.0.0/
./restart.sh  db_proxy_server
chmod 777 sync_lib_for_zip.sh 
./sync_lib_for_zip.sh 
./restart.sh  db_proxy_server
cd ../src/
chmod 777 build.sh 
./build.sh version 1
cd ../m-server-1/
chmod 777 sync_lib_for_zip.sh 
./sync_lib_for_zip.sh 
./restart.sh db_proxy_server
cd db_proxy_server/
../daeml db_proxy_server 
chmod 777 db_proxy_server 
./db_proxy_server 
cd ../file_server/
chmod 777 file_server 
./file_server 
cd ../msfs/
chmod 777 msfs
./msfs 
cd ../route_server/
chmod 777 route_server 
./route_server 
cd ../http_msg_server/
chmod 777 http_msg_server 
./http_msg_server 
cd ../login_server/
chmod 777 login_server 
./login_server 
cd ../msg_server/
chmod 777 msg_server 
./msg_server 
cd ..
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server
./restart.sh push_server
./restart.sh db_proxy_server
pwd
cd ../src/
chmod 777 make_protobuf.sh 
./make_protobuf.sh 
chmod 777 make_log4cxx.sh 
./make_log4cxx.sh 
chmod 777 build.sh 
./build.sh  version 2
cd ../tim-server-2/
chmod 777 sync_lib_for_zip.sh 
./sync_lib_for_zip.sh 
ip add
mysql -uroot -proot
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
vi /root/TeamTalk/server/src/db_proxy_server/business/InterLogin.cpp
ps -ef|grep server
vi /etc/my.cnf
cd /root/TeamTalk/server/tim-server-2/db_proxy_serve
cd /root/TeamTalk/server/tim-server-2/db_proxy_server/
chmod 777 db_proxy_server 
./db_proxy_server 
cd ../file_server/
chmod 777 file_server 
./file_server 
cd ../http_msg_server/
chmod 777 http_msg_server 
./http_msg_server 
cd ../login_server/
chmod 777 login_server 
./login_server 
cd ../msfs/
chmod 777 msfs
./msfs 
cd ../msg_server/
chmod 777 msg_server 
./msg_server 
cd ../push_server/
chmod 777 push_server 
./push_server 
cd ../route_server/
chmod 777 route_server 
./route_server 
cd ..
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server
./restart.sh push_server
pwd
cd /root/TeamTalk/server/tim-server-2
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server
./restart.sh push_server
./restart.sh db_proxy_server
./restart.sh msg_server
./restart.sh msfs
ps -ef|grep server
cd /root/TeamTalk/server/tim-server-2
./restart.sh db_proxy_server
./restart.sh file_server
./restart.sh msfs
./restart.sh route_server
./restart.sh http_msg_server
./restart.sh login_server
./restart.sh msg_server
./restart.sh push_server
ps -ef|grep server
cd /root/user/soft/php/PhpStorm/bin/
./phpstorm.sh 
phpstorm
histroy
history
ip add
ping 192.168.0.202
export LANG="en_US.UTF-8";export LANGUAGE="en_US.UTF-8";top
mysql -uroot -proot
ip add
mysql -uroot -proot
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
ip add
bt default
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
vi /etc/my.cnf
systemctl restart mysqld
bt default
ip add
mysql -uroot -proot
git -v
git --version
git remote -v


INSERT INTO IMUser (id, sex, name, domain, nick, password, salt, phone, email, avatar, departId, status, created, updated, push_shield_status, sign_info) VALUES ('1', '0', '101', '127.0.0.1', '', '', '', '', '', '', '1', '1', '1', '1', '0', '')

分包压缩的命令
tar czf - mysql-5.6.35.tar.gz | split -b 13m - mysql-5.6.35.tar.gz
tar czf - gdb-7.9.tar.gz | split -b 13m - gdb-7.9.tar.gz.tar.gz
tar czf - gcc-4.9.2.tar.gz | split -b 13m - gcc-4.9.2.tar.gz



