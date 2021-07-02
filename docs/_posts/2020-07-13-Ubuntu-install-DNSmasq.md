---
title: Ubuntu安装DNSmasq搭建自己的DNS解析服务器
date: 2020-07-13 11:29:53
description: 由于一些你懂得原因，国内运营商的dns总是被污染，所以可以自己部署一个dns服务器用来做dns解析，从此告别dns污染
categories:
- 服务器
---
# Ubuntu安装DNSmasq

1. #### 禁用系统dns解析（Dnsmasq 会和本机自带的DNS解析端口冲突，只能禁用）

   ```shell
   $ systemctl disable systemd-resolved.service
   $ service systemd-resolved stop
   ```

2. #### 安装dnsmasq

   ```shell
   $ apt-get update  // 如果更新特别慢 请先更换源
   $ apt-get install dnsmasq
   ```

3. #### 添加dnsmasq的上游DNS地址（因为你的服务器肯定不能解析所有的域名，所以依靠别人的本地解析不了的用别人的dns解析）

   ```shell
   # 新建一个文件，名字随便起，路径随便放，
   # 和下面的启动命令匹配就行了，我这边是放在/etc/resolv.dnsmasq.conf
   
   $ vim /etc/resolv.dnsmasq.conf
   # 添加下面的dns地址
   # 阿里公共dns 
   nameserver 223.5.5.5
   nameserver 223.6.6.6
   ```

4. #### 添加自定义hosts 这里就是你想自定义解析的域名和编辑hosts文件一样，简单方便

   ```shell
   # 新建一个文件，名字随便起，路径随便放，和下面的启动命令匹配就行了
   $ vim /etc/resolv.dnsmasq.conf
   
    xx.xx.xx.xx xxx.com
    # 比如
    123.123.123.123 baidu.com  # ip和域名中间一个空格就行
   ```

5. #### 启动dnsmasq

   ```shell
   # 启动命令看你上面的文件名 user可以不指定
   $ dnsmasq --user=root --resolv-file=/etc/resolv.dnsmasq.conf \
   --addn-hosts=/etc/dnsmasq.hosts