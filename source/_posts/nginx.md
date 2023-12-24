---
title: 网站https配置文件模版
date: 2023-11-14 17:08:13
tags: [Nginx]
categories: [配置]
---
```
server {
    listen 80;
    server_name www.mindmapping.zhuminhao.top mindmapping.zhuminhao.top;
    return 301 https://mindmapping.zhuminhao.top$request_uri;
}
server {
     #HTTPS的默认访问端口443。
     #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
     listen 443 ssl;

     #填写证书绑定的域名
     server_name mindmapping.zhuminhao.top;

     #填写证书文件名称
     ssl_certificate cert/mindmapping.zhuminhao.top.pem;
     #填写证书私钥文件名称
     ssl_certificate_key cert/mindmapping.zhuminhao.top.key;

     ssl_session_cache shared:SSL:1m;
     ssl_session_timeout 5m;

     #自定义设置使用的TLS协议的类型以及加密套件（以下为配置示例，请您自行评估是否需要配置）
     #TLS协议版本越高，HTTPS通信的安全性越高，但是相较于低版本TLS协议，高版本TLS协议对浏览器的兼容性较差。
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
     ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;

     #表示优先使用服务端加密套件。默认开启
     ssl_prefer_server_ciphers on;
    location / {
        alias /app/mindmapping/;
        try_files $uri $uri/ /assets /index.html;
        index index.html index.htm;
    }
}
server {
    listen 443 ssl;
    server_name www.mindmapping.zhuminhao.top;
    return 301 $scheme://mindmapping.zhuminhao.top$request_uri;
}
```