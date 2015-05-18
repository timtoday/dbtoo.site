---
title : 当symfony2遇到GFW
layout :  post
tags : 
- Php
- GFW
---
Symfony2本来是很不错的框架，由于现在版本安装方改成了composer 方式，导致之前在tar.gz zip包这些都用不起了。

于是无聊做了个拆解发现，其实内部下载的URL是：

```
http://get.symfony.com/Symfony_Standard_Vendors_x.x.x.tgz
```
把xxx换成想下载的版本即可

但是symfony2建议使用console方式启动php自带的http服务器来作本地调试，于是apache就会立即出现不适应了：

```
Oops! An Error Occurred

The server returned a "404 Not Found".
```

google了一下，解决方式是，把.htaccess文件调整一下：

```
<IfModule mod_rewrite.c>
    RewriteEngine On

    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ app_dev.php [QSA,L]
</IfModule>
```