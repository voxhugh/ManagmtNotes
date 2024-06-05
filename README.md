# 计算机网络



## 概述

计算机网络在信息时代的作用



**因特网**

​	网络：许多计算机连接在一起

​	互联网：internet许多网络连接在一起

​	因特网：Internet 全球最大的一个互联网

**局域网和广域网**

新的理解，不单单从网络覆盖范围区分：

​	应用了广域网技术

​	应用了局域网技术

- 局域网：自己购买设备 自己维护 带宽固定 100M 1000M 距离100米以内
- 广域网：花钱买服务 花钱买带宽

**OSI 7层**

- 应用层：能够产生网络流量能够和用户交互的应用程序
- 表示层：加密 压缩 开发人员
- 会话层：服务和客服端建立的会话 查木马 `netstat -nb`
- 传输层：可靠传输建立会话 不可靠传输 流量控制
- 网络层：IP地址编制 选择最佳路径
- 数据链路层：输入如何封装 添加物理层地址 MAC
- 物理层：电压 接口标准

**网络排错**

​	从底层到高层

**网络安全和OSI参考模型**

​	物理层安全

​	数据链路层安全：ADSL AP密码

​	网络层安全

​	应用层安全：SQL注入漏洞 上传漏洞



## 物理层&数据链路层



**集线器：**单工通信，无任何智商（物理层）

**网桥：**有学习能力，以分割冲突域（数据链路层）

**交换机：**接口直接与主机相连的网桥



## 网络层

​	**IP地址决定了数据包最终要到哪个计算机，而Mac地址决定了下一跳给谁**



**IP**

​	A类地址：[0,127]	    /8

​	B类地址：[128,191]	/16

​	C类地址：[192,223]	/24

注意：全零是网络地址，全一是广播地址

**特殊IP**

​	本地环回：127.0.0.1

​	临时分配：169.254.0.0

​	保留的私网：172.16.0.0 —— 172.31.0.0	192.168.0.0 —— 192.168.255.0



**子网掩码**

​	**总 8*4 = 32 位	/网络地址位数**

**子网划分**

​	若30台主机，2^5，主机占5位，/32-5；若划分8个网段，2^3，子网占3位，/24+3。

那么若主机IP及子网掩码为192.168.0.67/26，它在哪个网段？

​	*C类地址，子网占2位，划分4个网段，子网掩码：255.255.255.192   主机位归零后67在64网段*



**网关就是默认路由**

​	`route add` 

​	`route print`

​	`netstat -r`

**ARP协议**

​	网桥第一次获取Mac地址是广播，可能引发ARP欺骗

**ICMP协议**

​	`ping`

​	`pathping`

**动态路由协议**

​	RIP	最早	周期性广播	30s	跳数	16

​	`router rip`

​	`network`

**OSPF  动态路由协议  开放式**

​	    度量值 带宽 支持多区域 触发式更新

​	     三个表

​			邻居表	hello

​			链路状态表

​			计算路由表

- NAT：网络地址映射
- PAT：端口地址映射             允许Internet上的计算机访问企业内网段

## 传输层



**协议汇总**

​	应用层：http  https  fpt  DNS  PoP3  RDP

​	传输层：TCP  UDP

​	网络层：IP(RIP  OSPF  BGP)  ICMP  IGMP  ARP

**传输层两个协议应用场景**

​	**TCP**	分段  编号  流量控制  建立会话  `netstat -n`

​	**UDP**	一个数据包就能完成通信  不建立会话

**传输层和应用层之间的关系**

| 协议名称   | 对应                       |
| ---------- | -------------------------- |
| http       | TCP + 80                   |
| https      | TCP + 443                  |
| ftp        | TCP + 21                   |
| SMTP       | TCP + 25                   |
| POP3       | TCP + 110                  |
| RDP        | TCP + 3389                 |
| 共享文件夹 | TCP + 445                  |
| SQL        | TCP + 1433                 |
| DNS        | UDP + 53    or    TCP + 53 |

**应用层协议和服务之间的关系**

​	服务运行后在TCP或UDP的某个端口侦听客服端请求

​	Web

​	ftp

​	smtp

​	pop3

**查看自己计算机侦听的端口**

​	`netstat -an`

**测试远程计算机打开的端口**

​	`telnet IP port`



**端口**

​	端口代表服务

​	更改端口增加服务器安全



**Windows防火墙**

## 应用层



**域名**

​	根

​	顶级域名 `com edu net on org gov`

