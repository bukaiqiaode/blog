# Step 1 Download php-7 from official web-site
I selected to download php-71 `VC14 x64 Thread Safe (2018-Jul-19 11:54:52)`from the official [site](https://windows.php.net/download#)

# Step 2 Make a copy of the configuration file
- Make a copy of the configuration file `copy php.ini-development php.ini`
- Open the file and change the value for `extension_dir `

```shell
extension_dir = "E:\Downloads\tools\php71\ext"
```

# Step 3 Configure PHP for Apache
- Open Apache's configuration file `conf\httpd.conf`, from `E:\Downloads\tools\Apache24\conf`
- Add `LoadModule php7_module E:/Downloads/tools/php71/php7apache2_4.dll` to the `LoadModule` sections
- Add `PHPIniDir "E:/Downloads/tools/php71/"` to the end of the configuration file.
- Add below `AddType application/x-gzip .gz .tgz`
- Restart Apache by `httpd.exe -k restart`

# Step 4 Test the installation
- Create a file `test.php` under `E:\Downloads\tools\Apache24\htdocs`

```shell
E:\Downloads\tools\Apache24\htdocs>type test.php
<?php  phpinfo(); ?>
```

- Access from the browser & check `http://localhost:7070/test.php`

# Step 5 Download and install think-php
- Create a folder for it
- Change directory to the folder

```shell
E:\Downloads\tools>mkdir tp5
E:\Downloads\tools>cd tp5
E:\Downloads\tools\tp5>
```

- Clone the source code using `git`

```shell
E:\Downloads\tools\tp5>cd ..

E:\Downloads\tools>git clone https://github.com/top-think/think tp5
Cloning into 'tp5'...
remote: Counting objects: 13548, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 13548 (delta 0), reused 0 (delta 0), pack-reused 13545
Receiving objects: 100% (13548/13548), 23.21 MiB | 256.00 KiB/s, done.
Resolving deltas: 100% (8143/8143), done.

E:\Downloads\tools>cd tp5

E:\Downloads\tools\tp5>git clone https://github.com/top-think/framework thinkphp
Cloning into 'thinkphp'...
remote: Counting objects: 44318, done.
remote: Compressing objects: 100% (32/32), done.
remote: Total 44318 (delta 10), reused 20 (delta 5), pack-reused 44281
Receiving objects: 100% (44318/44318), 12.97 MiB | 495.00 KiB/s, done.
Resolving deltas: 100% (27409/27409), done.

E:\Downloads\tools\tp5>
```

# Step 6 Configure for Apache
- Copy the folder `tp5` into `E:\Downloads\tools\Apache24\htdocs`

```shell
E:\Downloads\tools\Apache24\htdocs>tree .
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\TOOLS\APACHE24\HTDOCS
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
- [Official site to download PHP for Windows](https://windows.php.net/download#)
- [Blog to install PHP on Windows](https://www.cnblogs.com/timmmmit/archive/2017/10/22/7709483.html)
- [Guide for Think-php](https://www.kancloud.cn/manual/thinkphp5/118006)
- [Disable Apache to show content of folders](https://blog.csdn.net/a916123063/article/details/52084153)
- [Configure Apache to recognize index.php](https://www.cnblogs.com/crystaltu/p/6068242.html)

## Back to [index](./index.md)