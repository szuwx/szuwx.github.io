---
title: learn about https
date: 2019-04-19 13:05:17
tags: technology
---
# 关于https处理流程的记录

**基本原则**
加密过程：公钥加密，只有私钥解密
签名过程：私钥加密明文，公钥解密为明文，公钥可以透过数字证书认证机构签授的电子证书形式公布
<!--more-->
**https流程：**
	1. client一般内置可信机构的CA信息以及公钥
	2. server用自己公钥去申请证书(上传公钥或者个人信息到第三方认证机构，得到证书(含公钥和私钥))，其中具体为：先通过csr工具生成两个文件，一个为CSR文件，用来向CA机构申请的文件，一般以CSR结尾，一个是KEY文件，这个文件一定要保存好，这个文件就是对应第一章节将的server端的私钥，重新生成也会变更，配置到nginx服务器，具体配置为参考下面nginx配置
	3. 发起请求时，server端回复证书，含具体的颁发机构，持有者，有效期，公钥，签名等
	4. client先比较持有者以及有效期，找操作系统内置的CA，和server端发过来的签名进行解密，找不到则不可信
	5. client从操作系统找出CA的公钥，对server端发过来的签名解密，使用相同的hash算法，计算服务器发过来的证书hash值，再将hash值和证书中的签名做对比，也对比证书中的域名和实际域名，如果一致，则没有冒充
	6. 浏览器读取证书公钥，生成后续通讯的对称密码，然后用服务器公钥对其加密，并将加密后的“预主密码”发送到服务器
	7. 客户端生成一个随机数，然后将随机数及其签名，客户端证书，加密过的“预主密码”发送到server端
	8. 如果服务器需要验证客户端身份(可选)，server检查CA日期是否有效，CA公钥能否解密客户端CA的签名，以及检查其有效性，，如果通过，服务端用自己的私钥解开加密的“预主密码”，然后和客户端用相同的方法生成相同的主通讯密码
	9. client向server发送消息，指明后面通讯使用主通讯密码，然后通知server握手过程结束
	10. server向client端发送消息，知名后面通讯使用主通讯密码，通知client端握手过程结束
	11. ssl握手结束，后续使用对称密钥进行通讯

在双向认证的时候才需要客户端证书，如支付宝或者网银需要认证客户端身份，此外一般web应用都使用单向认证

nginx的ssl配置
``` nginx
listen 443 ssl;
ssl_certificate   /iyunwen/server/ssl/20180731.cer;
ssl_certificate_key /iyunwen/server/ssl/20180731.key;
ssl_prefer_server_ciphers on;        
ssl_session_timeout 10m;        
ssl_session_cache shared:SSL:10m;        
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;        
ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS !RC4";
```
tomcat配置
其他

>SSL协议可分为两层： SSL记录协议（SSL Record Protocol）：它建立在可靠的传输协议（如TCP）之上，为高层协议提供数据封装、压缩、加密等基本功能的支持。 SSL握手协议（SSL Handshake Protocol）：它建立在SSL记录协议之上，用于在实际的数据传输开始前，通讯双方进行身份认证、协商加密算法、交换加密密钥等

附几种CA证书比较![dv-ov-ev](1357117-b5d787c2aeba0470.jpg)
