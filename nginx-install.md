---
title: centos7 安装Nginx
date: 2017-09-09 17:00:00
tags: [hexo, nginx]
categories: Dances with Nginx
---
## 摘要
> 服务器上通常使用Apache或者Nginx进行代理或者负载均衡等功能，相比Apache，Nginx更加轻量，配置也比较简单，静态处理能力比较高。

<!-- more -->

* 本机使用`Centos 7.3`
* 本机使用nginx版本为 `nginx-1.13.5`, 安装nginx到`/usr/local/nginx`

## 下载
直接在官网上下载对应操作系统的版本

    http://nginx.org/en/download.html
使用winSCP或者其他FTP工具上传文件到`/home`
**注意：** 安装里面有`conf/koi-win`的文件不能跟安装后的conf文件在同一个位置，因此建议安装包解压跟nginx存放地址不一样的地方，例如
安装包地址： `/home/nginx-1.13.5`
Nginx安装到的地址： `/usr/local/nginx`

解压

    tar -zxvf nginx-1.13.5.tar.gz

## 安装
依赖，视情况安装
    
    yum install gcc-c++
	yum install -y pcre pcre-devel
	yum install -y zlib zlib-devel
    yum install -y openssl openssl-devel
    
进入刚才解压的文件夹

    cd nginx-1.13.5
    
安装

    ./configure --prefix=/usr/local/nginx --conf-path=/usr/local/nginx/nginx.conf
    make
    make-install

检查是否安装成功

    /usr/local/nginx/sbin/nginx -t
    
## 启动
启动

    /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf 
    
停止，直接杀掉进程号
    
    ps -ef | grep nginx
    kill -9 [你的ngnix进程号]
    
平滑重启

    /usr/local/nginx/sbin/nginx -s reload 