---
title: Docker+Traefik部署Gitea并开启HTTPS+SSH容器直通
date: 2021-04-24 11:29:53
description: Docker+Traefik部署Gitea并开启HTTPS+SSH容器直通
categories:
- Traefik
- Docker
---

## 前言：

##### 	最近在学习docker swarm，然后发现如果使用集群的话，反向代理如果还需要手动配置服务发现就太麻烦了。所以又发现了traefik这个可以支持docker集群模式的主动服务发现工具。

### traefik介绍：[官网文档](https://doc.traefik.io/traefik/)

- 一个反向代理的工具，可以理解成一个不需要手动改配置的nginx。

### Gitea介绍：[Docker安装官方文档](https://docs.gitea.io/zh-cn/install-with-docker/)

- 一个轻量化的git代码仓库。小团队和个人使用完全足够了。 1h1g服务器也能跑的飞起

### SSH容器直通:

- 让gitea容器和宿主机共用宿主机的ssh通道，从而达到一个22端口宿主机和gitea服务一起使用

- 如果不使用ssh容器直通，你的gitea还想使用ssh功能的话，就必须再加一个端口，比如2224，这样的话也不是不能用，只不过是在网页上面点击ssh的clone链接的时候会带上一个端口号。

使用ssh容器直通后复制的链接是这样的：

```sh
git@yourdomain.com:yourname/test.git
```

不使用ssh容器直通后复制的链接是这样的：

```shell
 ssh://git@yourdomain.com:2224/yourname/test.git
```

##### 	使用起来没有任何区别，唯一的区别就是链接长得不一样

------

## 服务器环境：

- 安装前需要先在服务器上面安装Docker和Docker-compose，本篇文章不做安装这两个的教程。请根据自己的服务器系统版本来安装，并改好docker的仓库源地址
- 开启HTTPS需要一个解析到你服务器的域名（国内还需要备案）

------

## 开始安装：

- ### 首先创建一个名为git的用户，并加入git用户组

```shell
sudo groupadd -g 888 git && adduser --uid 888 --gid 888 --system --shell /bin/bash --gecos "Git Version Control" --disabled-password --home /home/git git

# 输出
Adding system user `git' (UID 888) ...
Adding new user `git' (UID 888) with group `git' ...
Creating home directory `/home/git' ...
```

- #### 给git用户创建秘钥。直接回车回车回车就行了

```shell
sudo -u git ssh-keygen -t rsa -b 4096 -C "Gitea Host Key" && echo "$(cat /home/git/.ssh/id_rsa.pub)" >> /home/git/.ssh/authorized_keys

# 输出
Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):
Created directory '/home/git/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/git/.ssh/id_rsa.
Your public key has been saved in /home/git/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:kHsow5CKInZwlzShXQOJ7xs1kBf1RN18j3l8BenFtbw Gitea Host Key
The key's randomart image is:
+---[RSA 4096]----+
|    .==+o..o. ++o|
|   o+++o. o  .oo*|
| .oo.+=    . . B+|
|..oo.. *      + *|
|=. .= + S      E.|
|+ .  = .         |
|      o          |
|     .           |
|                 |
+----[SHA256]-----+
```

- #### 编写一个可执行文件（很重要，ssh容器直通就是执行这个文件）

```shell
sudo mkdir -p /app/gitea && sudo echo 'ssh -p 2222 -o StrictHostKeyChecking=no git@127.0.0.1 "SSH_ORIGINAL_COMMAND=\"$SSH_ORIGINAL_COMMAND\" $0 $@"' >> /app/gitea/gitea && sudo chmod +x /app/gitea/gitea
```

- #### 配置traefik，配置有很多种方式，我这里选择以`yml`配置文件的方式进行配置, 并且动态和静态配置在一起。首先新建一个`traefik.yml`文件作为traefik的配置文件，名字是什么不重要。以`.yml`结尾就可以。配置文件放在 `./config/traefik.yml`

```yaml
providers:  # 配置traefik读取动态配置的配置文件，可以和动态配置指定为同一个文件
  file:
    directory: "/etc/traefik"
    filename: "traefik.yml"  # 这里是指需要加载的动态配置文件名
    watch: true  # 是否监听配置文件变动
  docker:
    exposedByDefault: false  # 建议禁止自动发信暴露的容器

log:
  level: INFO  # 日志输出等级

entryPoints:  # 定义入口点
  http:
    address: ":80"
  https:
    address: ":443"


certificatesResolvers:  # 使用该签发方式，一定要确保服务器80端口可以被访问
  myresolver:
    acme:
      email:  "yourdomain@qq.com"  # 配置接收证书过期通知邮箱
      storage:  "/letsencrypt/acme.json"
      httpChallenge:
        # used during the challenge
        entryPoint: http

api:
  dashboard: true  # 开启控制面板
  insecure: true  # 以不安全的方式开启
#↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑静态配置修改后需要重启traefik↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑



```

- ### 配置文件准备好后，使用`docker-compose up -d`启动即可，配置如下,配置了自动将http转发至https中间件

```yaml
version: "3"
services:
  traefik:
    image: "traefik:latest" # 我们直接部署最新版本，可自行调整
    container_name: "traefik"
    ports:
      - "443:443"
      - "80:80"
      - "8080:8080"
    volumes:
      # 将配置文件挂载进容器
      - ./config:/etc/traefik
      - ./letsencrypt:/letsencrypt
      - /var/run/docker.sock:/var/run/docker.sock
  gitea:
    image: gitea/gitea:latest
    container_name: gitea
    environment:
      - USER_UID=888
      - USER_GID=888
    restart: unless-stopped
    volumes:
      - /home/git/.ssh/:/data/git/.ssh
      - gitea_data:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "127.0.0.1:2222:22"
    labels:
        # 对外暴露容器服务
        - traefik.enable=true
        # 创建一个https路由
        - traefik.http.routers.gitea-https.rule=Host(`yourdomain.com`)
        # 为该路由开启https
        - traefik.http.routers.gitea-https.tls=true
        # 指定证书签发对象
        - traefik.http.routers.gitea-https.tls.certresolver=myresolver
        # 指定该路由对应的service
        - traefik.http.routers.gitea-https.service=gitea

        # 创建一个http路由
        - traefik.http.routers.gitea-http.rule=Host(`yourdomain.com`)
        # 指定该路由对应的service
        - traefik.http.routers.gitea-http.service=gitea

        # 给路由指定入站端口
        - traefik.http.routers.gitea-http.entrypoints=http
        - traefik.http.routers.gitea-https.entrypoints=https

        # 创建一个服务用来暴露容器内部的3000端口
        - traefik.http.services.gitea.loadbalancer.server.port=3000
        # 创建一个中间件 对于开启了tls的路由必须为http和https指定不同的路由，因为开启tls后的路由不再接收http请求
        - traefik.http.middlewares.redirect-http-to-https.redirectscheme.scheme=https
        # 转发参数
        - traefik.http.middlewares.redirect-http-to-https.redirectscheme.permanent=true
        # 将该中间件加载http路由上面
        - traefik.http.routers.gitea-http.middlewares=redirect-http-to-https

volumes:
  gitea_data:
```



