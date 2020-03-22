---
layout:     post
title:      一键安装v2ray
subtitle:   一键安装v2ray + wordpress
date:       2020-03-22
author:     guyang
header-img: "img/post-bg-2018.jpg"
tags:    
    - v2ray,wordpress    
---

### 环境安装：

#### 一键安装v2ray：
```
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh" && chmod +x install.sh && bash install.sh
```
#### 安装MySQL:
```
apt-get install mysql-server
```
#### 安装PHP7:
```
apt-get install php-fpm php-mysql
```
#### 配置Nginx使用PHP7:
 - 修改Nginx的配置文件：
```
vi /etc/nginx/conf/conf.d/v2ray.conf
```
 - 添加nginx对PHP的处理，修改后的配置文件如下所示：
```
index index.php

location /
{
	try_files $uri $uri/ /index.php?$args;
}
rewrite /wp-admin$ $scheme://$host$uri/ permanent;
location ~ \.php$ {
	fastcgi_pass unix:/run/php/php7.0-fpm.sock;
	fastcgi_index index.php;
	fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	include fastcgi_params;
}
```
#### 重启Nginx启动新配置文件:
```
systemctl restart nginx
```

### 下载WordPress:
```
wget http://wordpress.org/latest.tar.gz
```
#### 解压：
```
tar -xzvf latest.tar.gz
```
### 创建WordPress操作的数据库和用户:

#### root密码登录MySQL：
```
mysql -u root -p
```
#### 创建数据库：
```
CREATE DATABASE wordpress;
```
#### 创建用户：
```
CREATE USER wordpress@localhost;
```
#### 设置密码：
```
SET PASSWORD FOR wordpress@localhost=PASSWORD("wordpress");
```
#### 配置权限：
```
GRANT ALL PRIVILEGES ON wordpress.* TO wordpress@localhost IDENTIFIED BY 'wordpress';
```
#### 刷新权限配置：
```
FLUSH PRIVILEGES;
```
#### 退出MySQL：
```
QUIT;
```

### 配置WordPress:
重命名示例文件wp-config（此处的路径/root/wordpress对应你自己的存放路径）
```
mv /root/wordpress/wp-config-sample.php /root/wordpress/wp-config.php

vim /root/wordpress/wp-config.php


https://api.wordpress.org/secret-key/1.1/salt/
```

#### 配置Nginx:
```
cp -r /root/wordpress/* /home/wwwroot/3DCEList
```
#### 修改权限：
```
chown -R www-data:www-data /home/wwwroot/3DCEList
```
#### 重启Nginx:
```
systemctl restart nginx
```

### 配置WordPress
```
https://www.xxx.com/wp-admin/install.php
```
`