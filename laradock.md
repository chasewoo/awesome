# 深入研究 Laradock 的功能

## 第一步设置

假设目录：

``` js
someapp
  |- someapp-server
  |- someapp-site
  |- someapp-admin
  |- laradock
```

在 someapp 目录下：

``` js
git clone https://github.com/Laradock/laradock.git
```

进入 laradock 目录，将配置文件复制为 .env

``` js
cp env-example .env
```

接着打开 .env 文件进行配置

``` js
vim .env
```

修改项目名称

``` bash
# Define the prefix of container names. This is useful if you have multiple projects that use laradock to have seperate containers per project.
COMPOSE_PROJECT_NAME=someapp
```

为墙修改源

``` bash
# If you need to change the sources (i.e. to China), set CHANGE_SOURCE to true
CHANGE_SOURCE=true
```

关闭 Node 安装（看 PHP 项目需要）
``` bash
WORKSPACE_INSTALL_NODE=false
```

关闭 yarn 安装（看 PHP 项目需要）
``` bash
WORKSPACE_INSTALL_YARN=false
```

``` bash
WORKSPACE_INSTALL_NPM_GULP=false
WORKSPACE_INSTALL_NPM_BOWER=false
WORKSPACE_INSTALL_NPM_VUE_CLI=false
```

该参数根据项目的需要，一般不会直接占用 80，443 端口，如果改为 8000 端口，可能与后面的其他端口号冲突，都建议修改

``` bash
NGINX_HOST_HTTP_PORT=8888
NGINX_HOST_HTTPS_PORT=8889
```

设置 nginx 配置：nginx/sites/default.conf

``` bash
server {

    listen 80;
    listen [::]:80;

    server_name localhost;
    root /var/www/public;
    index index.php index.html index.htm;

    location / {
         try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_pass php-upstream;
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fixes timeouts
        fastcgi_read_timeout 600;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }

    location /.well-known/acme-challenge/ {
        root /var/www/letsencrypt/;
        log_not_found off;
    }

    error_log /var/log/nginx/laravel_error.log;
    access_log /var/log/nginx/laravel_access.log;
}
```

Fix MySQL 5.7 build bug
``` bash
### MySQL ################################################
    mysql:
      build:
        context: ./mysql
        args:
          - MYSQL_VERSION=${MYSQL_VERSION}
      environment:
        - MYSQL_DATABASE=${MYSQL_DATABASE}
        - MYSQL_USER=${MYSQL_USER}
        - MYSQL_PASSWORD=${MYSQL_PASSWORD}
        - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
        - TZ=${WORKSPACE_TIMEZONE}
      volumes:
        # without DATA_SAVE_PATH
        - mysql:/var/lib/mysql
        # remove docker-entrypoint-initdb.d
        # - ${MYSQL_ENTRYPOINT_INITDB}:/docker-entrypoint-initdb.d
      ports:
        - "${MYSQL_PORT}:3306"
      networks:
        - backend
```
