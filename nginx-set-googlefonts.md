---
title: Nginx设置google fonts 反向代理
date: 2017-09-11 02:32:21
tags: [hexo, nginx]
categories: Dances with Nginx
---
## 摘要
> google已经远离了china，使用`fonts.googleapis.com`会迟迟加载不下来，360CDN原本可以解决这个问题，但是经常抽风访问不了，因此使用自己的服务器搭建了fonts的代理，加快了网站获取fonts的速度。

<!-- more -->

* 本文已经预先安装好了`Nginx-1.13.5`
* Nginx安装目录为`/usr/local/nginx`

## 开始
打开配置文件`/usr/local/nginx/conf/nginx.conf`, 加入以下代码

    server {
        listen 80;
    
        server_name fonts.caixuhui.com; #改为你的域名
        valid_referers server_name *.caixuhui.com caixuhui.com; #限定只能从这些域名下访问，如果想做公益就删掉这4行代码
        if ($invalid_referer) {
            return 403;
        }
    
        location /css {
            sub_filter 'fonts.gstatic.com' 'fonts.caixuhui.com';#后面这个域名改为自己的，意思是把获取到的资源内的fonts.gstatic.com替换成我们自己的域名
            sub_filter_once off;
            sub_filter_types text/css;
            proxy_pass_header Server;
            proxy_set_header Host fonts.googleapis.com;
            proxy_set_header Accept-Encoding '';
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_pass http://fonts.googleapis.com:80;
        }
    
        location / {
            proxy_pass_header Server;
            proxy_set_header Host fonts.gstatic.com;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_pass http://fonts.gstatic.com:80;
        }
    }

## 可能遇到的问题
**sub_filter报错**
原因是Nginx没有安装`HttpSubModule`模块
**解决方法**
下载需要的文件
    
    cd /home
    git clone git://github.com/yaoweibin/ngx_http_substitutions_filter_module.git

重新编译Nginx, 加入httpSubModule, _如果原来有引用了哪些模块，请自行加上_
    
    ./configure --prefix=/usr/local/nginx --conf-path=/usr/local/nginx/conf/nginx.conf --with-http_sub_module  --add-module=/home/ngx_http_substitutions_filter_module
    
    make
只要到`make`就好，不用`make install`, 然后替换编译好的文件到原来的安装目录

    cd objs/
    cp nginx /usr/local/nginx/sbin/

然后重启Nginx就完成了 



## 引用资料
> http://wangxianyuan.com/post/nginx-reverse-proxy-with-google-fonts