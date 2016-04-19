# How to install lemp on your vps

yum install nginx -y

systemctl start nginx 
systemctl enable nginx

systemctl stop httpd
systemctl disable httpd

firewall-cmd --permanent --add-service=http
systemctl restart firewalld

vi /etc/nginx/nginx.conf

worker_processes 1;   //In my case it’s “1″. So I set this as ’1′,if you dont know,enter 'lscpu'

systemctl restart nginx

==============================================================================
#Install MariaDB

yum install mariadb-server mariadb -y

systemctl start mariadb
systemctl enable mariadb

mysql_secure_installation
####enter Y and set new passwor then press Y 4times
=============================================================================

#Install PHP

####CentOS / RHEL 7

yum install epel-release

rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

rpm -Uvh http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm


####CentOS / RHEL 6
 yum install epel-release
 
 rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
 
 rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm
 
 rpm -Uvh http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm

yum install php70w

yum search php70w

yum install php70w-mysql php70w-xml php70w-soap php70w-xmlrpc

yum install php70w-mbstring php70w-json php70w-gd php70w-mcrypt

yum install php70w-fpm

$ sudo nano /etc/nginx/conf.d/example.conf

----------------------
server {
        listen   80;

        root /var/www;
        index index.php index.html index.htm;       //the url of homepage
        server_name  example.com www.example.com;   //the domain you have

        location / {
                try_files $uri $uri/ /index.html;
        }

        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
              root /usr/share/nginx/www;
        }

        location ~ .php$ {
                try_files $uri =404;
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }
}
------------------------------
 systemctl restart nginx.service
 
 systemctl restart php-fpm.service

 firewall-cmd --permanent --zone=public --add-service=http
 
 firewall-cmd --permanent --zone=public --add-service=https
 
 firewall-cmd --reload

done!

 php -v
 
 nginx -v
