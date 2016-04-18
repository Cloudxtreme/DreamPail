# how to install lemp on your vps

#*step 1:Now, install Nginx with following command:

yum install nginx -y

*step 2:Start Nginx and make it to start automatically on every reboot:

systemctl start nginx 

systemctl enable nginx

*step 3:Stop Apache or any other web servers if you have any:

systemctl stop httpd

systemctl disable httpd

*step 4:Allow Nginx server through your firewall/router if you want to access the web server from other systems:

firewall-cmd --permanent --add-service=http

systemctl restart firewalld

Now point your web browser with “http://ip-address″. The test page of nginx will open.

Nginx web server has been installed now.

*step 5:Configure Nginx

Open the file /etc/nginx/nginx.conf and set the worker_processes (i.e No. of CPU’s in your system). To see the no. of CPU’s, use the command “lscpu”.

vi /etc/nginx/nginx.conf

In my case it’s “1″. So I set this as ’1′:

worker_processes 1;

Scroll down and make the changes as shown below.

[...]
server {
        listen       80;
        server_name  server.unixmen.local;
        root         /usr/share/nginx/html;
[...]
    ## Uncomment or Add the following lines

    location ~ \.php$ {
            root           /usr/share/nginx/html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

[...]
Save and close the file. Restart Nginx service:

systemctl restart nginx

==============================================================================
#Install MariaDB
