---
title: 记录最近的一次HTTPS证书错误事故
date: 2021-04-22 22:13:11
tags:
- HTTPS
---

> 最近运维团队报说我们的生产A系统登陆失败了。登陆不上是个大问题，业务一度想直接提P1，P1即最高级Bug，必须在2个小时内解决。事态一度紧张严重。。。



帮忙去看了下，发现提示是连接不安全，一般HTTPS连接不安全的原因会有两种可能

1. 证书过期
2. 证书不适用

查看了证书发现果然，证书是`*.lenovomm.com`，而我们系统是`a.lenovo.com`，因此可以断定证书错误。最后联系了相关负责人。诊断后，解释是因为证书过期，手动更新错误导致的，于是快速修复解决了。



事故虽然解决了，但是这中间有几个知识点值得梳理下，至少以后同类问题可以避免，且有必要搞懂这些。



1. 证书有效期

   HTTPS没有10年有效期一说，最长有效期为一年，因此重新签名是必然

2. 自动化运维

   证书更新属于重复体力劳动，自动化是必须，人工必有失误

3. HTTPS证书缓存

   出问题的时候，一个同事说自己的电脑没事，WHY？因为他还是安装的之前的证书，毕竟HTTPS证书无效验证需要时间，如果刷新就会告知新证书失效

4. 泛域名证书
   - 泛域名证书比如是`*.lenovo.com`，那么任何lenovo下面的二级域名站点都会支持，但是三级并不会，需要单独签发证书
   - 泛域名证书减少了证书签发的成本，但假如证书出错也就有可能导致不止一个站点出问题，所以说也算是有利有弊，使用需要格外小心

5. Chrome/Firefox下对于HTTPS的不安全提醒

   之前如果不安全，可以继续访问，但是现在是没有该选项的

![](https://static.1991421.cn/2021/2021-04-22-222659.jpeg)





