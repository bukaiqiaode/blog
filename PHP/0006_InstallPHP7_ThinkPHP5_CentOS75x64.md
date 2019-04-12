# Background
Linux version = CentOS 7.5(x64)

# Step 1 Remove existing PHP

```shell
[root@0001 ~]# rpm -qa | grep php
php-cli-5.4.16-46.el7.x86_64
php-pdo-5.4.16-46.el7.x86_64
php-common-5.4.16-46.el7.x86_64
[root@0001 ~]# rpm -e php-cli-5.4.16-46.el7.x86_64
[root@0001 ~]# rpm -e php-pdo-5.4.16-46.el7.x86_64
[root@0001 ~]# rpm -e php-common-5.4.16-46.el7.x86_64
[root@0001 ~]# rpm -qa | grep php
[root@0001 ~]# php -v
-bash: php: command not found
[root@0001 ~]#
```

# Step 2 Install PHP-7x
Follow `https://www.cnblogs.com/lightsrs/p/7899676.html` to install PHP-7x.

```shell
#rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
#rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
#yum search php72w
#yum install -y php72w php72w-cli php72w-common php72w-devel php72w-embedded php72w-fpm php72w-gd php72w-mbstring php72w-mysqlnd php72w-opcache php72w-pdo php72w-xml 

#whereis php
```

```shell
[root@0001 ~]# php -v
PHP 7.2.14 (cli) (built: Jan 12 2019 12:47:33) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.14, Copyright (c) 1999-2018, by Zend Technologies
```

# Step 3 Install & configure httpd

```shell
# yum install -y httpd
# service httpd start
# chkconfig httpd on

# service php-fpm start
# chkconfig httpd on

# whereis httpd
# vi /etc/httpd/conf/httpd.conf
LoadModule php7_module modules/libphp7.so
AddType application/x-httpd-php .php
DirectoryIndex index.php index.htm index.html

# service httpd restart
```

# Step 4 Test the installation
- Create a file `test.php` under `/var/www/html/`

```shell
vi /var/www/html/test.php
<?php  phpinfo(); ?>
```

- Access from the browser & check `http://localhost/test.php`

# Step 5 Download and install think-php
- Create a folder for it
- Change directory to the folder

```shell
>mkdir tp5
>cd tp5
```

- Clone the source code using `git`

```shell
tp5>cd ..

>git clone https://github.com/top-think/think tp5
Cloning into 'tp5'...
remote: Counting objects: 13548, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 13548 (delta 0), reused 0 (delta 0), pack-reused 13545
Receiving objects: 100% (13548/13548), 23.21 MiB | 256.00 KiB/s, done.
Resolving deltas: 100% (8143/8143), done.

>cd tp5

tp5>git clone https://github.com/top-think/framework thinkphp
Cloning into 'thinkphp'...
remote: Counting objects: 44318, done.
remote: Compressing objects: 100% (32/32), done.
remote: Total 44318 (delta 10), reused 20 (delta 5), pack-reused 44281
Receiving objects: 100% (44318/44318), 12.97 MiB | 495.00 KiB/s, done.
Resolving deltas: 100% (27409/27409), done.
```

# Step 5 Configure for Apache
- Copy the folder `tp5` into `/var/www/html/`

```shell
└─tp5
    ├─application
    │  └─index
    │      └─controller
    ├─config
    ├─extend
    ├─public
    │  └─static
    ├─route
    ├─runtime
    │  └─cache
    ├─thinkphp
    │  ├─lang
    │  ├─library
    │  │  ├─think
    │  │  │  ├─cache
    │  │  │  │  └─driver
    │  │  │  ├─config
    │  │  │  │  └─driver
    │  │  │  ├─console
    │  │  │  │  ├─bin
    │  │  │  │  ├─command
    │  │  │  │  │  ├─make
    │  │  │  │  │  │  └─stubs
    │  │  │  │  │  └─optimize
    │  │  │  │  ├─input
    │  │  │  │  └─output
    │  │  │  │      ├─descriptor
    │  │  │  │      ├─driver
    │  │  │  │      ├─formatter
    │  │  │  │      └─question
    │  │  │  ├─db
    │  │  │  │  ├─builder
    │  │  │  │  ├─connector
    │  │  │  │  └─exception
    │  │  │  ├─debug
    │  │  │  ├─exception
    │  │  │  ├─facade
    │  │  │  ├─log
    │  │  │  │  └─driver
    │  │  │  ├─model
    │  │  │  │  ├─concern
    │  │  │  │  └─relation
    │  │  │  ├─paginator
    │  │  │  │  └─driver
    │  │  │  ├─process
    │  │  │  │  ├─exception
    │  │  │  │  └─pipes
    │  │  │  ├─response
    │  │  │  ├─route
    │  │  │  │  └─dispatch
    │  │  │  ├─session
    │  │  │  │  └─driver
    │  │  │  ├─template
    │  │  │  │  ├─driver
    │  │  │  │  └─taglib
    │  │  │  ├─validate
    │  │  │  └─view
    │  │  │      └─driver
    │  │  └─traits
    │  │      └─controller
    │  └─tpl
    └─vendor
```

- Modify `conf/httpd.conf`, remove the `indexes` for the directory access

```xml
<Directory "${SRVROOT}/htdocs">
    Options FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

- Configure Apache to recognize index.php

```xml
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>
```
- Restart Apache using `httpd.exe -k restart`
- Visit `http://localhost:7070/tp5/public/` and now we see welcome page from think-php


# Reference
- [PHP official site](http://www.php.net/)
- [Centos7 PHP的安装和配置](https://www.cnblogs.com/lightsrs/p/7899676.html)
- [contos7.2编译安装php](https://blog.csdn.net/qq_35884773/article/details/87865415)
- [解决Apache无法解析PHP问题](https://www.cnblogs.com/ningmeng666/p/9860207.html)
- [Guide for Think-php](https://www.kancloud.cn/manual/thinkphp5/118006)
- [Disable Apache to show content of folders](https://blog.csdn.net/a916123063/article/details/52084153)
- [Configure Apache to recognize index.php](https://www.cnblogs.com/crystaltu/p/6068242.html)

## Back to [index](./index.md)