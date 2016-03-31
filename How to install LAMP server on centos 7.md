#How to install LAMP server on centos 7

 LAMP is a group of open source softwares installed together to build a webserver. LAMP refers to Linux (Operating system), Apache (Web service), MySQL/MariaDB (Database) , PHP (Programming language).
This guide helps you to install LAMP server on centos 7.
Before starting installation, you need to setup Static IP and hostname .
You can also refer this guide – Setup network on centos 7 to setup static ip and hostname.
Install LAMP server on centos 7.
1. Apache installation.
2. Mysql installation.
3. PHP installation.
4. Testing all together.
let’s start

Apache installation
##Step 1 » Update the repositories.
###[root@ ^_^ ~]# yum check-update

##Step 2 » After updating repository, issue the below command to install apache package.
[root@ ^_^ ~]# yum install httpd

##Step 3 » Now start the service and enable it at startup.
Command to start the service

[root@ ^_^ ~]# systemctl start httpd.service

Command to enable at startup

[root@ ^_^ ~]# systemctl enable httpd.service

##Step 4 » By default, Apache will listen on port 80. you need to exclude from firewall.
 you can simply exclude http service from firewall.
 
[root@ ^_^ ~]# firewall-cmd --permanent --add-service http

 or you can exclude using port number. Below command will be useful for ports other than 80
 
[root@ ^_^ ~]# firewall-cmd --permanent --add-port=8080/tcp

##Step 5 » Now restart firewall service.
[root@ ^_^ ~]# systemctl restart firewalld.service

##Step 6 » Apache installation is over . For testing, open http://serverip in your browser, you can see apache demo page like below.
install lamp server on centos 7

MySQL installation.
##Step 7 » Start installing MariaDB, MySQL drop-in replacement.
[root@ ^_^ ~]# yum install mariadb-server mariadb

##Step 8 » Now start the service and enable it at startup.
Start the service

[root@ ^_^ ~]# systemctl start mariadb

Enable at startup

[root@ ^_^ ~]# systemctl enable mariadb.service

##Step 9 » Secure your DB installation. Type the below command and provide values.

[root@ ^_^ ~]# mysql_secure_installation

1. current password ( Leave blank and hit Enter ).
2. Enter new password.
3. Re Enter password.
and Hit enter for all the other options.


 
##Step 10 » MariaDB installation is over. For testing, Check login into DB using the below command.

[root@ ~]# mysql -u root -p

Enter password:
Welcome to the MariaDB monitor. Commands end with ; or g.
Your MariaDB connection id is 10
Server version: 5.5.37-MariaDB MariaDB Server
Copyright (c) 2000, 2014, Oracle, Monty Program Ab and others.
Type 'help;' or 'h' for help. Type 'c' to clear the current input statement.
MariaDB [(none)]>
PHP installation.
##Step 11 » Install PHP and other recommended packages.

[root@ ~]# yum install php php-mysql

Additional packages are required if you would like to install phpmyadmin .

[root@ ~]# yum install php-gd php-pear php-mbstring php-pgsql

##Step 12 » Now restart apache service.

[root@ ~]# systemctl restart httpd.service

##Step 13 » For testing, Create a file phpinfo.php in /var/www/html/ ( Default root directory ) and add the below code.

<?php phpinfo(); ?>

Now open http://serverIP/phpinfo.php in your browser. you will see PHP version and other configuration details like below.
install lamp server  on centos 7

Testing all together
##Step 14 » For testing Database connectivity through PHP. Create a file dbtest.php in /var/www/html/ and add below code . Kindly replace with your root password in the below code .


<?php
$con = mysql_connect("localhost","root","password");
if (!$con)
{
die('Could not connect: ' . mysql_error());
}
else
{
echo "Congrats! connection established successfully";
}
mysql_close($con);
?>
1  <?php
2  $con = mysql_connect("localhost","root","password");
3  if (!$con)
4  {
5  die('Could not connect: ' . mysql_error());
6  }
7  else
8  {
9  echo "Congrats! connection established successfully";
10 }
11 mysql_close($con);
12 ?>

Now access http://serverIP/dbtest.php . you should get congrats message.
Have a nice day.

