# 【Sing-box 全家桶】

* * *

# 目录

- [1.更新信息](README.md#1更新信息)
- [2.项目特点](README.md#2项目特点)
- [3.Sing-box for VPS 运行脚本](README.md#3sing-box-for-vps-运行脚本)
- [4.无交互极速安装](README.md#4无交互极速安装)
- [5.Token Argo Tunnel 方案设置任意端口回源以使用 cdn](README.md#5token-argo-tunnel-方案设置任意端口回源以使用-cdn)
- [6.Vmess / Vless 方案设置任意端口回源以使用 cdn](README.md#6vmess--vless-方案设置任意端口回源以使用-cdn)
- [7.Docker 和 Docker compose 安装](README.md#7docker-和-docker-compose-安装)
- [8.Nekobox 设置 shadowTLS 方法](README.md#8nekobox-设置-shadowtls-方法)
- [9.主体目录文件及说明](README.md#9主体目录文件及说明)
- [10.鸣谢下列作者的文章和项目](README.md#10鸣谢下列作者的文章和项目)
- [11.免责声明](README.md#11免责声明)


* * *


## 2.项目特点:

* 一键部署多协议，可以单选、多选或全选 ShadowTLS v3 / XTLS Reality / Hysteria2 / Tuic V5 / ShadowSocks / Trojan / Vmess + ws / Vless + ws + tls / H2 Reality / gRPC Reality / AnyTLS, 总有一款适合你
* 所有协议均不需要域名，可选 Cloudflare Argo Tunnel 内网穿透以支持传统方式为 websocket 的协议
* 节点信息输出到 V2rayN / Clash Verge / 小火箭 / Nekobox / Sing-box (SFI, SFA, SFM)，订阅自动适配客户端，一个订阅 url 走天下
* 自定义端口，适合有限开放端口的 nat 小鸡
* 内置 warp 链式代理解锁 chatGPT
* 智能判断操作系统: Ubuntu 、Debian 、CentOS 、Alpine 和 Arch Linux,请务必选择 LTS 系统
* 支持硬件结构类型: AMD 和 ARM，支持 IPv4 和 IPv6
* 无交互极速安排模式: 一个回车完成 11 个协议的安装


## 3.Sing-box for VPS 运行脚本:

* 首次运行
```
bash <(wget -qO- https://raw.githubusercontent.com/fscarmen/sing-box/main/sing-box.sh)
```

* 再次运行
```
sb
```

  | Option 参数      | Remark 备注 |
  | --------------- | ------ |
  | -c              | Chinese 中文 |
  | -e              | English 英文 |
  | -u              | Uninstall 卸载 |
  | -n              | Export Nodes list 显示节点信息 |
  | -p <start port> | Change the nodes start port 更改节点的起始端口 |
  | -d              | Change CDN 更换 CDN |
  | -s              | Stop / Start the Sing-box service 停止/开启 Sing-box 服务 |
  | -a              | Stop / Start the Argo Tunnel service 停止/开启 Argo Tunnel 服务 | 
  | -v              | Sync Argo Xray to the newest 同步 Argo Xray 到最新版本 |
  | -b              | Upgrade kernel, turn on BBR, change Linux system 升级内核、安装BBR、DD脚本 |
  | -r              | Add and remove protocols 添加和删除协议 |



### 参数说明
| Key 大小写不敏感（Case Insensitive）| Value |
| --------------- | ----------- |
| --LANGUAGE | c=中文;  e=英文 |
| --CHOOSE_PROTOCOLS | 可多选，如 bcdfk<br> a=全部<br> b=XTLS + reality<br> c=hysteria2<br> d=tuic<br> e=ShadowTLS<br> f=shadowsocks<br> g=trojan<br> h=vmess + ws<br> i=vless + ws + tls<br> j=H2 + reality<br> k=gRPC + reality<br> l=AnyTLS |
| --START_PORT | 100 - 65520 |
| --PORT_NGINX | n=不需要订阅，或者 100 - 65520 |
| --SERVER_IP | IPv4 或 IPv6 地址，不需要中括号 |
| --CDN | 优选 IP 或者域名，如 --CHOOSE_PROTOCOLS 是 [a,h,i] 时需要 |
| --VMESS_HOST_DOMAIN | vmess sni 域名，如 --CHOOSE_PROTOCOLS 是 [a,h] 时需要 |
| --VLESS_HOST_DOMAIN | vless sni 域名，如 --CHOOSE_PROTOCOLS 是 [a,i] 时需要 |
| --UUID_CONFIRM | 协议的 uuid 或者 password |
| --ARGO | 是否使用 Argo Tunnel，如果是填 true，如果使用 Origin rules，则可以忽略本 Key |
| --ARGO_DOMAIN | 固定 Argo 域名，即是 Json 或者 Token 隧道的域名 |
| --ARGO_AUTH | Json 或者 Token 隧道的内容 |
| --PORT_HOPPING_RANGE | hysteria2 跳跃端口范围，如 50000:51000 |
| --NODE_NAME_CONFIRM | 节点名 |





### 常用指令
| 功能 | 指令 |
| ---- | ---- |
| 查看节点信息 | `docker exec -it sing-box cat list` |
| 查看容器日志 | `docker logs -f sing-box` |
| 更新 Sing-box 版本 | `docker exec -it sing-box bash init.sh -v` |
| 查看容器内存,CPU，网络等资源使用情况 | `docker stats sing-box` |
| 暂停容器 | docker: `docker stop sing-box`</br> compose: `docker-compose stop` |
| 停止并删除容器 | docker: `docker rm -f sing-box`</br> compose: `docker-compose down` |
| 删除镜像 | `docker rmi -f fscarmen/sb:latest` |



### 参数说明
| 参数 | 是否必须 | 说明 |
| --- | ------- | --- |
| -p /tcp | 是 | 宿主机端口范围:容器 sing-box 及 nginx 等 tcp 监听端口 |
| -p /udp | 是 | 宿主机端口范围:容器 sing-box 及 nginx 等 udp 监听端口 |
| -e START_PORT | 是 | 起始端口 ，一定要与端口映射的起始端口一致 |
| -e SERVER_IP | 是 | 服务器公网 IP |
| -e XTLS_REALITY | 是 |    true 为启用 XTLS + reality，不需要的话删除本参数或填 false |
| -e HYSTERIA2 | 是 |       true 为启用 Hysteria v2 协议，不需要的话删除本参数或填 false |
| -e TUIC | 是 |            true 为启用 TUIC 协议，不需要的话删除本参数或填 false |
| -e SHADOWTLS | 是 |       true 为启用 ShadowTLS 协议，不需要的话删除本参数或填 false |
| -e SHADOWSOCKS | 是 |     true 为启用 ShadowSocks 协议，不需要的话删除本参数或填 false |
| -e TROJAN | 是 |          true 为启用 Trojan 协议，不需要的话删除本参数或填 false |
| -e VMESS_WS | 是 |        true 为启用 VMess over WebSocket 协议，不需要的话删除本参数或填 false |
| -e VLESS_WS | 是 |        true 为启用 VLess over WebSocket 协议，不需要的话删除本参数或填 false |
| -e H2_REALITY | 是 |      true 为启用 H2 over reality 协议，不需要的话删除本参数或填 false |
| -e GRPC_REALITY | 是 |    true 为启用 gRPC over reality 协议，不需要的话删除本参数或填 false |
| -e ANYTLS | 是 |          true 为启用 AnyTLS 协议，不需要的话删除本参数或填 false |
| -e UUID | 否 | 不指定的话 UUID 将默认随机生成 |
| -e CDN | 否 | 优选域名，不指定的话将使用 www.csgo.com |
| -e NODE_NAME | 否 | 节点名称，不指定的话将使用 sing-box |
| -e ARGO_DOMAIN | 否 | Argo 固定隧道域名 , 与 ARGO_DOMAIN 一并使用才能生效 |
| -e ARGO_AUTH | 否 | Argo 认证信息，可以是 Json 也可以是 Token，与 ARGO_DOMAIN 一并使用才能生效，不指定的话将使用临时隧道 |


## 8.Nekobox 设置 shadowTLS 方法
1. 复制脚本输出的两个 Neko links 进去
<img width="630" alt="image" src="https://github.com/fscarmen/sing-box/assets/62703343/db5960f3-63b1-4145-90a5-b01066dd39be">

2. 设置链式代理，并启用
右键 -> 手动输入配置 -> 类型选择为 "链式代理"。

点击 "选择配置" 后，给节点起个名字，先后选 1-tls-not-use 和 2-ss-not-use，按 enter 或 双击 使用这个服务器。一定要注意顺序不能反了，逻辑为 ShadowTLS -> ShadowSocks。

<img width="408" alt="image" src="https://github.com/fscarmen/sing-box/assets/62703343/753e7159-92f9-4c88-91b5-867fdc8cca47">


## 9.主体目录文件及说明

```
/etc/sing-box/                               # 项目主体目录
|-- cert                                     # 存放证书文件目录
|   |-- cert.pem                             # SSL/TLS 安全证书文件
|   `-- private.key                          # SSL/TLS 证书的私钥信息
|-- conf                                     # sing-box server 配置文件目录
|   |-- 00_log.json                          # 日志配置文件
|   |-- 01_outbounds.json                    # 服务端出站配置文件
|   |-- 02_endpoints.json                    # 配置 endpoints，添加 warp 账户信息配置文件
|   |-- 03_route.json                        # 路由配置文件，chatGPT 使用 warp ipv6 链式代理出站
|   |-- 04_experimental.json                 # 缓存配置文件
|   |-- 05_dns.json                          # DNS 规则文件
|   |-- 06_ntp.json                          # 服务端时间同步配置文件
|   |-- 11_xtls-reality_inbounds.json        # Reality vision 协议配置文件
|   |-- 12_hysteria2_inbounds.json           # Hysteria2 协议配置文件
|   |-- 13_tuic_inbounds.json                # Tuic V5 协议配置文件 # Hysteria2 协议配置文件
|   |-- 14_ShadowTLS_inbounds.json           # ShadowTLS 协议配置文件     # Tuic V5 协议配置文件
|   |-- 15_shadowsocks_inbounds.json         # Shadowsocks 协议配置文件
|   |-- 16_trojan_inbounds.json              # Trojan 协议配置文件
|   |-- 17_vmess-ws_inbounds.json            # vmess + ws 协议配置文件
|   |-- 18_vless-ws-tls_inbounds.json        # vless + ws + tls 协议配置文件
|   |-- 19_h2-reality_inbounds.json          # Reality http2 协议配置文件
|   |-- 20_grpc-reality_inbounds.json        # Reality gRPC 协议配置文件
|   `-- 21_anytls_inbounds.json              # AnyTLS 协议配置文件
|-- logs
|   `-- box.log                              # sing-box 运行日志文件
|-- subscribe                                # sing-box server 配置文件目录
|   |-- qr                                   # Nekoray / V2rayN / Shadowrock 订阅二维码
|   |-- shadowrocket                         # Shadowrock 订阅文件
|   |-- proxies                              # Clash proxy provider 订阅文件
|   |-- clash                                # Clash 订阅文件1
|   |-- clash2                               # Clash 订阅文件2
|   |-- sing-box-pc                          # SFM 订阅文件1
|   |-- sing-box-phone                       # SFI / SFA 订阅文件1
|   |-- sing-box2                            # SFI / SFA / SFM 订阅文件2
|   |-- v2rayn                               # V2rayN 订阅文件
|   `-- neko                                 # Nekoray 订阅文件
|-- cache.db                                 # sing-box 缓存文件
|-- nginx.conf                               # 用于订阅服务的 nginx 配置文件
|-- language                                 # 存放脚本语言文件，E 为英文，C 为中文
|-- list                                     # 节点信息列表
|-- sing-box                                 # sing-box 主程序
|-- cloudflared                              # Argo tunnel 主程序
|-- tunnel.json                              # Argo tunnel Json 信息文件
|-- tunnel.yml                               # Argo tunnel 配置文件
|-- sb.sh                                    # 快捷方式脚本文件
|-- jq                                       # 命令行 json 处理器二进制文件
`-- qrencode                                 # QR 码编码二进制文件
```


## 10.鸣谢下列作者的文章和项目:
千歌 sing-box 模板: https://github.com/chika0801/sing-box-examples  


## 11.免责声明:
* 本程序仅供学习了解, 非盈利目的，请于下载后 24 小时内删除, 不得用作任何商业用途, 文字、数据及图片均有所属版权, 如转载须注明来源。
* 使用本程序必循遵守部署免责声明。使用本程序必循遵守部署服务器所在地、所在国家和用户所在国家的法律法规, 程序作者不对使用者任何不当行为负责。
