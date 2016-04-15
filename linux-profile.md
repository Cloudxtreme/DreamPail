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
