## Centos7 安装 Shadowsocks服务器

* 本文环境Centos7.3

## 开始

shadowsocks依赖python运行，可以用`pip`安装，如果没有安装`pip`的就装一下

    curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
    python get-pip.py

安装完成后就可以安装下shadowsocks了

    pip install --upgrade pip
    pip install shadowsocks

编辑配置文件`/etc/shadowsocks.json`

    {
        "server": "0.0.0.0",
        "server_port": 8388,
        "password": "mypassword",
        "method": "aes-256-cfb"
    }

参数说明:
* method为加密方法，可选aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table
* server_port为服务监听端口

## 配置自启动

新建启动脚本文件`/etc/systemd/system/shadowsocks.service`

    [Unit]
    Description=Shadowsocks

    [Service]
    TimeoutStartSec=0
    ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

    [Install]
    WantedBy=multi-user.target

启动

    systemctl enable shadowsocks
    systemctl start shadowsocks

查看状态

    systemctl status -l shadowsocks

## 客户端

windows 下的可以到这里下载

    https://github.com/shadowsocks/shadowsocks-windows
