---
title: 使用acme.sh结合阿里云服务器自动生成证书
date: 2023-12-24 14:52:49
tags: [acme]
categories: [配置]
---

由于阿里云的免费证书名额已经用完，且最长的有效期只有3个月，故转为acme.sh自动生成以及更新证书的方式

acme.sh 实现了 acme 协议, 可以从 letsencrypt 生成免费的证书.

主要步骤:

1.安装 acme.sh

2.生成证书

在生成证书这个步骤中，我采用的是手动 dns 方式, 手动在域名上添加一条 txt 解析记录, 验证域名所有权。利用阿里云的access key自动集成。

```
acme.sh --issue --dns dns_ali -d example.com -d *.example.com
```
过程有可能很漫长
3.copy 证书到 nginx/apache 或者其他服务


更新证书

Nginx example:

```
acme.sh --install-cert -d example.com \
--key-file       /path/to/keyfile/in/nginx/key.pem  \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd     "service nginx force-reload"
```
证书文件和私钥文件格式自定义，pem和key，不一定都是pem
(一个小提醒, 这里用的是 service nginx force-reload, 不是 service nginx reload, 据测试, reload 并不会重新加载证书, 所以用的 force-reload)

Nginx 的配置 ssl_certificate 使用 /etc/nginx/ssl/fullchain.cer ，而非 /etc/nginx/ssl/<domain>.cer ，否则 SSL Labs 的测试会报 Chain issues Incomplete 错误。

--install-cert命令可以携带很多参数, 来指定目标文件. 并且可以指定 reloadcmd, 当证书更新以后, reloadcmd会被自动调用,让服务器生效.

详细参数请参考: https://github.com/Neilpang/acme.sh#3-install-the-issued-cert-to-apachenginx-etc

值得注意的是, 这里指定的所有参数都会被自动记录下来, 并在将来证书自动更新以后, 被再次自动调用.

