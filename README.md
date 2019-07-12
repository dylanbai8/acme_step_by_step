# 使用 acme 签发 Let's Encrypt SSl 证书 日常使用命令 备记

## ▚ [Linux系统] 1.安装脚本

```
curl https://get.acme.sh | sh
```

## ▚ [Linux系统] 2.签发证书

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

## ▚ [Linux系统] 3.安装证书到指定路径 并开启自动续期（以caddy为例）

```
acme.sh --install-cert -d www.mydomain.com --cert-file /wwwroot/ssl/mydomain.pem --key-file /wwwroot/ssl/mydomain.pem --reloadcmd "systemctl restart caddy"
```

Apache示例：
```
acme.sh --install-cert -d www.mydomain.com \
--cert-file      /path/to/certfile/in/apache/cert.pem \
--key-file       /path/to/keyfile/in/apache/key.pem \
--fullchain-file /path/to/fullchain/certfile/apache/fullchain.pem \
--reloadcmd      "service apache2 force-reload"
```

Nginx示例：
```
acme.sh --install-cert -d www.mydomain.com \
--key-file       /path/to/keyfile/in/nginx/key.pem \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd      "service nginx force-reload"
```

官方文档：

https://github.com/Neilpang/acme.sh

https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E

https://github.com/Neilpang/acme.sh/wiki/How-to-issue-a-cert

https://github.com/Neilpang/acme.sh/wiki/dnsapi

## ▚ [windows系统] 1.安装软件

```
https://github.com/PKISharp/win-acme/releases

下载最新版本 如：win-acme.v2.0.8.356.zip
```

## ▚ [windows系统] 2.签发证书 安装证书到指定路径 并开启自动续期

## 命令行 CMD 模式

修改为自己的 域名、网站根目录、SSL储存路径、续签提醒邮箱

```
wacs.exe --target manual --host www.mydomain.com --validation filesystem --webroot C:\wwwroot --store pemfiles --pemfilespath C:\sslroot --emailaddress email@mydomain.com --accepttos --usedefaulttaskuser
```

## 可视化 UI 模式

可视化步骤比较繁琐 如下：

右键-以管理员身份运行 wacs.exe

```
1.输入：m （生成一个新的证书） 

2.输入：1 （手动输入模式）

3.输入：www.mydomain.com （输入你的域名）

4.输入：直接回车 （设置为默认备注名称）

5.输入：5 （选择域名验证方式、常用验证方式选5、使用DNS TXT记录验证选2）

6.输入：D:\wwwroot （输入网站根目录）

7.输入：n （不复制默认web.config 环境为IIS时可尝试输入y）

8.输入：2 （选择证书格式）

9.输入：3 （将证书签发到指定文件夹）

10.输入：D:\sslroot （输入自定义文件夹路径）

11.输入：3 （不需要额外储存证书）

12.输入：1 （不需要额外的安装步骤）

13.输入：email@mydomain.com （设置续签提醒邮箱）

14.输入：y （查看用户协议）

15.输入：y （同意用户协议）

16.输入：n （使用当前用户 无需指定用户运行定时续签计划任务）


至此所有步骤完成 不出问题的话ssl证书已经签发到指定的文件夹了
```

打开计划任务-计划任务程序库 查看是否存在以下任务：

win-acme renew (acme-v02.api.letsencrypt.org)

不要移动wacs.exe位置 否则续签会出错 程序配置文件储存路径在：

C:\ProgramData\win-acme

特别注意：

当自动续签完成后 由于win-acme并不能自动重启web环境 续签后的证书可能无法自动载入
你可能需要使用其它方法定时重启web环境 以载入新签发的证书

官方文档：

https://github.com/PKISharp/win-acme

https://github.com/PKISharp/win-acme/wiki/Command-line

https://github.com/PKISharp/win-acme/wiki/How-To-Run

使用DNS TXT记录方式验证 可能无法自动续签 因此不管是Linux还是Windows均不建议采用

## ▚ 证书的格式

--cert-file      格式：pem（Nginx、Apache、Caddy）或者 crt （Caddy）

--key-file       格式：pem（Nginx、Apache、Caddy）或者 key（Caddy）

--fullchain-file 格式：pem（Apache）
