## Centos7 修改内核版本安装锐速

* 本机系统Centos7.3

## 开始
### 检查
登录到VPS,查看架构是否支持

    wget http://dl.lancdn.com/landian/tools/vm_check/vm_check.sh && bash vm_check.sh
执行完毕会显示KVM、XEN、OpenVZ，只要不是显示OpenVZ就可以

### 修改内核
由于本机的内核版本不支持锐速的版本，这里附上一个本机测试可用的内核版本

    rpm -ivh http://xz.wn789.com/CentOSkernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm --force
    rpm -qa | grep kernel #查看内核是否安装成功
    
显示有 `kernel-3.10.0-229.1.2.el7.x86_64` 就没问题了

然后重启

    reboot
    # 重启完查看版本
    uname -r

## 安装锐速

    wget http://dl.lancdn.com/landian/tools/serverspeeder-all.sh && bash serverspeeder-all.sh
    
如果没有匹配的版本会自动退出的，需要在更换内核重新尝试一下。
如果有相近版本支持的话，会让你选择，输入数字键选择。

## 锐速常用命令

    service serverSpeeder status     <==检查锐速状态
    ### ServerSpeeder is running!    <==锐速正在运行中
    service serverSpeeder stop       <==停止锐速
    service serverSpeeder start      <==开启锐速
    service serverSpeeder restart    <==重启锐速    
    
## 卸载锐速

    chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f
    
    
## 参考资料
> https://www.landiannews.com/archives/28720.html
> https://www.wn789.com/4689.html