​	二级域名 `github`

​	三级域名 `dba`

**域名解析测试**

​	`ping www.github.com`

​	`nslookup www.github.com`

**DNS服务作用**

​	负责解析域名 将域名解析成IP

**安装自己的DNS服务器**

1. 解析内网自己的域名
2. 降低到Internet的域名解析流量
3. 域环境

**DHCP服务器必须静态地址**



**释放租约**	`ipconfig/release`

**跨网段地址分配**	`Router (config -if)#ip helper-address IP`

**FTP协议**

- **主动模式**	FTP客户端告诉FTP服务器使用什么端口侦听

​				FTP服务器和FTP客户端的这个端口建立连接	源端口20

- **被动模式**	FTP服务器打开一个新端口，等待FTP客户端的连接

​	**注意：FTP服务器端 若有防火墙，需要在防火墙开21和20端口，使用主动模式进行数据连接**



`telnet` 使用TCP的23端口



**RDP协议**

​	`net user voxhugh a1!` 	//添加用户

​	`net user administrator a1!`	//更改用户密码

​	将用户添加到远程桌面组 `Remote Desktop Users`组



**mstsc**

​	Server多用户操作系统 启用远程桌面可以多用户同时使用服务器

​	XP和Windows7 单用户操作系统，不支持多用户同时登陆

​	将本地硬盘映射到远程



**安装Web服务 创建Web站点**

- 网站的标识
- 不同端口
- 不同IP地址
- 使用主机头（域名）

**使用Web代理服务器访问网站**

1. 节省内网访问Internet的带宽
2. 通过Web服务器绕过防火墙

**安装邮件服务**

1. 安装Pop3和SMTP服务以及DNS服务

2. 在DNS服务器上     创建91xueit.com和51cto.com

   ​				创建主机记录mail 192.168.80.100

   ​				创建邮件交换记录 MX记录

3. 在POP3服务服务器上创建域名 创建邮箱

4. 配置SMTP服务器 创建远程域名*.com 允许发送到远程

5. 配置outlookExpress 指明收件的服务器和发邮件的服务器

   ​				    使用POP3协议收邮件

**搭建能够在Internet上使用的邮件服务器**

1. 在Internet上注册了域名 MX记录
2. 邮件服务器有公网IP地址 或端口映射到邮件服务器 SMTP TCP 25

## 网络安全



**查杀木马程序**

1. 查看会话 `netstat -n` 是否有可疑会话
2. 运行 `msconfig` 服务 隐藏微软服务
3. 安装杀毒软件

**加密技术**

- 对称加密	优点：效率高	缺点：密钥不适合在网上传输 密钥维护 麻烦

​				加密算法

​				加密密钥

- 非对称加密     加密密钥和解密密钥是不同的	密钥对：公钥和私钥

​				公钥加密私钥解密

​				私钥加密公钥解密

**非对称加密细节**

​	数字签名	防止抵赖，能够检查签名之后内容是否被更改

**证书颁发机构作用**

- 为企业颁发数字证书 确认这些企业和个人的身份
- 发布证书吊销列表
- 企业和个人信任证书颁发机构

**在Internet上使用的两个安全协议**

​	imaps	tcp-993

​	pop3s	tcp-995

​	smtps	tcp-465

​	https  	tcp-443



使用IPSec实现网络层加密通信



**数据链路层安全**

​	数据链路层身份验证 PPP 身份验证

​	ADSL 数据链路层安全

**防火墙**

- 网络层防火墙：基于数据包 源地址 目标地址 协议和端口 控制流量
- 应用层防火墙：数据包 源地址 目标地址 协议 端口 用户名 时间段 内容 防病毒进入内网

**防火墙网络拓扑**

​	三像外围网

​	背靠背防火墙

​	单一网卡

​	边缘防火墙

### Internet上的音频视频

- 音频视频	占用的带宽高 网速恒定 延迟低
- 数据信息	对带宽 网速是否恒定 延迟 要求不高

**在Internet上传输音频视频面临哪些问题？**

1. 延迟 对于非交互式的音频视频影响不大
2. 带宽不稳定

**因特网提供的音频/视频服务大体上可以分为三种类型**

1. 流式（streaming）存储音频/视频 —— 边下载边播放

   节省客户端硬盘空间 不用下载

   保护视频版权

2. 流式 实况音频/视频 —— 边录制边发送

   通过网络 现场直播

3. 交互式音频/视频 —— 实时交互式通信
