## 使用Gzip加快网站加载速度

* 本文使用`Nginx`,下面将对它的配置文件进行修改
* 本机`Nginx`安装路径在`/usr/local/nginx`

## nginx.conf
打开配置文件 

    vi /usr/local/nginx/conf/nginx.conf
    
找到`#gzip: on`，默认带`#`号注释了该功能，添加以下代码

    ##
    # Gzip Settings
    ##
    gzip on;
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    gzip_comp_level 5;
    gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php

## 参数解析
**gzip**
语法：gzip on/off
默认值：off
作用域：http,server,location
说明：开启或者关闭 gzip 模块，这里使用 on 表示启动

**gzip_min_length**
语法：gzip_min_length length
默认值：gzip_min_length 0
作用域：http, server, location
说明：设置允许压缩的页面最小字节数，页面字节数从header头中的Content-Length中进行获取。默认值是0，不管页面多大都压缩。建议设置成大于1k的字节数，小于1k可能会越压越大。

**gzip_buffers**
语法: gzip_buffers number size
默认值: gzip_buffers 4 4k/8k
作用域: http, server, location
说明：设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。4 16k 代表以 16k 为单位，按照原始数据大小以 16k 为单位的4倍申请内存。

**gzip_comp_level**
语法: gzip_comp_level 1..9
默认值: gzip_comp_level 1
作用域: http, server, location
说明：gzip压缩比，1 压缩比最小处理速度最快，9 压缩比最大但处理最慢（传输快但比较消耗cpu）。这里设置为 5

**gzip_types**
语法: gzip_types mime-type [mime-type …]
默认值: gzip_types text/html
作用域: http, server, location
说明：匹配MIME类型进行压缩，（无论是否指定）”text/html” 类型总是会被压缩的。这里设置为 text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php。