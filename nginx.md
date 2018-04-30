#### NGINX 在linux(CENTOS)环境下的安装 NGINX作为一个高性能和反向代理服务器,大多数情况下被用来作为负载均衡服务器和反向代理,本文主要讲述如何在linux环境下安装nginx服务器**安装NGINX之前，首先需要安装gcc ,gcc-c++编译环境、zlib数据压缩用的函式库、OPENSSL安全套接字层密码库、PCRE Perl库兼容的正则表达式库**首先安装GCC 、GCC-G++
```
yum install -y gcc
yum install -y gcc-c++
```
安装完成后就可以其他库,先下载所需库源码文件:
**下载ZLIB,并解压**
```
wget https://nchc.dl.sourceforge.net/project/libpng/zlib/1.2.11/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
```
**下载pcre,并解压**
```
wget https://nchc.dl.sourceforge.net/project/pcre/pcre/8.41/pcre-8.41.tar.gz
tar -zxvf  pcre-8.41.tar.gz
```
**下载openssl,并解压**
```
wget https://www.openssl.org/source/openssl-1.0.2n.tar.gz
tar -zxvf openssl-1.0.2n.tar.gz```
**安装ZLIB**
```cd zlib-1.2.11
./configure && make && make install```
**安装PCRE**
```cd pcre-8.41
./configure && make && make install
```
**安装OPENSSL**
```cd openssl-1.0.2n
./config && make && make install
```
**下载NGINX安装包，并安装**
```
wget http://nginx.org/download/nginx-1.13.8.tar.gz
tar -zxvf nginx-1.13.8.tar.gz
cd nginx-1.13.8
./configure --sbin-path=/usr/local/nginx/nginx 
--conf-path=/usr/local/nginx/nginx.conf 
--pid-path=/usr/local/nginx/nginx.pid 
--with-http_ssl_module 
--with-pcre=/opt/app/openet/oetal1/chenhe/pcre-8.37 
--with-zlib=/opt/app/openet/oetal1/chenhe/zlib-1.2.8 
--with-openssl=/opt/app/openet/oetal1/chenhe/openssl-1.0.1t 
make && make install
```
- --sbin-path=path 设置nginx的可执行文件的路径，默认为  prefix/sbin/nginx.
- --conf-path=path  设置在nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf.
- --with-pcre 设置PCRE库的源码路径。PCRE库的源码（版本4.4 - 8.30）需要从PCRE网站下载并解压。其余的工作是Nginx的./ configure和make来完成。正则表达式使用在location指令和 ngx_http_rewrite_module 模块中
- --with-zlib=path —设置的zlib库的源码路径。要下载从 zlib（版本1.1.3 - 1.2.5）的并解压。
其余的工作是Nginx的./ configure和make完成。
ngx_http_gzip_module模块需要使用zlib最后进入nginx文件夹,执行命令:```./nginx ```启动。
到这里nginx基本就安装完成了
> 相关链接:- [zlib官网](http://www.zlib.net/)
- [pcre官网](http://www.pcre.org/)
- [openssl官网](https://www.openssl.org/)
- [nginx官网](http://nginx.org/)
- [nginx中文文档](http://www.nginx.cn/doc/)
