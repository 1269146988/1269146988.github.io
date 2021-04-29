---
title: 用DenyHost来防止服务器被恶意ssh爆破
date: 2020-02-15 11:29:53
description: 今天一朋友突然想起来看看服务器有没有被恶意ssh爆破，结果不看不知道，还真的有。还不少，上github看了一下，果然还是有和我们一样的朋友的。。已经有大神写好了插件了，直接用就行了。
categories:
- 服务器
---

#### **今天一朋友突然想起来看看服务器有没有被恶意ssh爆破，结果不看不知道，还真的有。还不少，上github看了一下，果然还是有和我们一样的朋友的。。已经有大神写好了插件了，直接用就行了。**
#### 本来我准备把密码访问关闭，然后端口也改一下，后来想了想，关闭密码访问真的不方便，改端口一样可以扫出来，还不如来点儿狠得，直接拉黑得了。
##    一、DenyHost简介:

> **DenyHosts是基于Python2写的一个程序软件，运行于Linux上预防SSH暴力破解的，它会分析sshd的日志文件（/var/log/auth.log），当发现重复的攻击时就会记录IP到/etc/hosts.deny文件，并且会列入系统防火墙黑名单**
>
> - DenyHosts官网：[点击访问](http://denyhosts.sourceforge.net/)   http://denyhosts.sourceforge.net/

# 重要提示！！！

> #### 使用这个软件之前，最好清空一下已经存在的系统日志，因为软件会扫描已经存在的日志文件，如果他检测到你的允许次数 小于 日志中已经存在的ip的错误次数。那么即使是之前的登录日志中的ip也会被封的。
> #### 所以建议先清理一下旧日志，清理之后一定要重启系统日志服务！！

```shell
$ sudo cat /dev/null > /var/log/auth.log  
$ sudo service rsyslog restart    # 重启系统日志服务 不重启的话，auth.log就不会再有记录了，denyhost也就不会扫描相当于废了
```

# 二、DenyHost安装
**这里以ubuntu为例**
```shell
  $ sudo apt-get update      # 更新ubuntu 软件源：
  $ sudo apt-get install denyhosts   # 安装 DenyHost  提示按 “Y”
  $ tail -f /var/log/auth.log  # 查看登录系统的日志  denyhosts就是扫描的这个日志文件
```

安装完成后程序会自动启动  查看是否启动命令：

```shell
$ sudo ps -ef | grep denyhosts

输出： # 至少有两条记录。下面的一条为ps的进程
root      8680     1  0 13:38 ?         00:00:00 python /usr/sbin/denyhosts --daemon --purge --config=/etc/denyhosts.conf
yourname  15322 14556  0 22:19 pts/0    00:00:00 grep --color=auto denyhosts

```

我今天一安装，程序扫描我的日志文件，居然发现了300多个想尝试登陆我服务器得ip.天呐，
我服务器上是有什么宝吗? ?
## 目录：

>   /var/lib/denyhosts/        #   程序工作目录 所有的日志都在这里面
>   /etc/hosts.deny   # 已加入黑名单的ip


---
# 配置文件详细解读

```ini
SECURE_LOG = /var/log/auth.log   # ssh日志文件  
HOSTS_DENY = /etc/hosts.deny    # 将阻止IP写入到hosts.deny
PURGE_DENY =     # 过多久后清除已经禁止的，空代表永不解禁 其中w代表周，d代表天，h代表小时，s代表秒，m代表分钟
BLOCK_SERVICE = sshd   # 阻止服务名
DENY_THRESHOLD_INVALID = 5  # 允许无效用户（在/etc/passwd未列出）登录失败次数,允许无效用户登录失败的次数.
DENY_THRESHOLD_VALID = 5  #  允许普通用户登录失败的次数
DENY_THRESHOLD_ROOT = 5  #  允许root登录失败的次数
DENY_THRESHOLD_RESTRICTED = 1  #  设定 deny host 写入到该资料夹
WORK_DIR = /var/lib/denyhosts/   # 将deny的host或ip纪录到Work_dir中
SUSPICIOUS_LOGIN_REPORT_ALLOWED_HOSTS = YES
HOSTNAME_LOOKUP=no  # 是否做域名反解
LOCK_FILE = /run/denyhosts.pid  # 将DenyHOts启动的pid纪录到LOCK_FILE中，已确保服务正确启动，防止同时启动多个服务。
IPTABLES = /sbin/iptables  # 防火墙脚本文件
ADMIN_EMAIL = youremailaddress@domain.com  # 设置接收通知邮件的地址 多个 ,分隔
SMTP_HOST = localhost  # SMTP服务器地址
SMTP_PORT = 25  # 端口
SMTP_USERNAME = senderemailaddress@domain.com  # 接收的邮箱看到的发送者邮箱地址
SMTP_PASSWORD= password   # 使用第三方邮箱的服务一般都有授权码 不是邮箱的密码哦
SMTP_FROM = DenyHosts senderemailaddress@domain.com  # 显示发件人是谁 
SMTP_SUBJECT = DenyHosts Report  # 邮件主题
AGE_RESET_VALID=1d  # 有效用户登录失败计数归零的时间
AGE_RESET_ROOT=1d  # root用户登录失败计数归零的时间
AGE_RESET_RESTRICTED=5d  # 用户的失败登录计数重置为0的时间
AGE_RESET_INVALID=10d  # 无效用户登录失败计数归零的时间
DAEMON_LOG = /var/log/denyhosts  自己的日志文件
DAEMON_SLEEP = 30s
DAEMON_PURGE = 5m  # 该项与PURGE_DENY 设置成一样，也是清除hosts.deniedssh 用户的时间
```

## 相关命令

```shell
$ sudo service denyhosts start  
$ sudo service denyhosts stop
$ sudo service denyhosts restart
$ sudo service denyhosts status   # 如果启动失败可以查看错误信息
$ sudo dpkg -l|grep denyhost   # 查看是否安装了 denyhost
$ sudo apt-get purge denyhost  # 卸载denyhost
```
---
### 误封了自己的ip怎么办？我刚开始安装，为了测试，把自己ip封了，整了好久才搞好。
##### 我之前误封之后，把ip从 hosts.deny里面移除了也不行。后来发现官网早有解释。
![官方解释](https://img-blog.csdnimg.cn/20190828225825997.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEwMzMwMA==,size_16,color_FFFFFF,t_70)
#### 但是问题来了，我按照他的方法操作了，还是不行，后来我直接把这个软件卸载了，并且重启了服务器，发现我的ip依然被封禁，那这就不用想了，肯定是系统级别的禁止。果不其然。执行 
```shell
$ sudo iptables -L  # 查看本机所有防火墙的防火墙规则
```

#### 发现了我自己的ip。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190828230254113.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEwMzMwMA==,size_16,color_FFFFFF,t_70)
#### 由于官方并没有提供命令移除被封禁的ip，所以我在这里帮大家写好了一键移除的脚本。
```# 
IP=$1
if [ -n "$IP" ];then
    if [[ $IP =~ ^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}$ ]];then
        service denyhosts stop
        sed -i "/$IP/d" /etc/hosts.deny
        sed -i "/$IP/d" /var/lib/denyhosts/hosts-valid
        sed -i "/$IP/d" /var/lib/denyhosts/users-hosts
        sed -i "/$IP/d" /var/lib/denyhosts/hosts
        sed -i "/$IP/d" /var/lib/denyhosts/hosts-root
        sed -i "/$IP/d" /var/lib/denyhosts/hosts-restricted
        iptables -D INPUT -s $IP -j DROP
        echo $IP remove from Denyhosts
        service denyhosts start
    else
        echo "This is not IP"
    fi
else
    echo "IP is empty"
fi
```

> 用法 新建一个后缀.sh的文件，将下面代码复制进去，然后执行
>
> ```shell
> $ sudo chmod +x name.sh   # 添加可执行权限
> ```
>
> 解除封禁ip命令
>
> ```shell
> $ sudo ./name.sh 127.0.0.1
> ```
## 添加白名单ip ⬇️ 官网介绍
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190828232528869.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEwMzMwMA==,size_16,color_FFFFFF,t_70)
### 命令

```shell
$ sudo vim /var/lib/denyhosts/allowed-hosts  
# 然后将你的白名单ip写进去  一行一个，最后一个字符支持通配符 *
```

---
### 现在看我的收件箱：
大概看了下ip，有国外也有国内的。。。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190828230739677.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEwMzMwMA==,size_16,color_FFFFFF,t_70)