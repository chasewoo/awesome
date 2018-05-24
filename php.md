# install PHP 7.1 in unbuntu 16.04

``` bash
sudo apt-get install -y python-software-properties
sudo add-apt-repository -y ppa:ondrej/php
sudo apt-get update -y
```

``` bash
apt-cache pkgnames | grep php7.1
```

``` bash
# install PHP 7.1
apt-get install php7.1 php7.1-fpm php7.1-cli php7.1-common php7.1-mbstring php7.1-gd php7.1-intl php7.1-xml php7.1-mysql php7.1-mcrypt php7.1-zip
```

如果报错，缺少依赖，查看根目录下其他文章


### 引用

* https://www.vultr.com/docs/how-to-install-and-configure-php-70-or-php-71-on-ubuntu-16-04
* https://blog.csdn.net/wuzuodingfeng/article/details/54638999
