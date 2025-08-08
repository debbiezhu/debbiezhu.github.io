---
source: https://www.bilibili.com/video/BV1wN411y7Rx?spm_id_from=333.788.videopod.sections&vd_source=549bde2564979641a5f0adbcfa529b0a
title: 微服务 02｜RPC 和 HTTP 有哪些区别｜面试经典
---

#rpc #面试 
# why
看看 rpc 与 http 的区别
# conclusion
- 此处 http 指 http1.1.
- http 是应用层协议. 而 rpc 是一个调用方式. 如果说 rpc 是一个协议, 是指在各家公司有各家的 rpc 实现.
- http 是用 json 的 encode/decode 序列化协议. 而 rpc 是 protobuf 序列化协议, 看上去就是一个 python dict/golang map.
- http 默认是给 浏览器 使用的, 所以, request header 会放很多适配浏览器的字段. 相较而言, rpc 头就很小.
- http 用 keep-alive 来复用网络连接/建立连接池来保证连接. 而 rpc 自带连接池.
---
- rpc 自身无法解决服务发现, 服务治理等微服务架构问题.
- rpc 如果 server 端更新了, client 端的 stub 文件也要更新. 使得更新工作量比 http 方式仅更改 url 的方法更大.
# rate
4