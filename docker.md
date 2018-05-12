# 安装 docker-ce 时报 Depend 缺失的错误

在 Ubuntu 16.04 LTS 安装 Docker 时，由于 Docker 的版本升级可能会带来依赖的升级，而这些依赖在 Ubuntu 16.04 内并没有安装。

常见报错时由以下两个依赖：

* libltdl7_2.4.6
* libseccomp2_2.3.1

可以通过在 https://launchpad.net 网站上下载，以 libseccomp2 为例：

打开 https://launchpad.net/ubuntu/xenial/amd64/libseccomp2/2.3.1-2.1ubuntu2~16.04.1 网站，找到相关的 deb 包。

``` bash
wget http://launchpadlibrarian.net/344879847/libseccomp2_2.3.1-2.1ubuntu2~16.04.1_amd64.deb
dpkg -i libseccomp2_2.3.1-2.1ubuntu2~16.04.1_amd64.deb
```

安装完成后再安装 docker-ce 即可成功：

``` bash
apt install docker-ce
```


## 相关引用

* https://www.jianshu.com/p/92205963ce23
* https://yeasy.gitbooks.io/docker_practice/content/install/ubuntu.html
