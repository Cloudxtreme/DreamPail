安裝 MariaDB

執行以下指令安裝 MariaDB:

'# yum install mariadb-server mariadb

啟動及設定開機自動執行 MariaDB:

'# systemctl start mariadb

'# systemctl enable mariadb

執行以下指令設定 MariaDB 的 root 密碼, 預設是空密碼, 所以建議盡快修改:

'# /usr/bin/mysql_secure_installation

完成後可以用測試一下 MariaDB 是否已經啟動:

'# mysql -u root -p

安裝 Apache

'# yum install httpd

跟著回答 “y” 後便會完成安裝, 然後輸入以下指令啟動及設定 Apache 開機自動執行:

'# systemctl start httpd

'# systemctl enable httpd

這時 Apache 已經啟動了, 可以在瀏覽器輸入伺服器的位置試試, 例如 http://localhost







---------------------------------------------------
#从GitHub下载php7安装包

wget -c --no-check-certificate -O php7-src-master.zip https://github.com/php/php-src/archive/master.zip

# 解压php7包

unzip -q php7-src-master.zip && cd php-src-master

#安装依赖包

yum -y install libxml2 libxml2-devel openssl openssl-devel curl-devel libjpeg-devel libpng-devel freetype-devel libmcrypt-devel epel-release libmcrypt-devel

#配置编译参数

# 开始配置
./configure 

--prefix=/usr/local/php7 
                             
--exec-prefix=/usr/local/php7 

--bindir=/usr/local/php7/bin 

--sbindir=/usr/local/php7/sbin 

--includedir=/usr/local/php7/include 

--libdir=/usr/local/php7/lib/php 

--mandir=/usr/local/php7/php/man 

--with-config-file-path=/usr/local/php7/etc 
          
--with-mysql-sock=/var/run/mysql/mysql.sock 
          
--with-mcrypt=/usr/include 

--with-mhash 

--with-openssl 

--with-mysql=shared,mysqlnd 
                          
--with-mysqli=shared,mysqlnd 
                         
--with-pdo-mysql=shared,mysqlnd 
                      
--with-gd 

--with-iconv 

--with-zlib 

--enable-zip 

--enable-inline-optimization 

--disable-debug 

--disable-rpath 

--enable-shared 

--enable-xml 

--enable-bcmath 

--enable-shmop 

--enable-sysvsem 

--enable-mbregex 

--enable-mbstring 

--enable-ftp 

--enable-gd-native-ttf 

--enable-pcntl 

--enable-sockets 

--with-xmlrpc 

--enable-soap 

--without-pear 

--with-gettext 

--enable-session 
                                     
--with-curl 
                                          
--with-jpeg-dir 

--with-freetype-dir 

--enable-opcache 
                                     
--enable-fpm 

--with-fpm-user=nginx 
                                 
--with-fpm-group=nginx 
                                
--without-gdbm 

--disable-fileinfo

#编译和安装

make -j `grep processor /proc/cpuinfo | wc -l`

make install

由于需要和MySQL进行通信，所以需要特别查看PHP7安装后的lib扩展库目录（/usr/local/php7/lib/php/extensions/no-debug-non-zts-20141001/）。需要确保至少存在mysqli.so、pdo_mysql.so这两个动态库文件

#配置文件和脚本

cp php.ini-production /usr/local/php7/etc/php.ini

cp /root/php-src-master/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm

cp /usr/local/php7/etc/php-fpm.conf.default /usr/local/php7/etc/php-fpm.conf

cp /usr/local/php7/etc/php-fpm.d/www.conf.default /usr/local/php7/etc/php-fpm.d/www.conf

#添加php的环境变量

echo -e '\nexport PATH=/usr/local/php7/bin:/usr/local/php7/sbin:$PATH\n' >> /etc/profile && source /etc/profile

#设置目录
##设置PHP日志目录和php-fpm进程文件（php-fpm.sock）目录

##其中，设置php-fpm进程目录的用户和用户组为nginx，并创建php会话session目录。

# 设置PHP日志目录和php-fpm的运行进程ID文件（php-fpm.sock）目录

mkdir -p /var/log/php-fpm/ && mkdir -p /var/run/php-fpm && cd /var/run/ && chown -R nginx:nginx php-fpm


# 修改session的目录配置

mkdir -p /var/lib/php/session

chown -R nginx:nginx /var/lib/php

#设置PHP开机启动

# 配置开机自启动，增加到主机sysV服务

chmod +x /etc/init.d/php-fpm

chkconfig --add php-fpm

chkconfig php-fpm on

# 测试PHP的配置文件是否正确合法

php-fpm -t

#启动php服务

service php-fpm start
