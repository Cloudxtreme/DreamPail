#How to install shadowsocks on CentOS 6.x
================================================
step 1:yum install wget 

step 2:wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

step 3:chmod +x shadowsocks.sh

step 4:./shadowsocks.sh 2>&1 | tee shadowsocks.log

#how to uninstall：
login with root and input this：

./shadowsocks.sh uninstall

path of profile ：/etc/shadowsocks.json


multiple user：

 "port_password":{
 
         "8989":"password0",
         
         "9001":"password1",
         
         "9002":"password2",
         
         "9003":"password3",
         
         "9004":"password4"
         
    },

start ：/etc/init.d/shadowsocks start

stop ：/etc/init.d/shadowsocks stop

restart ：/etc/init.d/shadowsocks restart

status ：/etc/init.d/shadowsocks status


#How to install shadowsocks on CentOS 7+
=====================================================
yum install -y gcc automake autoconf libtool make build-essential curl curl-devel zlib-devel openssl-devel perl perl-devel cpio expat-devel gettext-devel git

git clone https://github.com/shadowsocks/shadowsocks-libev.git

cd shadowsocks-libev

./configure && make

make install

mkdir -p /etc/shadowsocks

vi /etc/shadowsocks/config.json

{

 "server":"0.0.0.0",
 
 "server_port":443,
 
 "local_address": "127.0.0.1",
 
 "local_port":1080,
 
 "password":"password",
 
 "timeout":300,
 
 "method":"rc4-md5",
 
 "fast_open": false,
 
 "workers": 1
 
}


vi /etc/systemd/system/shadowsocks-server.service

[Unit]

Description=Shadowsocks service

After=network.target


[Service]

Type=simple

User=root

ExecStart=/usr/local/bin/ss-server -c /etc/shadowsocks/config.json

ExecReload=/bin/kill -HUP $MAINPID

ExecStop=/bin/kill -s QUIT $MAINPID PrivateTmp=true

KillMode=process

Restart=on-failure

RestartSec=5s


[Install]
WantedBy=multi-user.target


systemctl start shadowsocks-server.service

systemctl enable shadowsocks-server.service



#Optimize centos
======================================================

edit limits.conf

vi /etc/security/limits.conf

add two column

* soft nofile 51200
* 
* hard nofile 51200

edit sysctl.conf

vi /etc/sysctl.conf

add content:

fs.file-max = 51200

net.ipv4.conf.lo.accept_redirects=0

net.ipv4.conf.all.accept_redirects=0

net.ipv4.conf.eth0.accept_redirects=0

net.ipv4.conf.default.accept_redirects=0

net.ipv4.ip_local_port_range = 10000 65000

net.ipv4.tcp_congestion_control = hybla

net.ipv4.tcp_fin_timeout = 30

net.ipv4.tcp_fastopen = 3

net.ipv4.tcp_keepalive_time = 1200

net.ipv4.tcp_rmem  = 32768 436600 873200

net.ipv4.tcp_syncookies = 1

net.ipv4.tcp_synack_retries = 2

net.ipv4.tcp_syn_retries = 2

net.ipv4.tcp_tw_reuse = 1

net.ipv4.tcp_tw_recycle = 0

net.ipv4.tcp_timestsmps = 0

net.ipv4.tcp_tw_reuse = 1

net.ipv4.tcp_max_tw_buckets = 9000

net.ipv4.tcp_max_syn_backlog = 65536

net.ipv4.tcp_mem = 94500000 91500000 92700000

net.ipv4.tcp_max_orphans = 3276800

net.ipv4.tcp_mtu_probing = 1

net.ipv4.tcp_wmem = 8192 436600 873200

net.core.netdev_max_backlog = 250000

net.core.somaxconn = 32768

net.core.wmem_default = 8388608

net.core.rmem_default = 8388608

net.core.rmem_max = 67108864

net.core.wmem_max = 67108864

execute this:

sysctl -p


#Install serverspeeder
=========================================================
Install：

wget http://soft.91yun.org/soft/serverspeeder/serverspeeder-all.sh && bash serverspeeder-all.sh

uninstall：

chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f

------------------------------------------------
