---
title: Traefik使用注意点
date: 2021-04-24 11:29:53
categories:
- Traefik
---
Traefik使用注意点

1. 类似nginx的反向代理

2. 用在docker上面时需要注意

   1. 想要被托管的容器必须和traefik处于同一网络中

   2. 建议关闭主动发现暴露的容器的端口 exposedByDefault: false

   3. 主动将容器暴露给traefik 

      ```
      # 对外暴露容器服务
      traefik.enable=true
      ```

   4. 需要显式的在docker-compose.yml文件中指定一个service

      ```
      # 配置一个服务指向内部端口
      traefik.http.services.whoami-service.loadbalancer.server.port=80
      ```

   5. traefik的docker-compose.yml配置文件如下。需要手动创建外部网络

      ```yaml
      version: "3.7"
      services:
        traefik:
          image: "traefik:latest" # 我们直接部署最新版本，可自行调整
          container_name: "traefik"
          ports:
            - "80:80"
            - "443:443"
          volumes:
            # 绑定我们的配置目录,traefik会自动寻找traefik.yml 等命名的配置文件
            - ./config:/etc/traefik
            - ./letsencrypt:/letsencrypt
            # So that Traefik can listen to the Docker events
            - /var/run/docker.sock:/var/run/docker.sock
          networks:
            - test
        whoami:
          image: containous/whoami
          container_name: whoami
          networks:
            - test
          labels:
            # 对外暴露容器服务
            - traefik.enable=true
            # 配置一个服务指向内部端口
            - traefik.http.services.whoami-service.loadbalancer.server.port=80
      
      networks:
        test:
          external: true
      ```

   6. 配置文件路径  ./config/traefik.yml

      ```yaml
      providers:  # 配置traefik读取动态配置的配置文件，可以和动态配置指定为同一个文件
        file:
          directory: "/etc/traefik"
          filename: "traefik.yml"
          watch: true  #  监听配置文件变动
        docker:
          exposedByDefault: false
      
      log:
        level: DEBUG
      
      entryPoints:  # 定义入口点
        web:
          address: ":80"
        websecure:
          address: ":443"
      
      
      api:
        dashboard: true  # 开启控制面板
        insecure: true
        
      certificatesResolvers:  # 使用该签发方式，一定要确保服务器80端口可以被访问
        mytlschallenge:
          acme:
            email:  "yourname@domain"  # 配置接收证书过期通知邮箱
            storage:  "/letsencrypt/acme.json"
            tlsChallenge: {}
      
      #↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑静态配置修改后需要重启traefik↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
      
      http:
        routers:
          whoami:
            entryPoints:
              - web
            rule: "Path(`/whoami`)"
            service: whoami-service@docker
          api:
            entryPoints:
              - "web"
            rule: "PathPrefix(`/`)"
            service: api@internal
      
      ```

      

