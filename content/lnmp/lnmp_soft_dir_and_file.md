---
title: "LNMP软件目录与文件位置"
layout: page
date: 2016-05-25 00:00
---

[TOC]

### LNMP相关软件安装目录 ###

Nginx 目录: /usr/local/nginx/

MySQL 目录 : /usr/local/mysql/

MySQL数据库所在目录：/usr/local/mysql/var/

MariaDB 目录 : /usr/local/mariadb/

MariaDB数据库所在目录：/usr/local/mariadb/var/

PHP目录 : /usr/local/php/

PHPMyAdmin目录 :

* 0.9版本为/home/wwwroot/phpmyadmin/
* 1.0及以后版本为 /home/wwwroot/default/phpmyadmin/
* 强烈建议将此目录重命名为其不容易猜到的名字。phpmyadmin可自己从官网下载新版替换。

默认网站目录 :

* 0.9版本为 /home/wwwroot/
* 1.0及以后版本为 /home/wwwroot/default/

Nginx日志目录：/home/wwwlogs/

/root/vhost.sh添加的虚拟主机配置文件所在目录：/usr/local/nginx/conf/vhost/

PureFtpd 目录：/usr/local/pureftpd/

PureFtpd web管理目录： 0.9版为/home/wwwroot/default/ftp/ 1.0版为 /home/wwwroot/default/ftp/

Proftpd 目录：/usr/local/proftpd/

Redis 目录：/usr/local/redis/

<br>
### LNMP相关配置文件位置 ###

Nginx主配置(默认虚拟主机)文件：/usr/local/nginx/conf/nginx.conf

添加的虚拟主机配置文件：/usr/local/nginx/conf/vhost/域名.conf

MySQL配置文件：/etc/my.cnf

PHP配置文件：/usr/local/php/etc/php.ini

php-fpm配置文件：/usr/local/php/etc/php-fpm.conf

PureFtpd配置文件：/usr/local/pureftpd/pure-ftpd.conf

PureFtpd MySQL配置文件：/usr/local/pureftpd/pureftpd-mysql.conf

Proftpd配置文件：/usr/local/proftpd/etc/proftpd.conf

* 1.2及之前版本为/usr/local/proftpd/proftpd.conf

Proftpd 用户配置文件：/usr/local/proftpd/etc/vhost/用户名.conf

Redis 配置文件：/usr/local/redis/etc/redis.conf

<br>
### LNMPA相关目录文件位置 ###

Apache目录：/usr/local/apache/

Apache配置文件：/usr/local/apache/conf/httpd.conf

Apache虚拟主机配置文件目录：/usr/local/apache/conf/vhost/

Apache默认虚拟主机配置文件：/usr/local/apache/conf/extra/httpd-vhosts.conf

虚拟主机配置文件名称：/usr/local/apache/conf/vhost/域名.conf
