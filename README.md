# 使用 acme 签发 Let's Encrypt SSl 证书 日常使用命令 备记

## ▚ [Linux系统] 安装脚本

```
curl https://get.acme.sh | sh
```

## ▚ [Linux系统] 签发证书

## 普通模式

```
acme.sh --issue -d www.mydomain.com --webroot /wwwroot/
```

## DNS API模式

1.阿里云 Aliyun domain API

接口申请地址：https://usercenter.console.aliyun.com/#/manage/ak

```
export Ali_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export Ali_Secret="jlsdflanljkljlfdsaklkjflsa"

acme.sh --issue --dns dns_ali -d www.mydomain.com
```

2.腾讯云 DNSPod.cn domain API

接口申请地址：https://www.dnspod.cn/console/user/security

```
export DP_Id="1234"
export DP_Key="sADDsdasdgdsf"

acme.sh --issue --dns dns_dp -d www.mydomain.com
```

3.CloudFlare解析 CloudFlare domain API

接口申请地址：https://www.cloudflare.com/a/profile

```
export CF_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export CF_Email="xxxx@sss.com"

acme.sh --issue --dns dns_cf -d www.mydomain.com
```

（密钥储存位置：~/.acme.sh/account.conf）

## ▚ [Linux系统] 安装证书到指定路径 并开启自动续期（以caddy为例）

```
acme.sh --install-cert -d www.mydomain.com --cert-file /wwwroot/ssl/mydomain.crt --key-file /wwwroot/ssl/mydomain.key --reloadcmd "systemctl restart caddy"
```

Apache示例：
```
acme.sh --install-cert -d example.com \
--cert-file      /path/to/certfile/in/apache/cert.pem  \
--key-file       /path/to/keyfile/in/apache/key.pem  \
--fullchain-file /path/to/fullchain/certfile/apache/fullchain.pem \
--reloadcmd     "service apache2 force-reload"
```

Nginx示例：
```
acme.sh --install-cert -d example.com \
--key-file       /path/to/keyfile/in/nginx/key.pem  \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd     "service nginx force-reload"
```

官方文档：

https://github.com/Neilpang/acme.sh

https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E

https://github.com/Neilpang/acme.sh/wiki/How-to-issue-a-cert

https://github.com/Neilpang/acme.sh/wiki/dnsapi

## ▚ [windows系统] 安装软件

```
https://github.com/PKISharp/win-acme/releases

下载最新版本 如：win-acme.v2.0.8.356.zip
```

## ▚ [windows系统] 签发证书

```
wacs.exe --target manual --host www.mydomain.com --webroot C:\wwwroot --emailaddress email@mydomain.com --accepttos
```

官方文档：

https://github.com/PKISharp/win-acme

https://github.com/PKISharp/win-acme/wiki/Command-line

https://github.com/PKISharp/win-acme/wiki/How-To-Run



