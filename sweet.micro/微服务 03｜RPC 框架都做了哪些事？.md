---
source: https://www.bilibili.com/video/BV1Su41187nF?spm_id_from=333.788.videopod.sections&vd_source=549bde2564979641a5f0adbcfa529b0a
title: 微服务 03｜RPC 框架都做了哪些事？
---

#rpc #面试 
# why
看看框架
# conclusion
- rpc 框架的职责:
	- 编解码层: 
		- 生成 lib 代码
		- 将 obj encode 为 binary / 将 binary decode 为 obj
	- 协议层:
		- 支持解析 http/http2/自定义rpc 协议
	- 网络传输层:
		- 快
		- 可靠
- 流行的 rpc 框架:
	- 跨语言型: grpc, thrift
	- 服务治理型: rpcx, kitex(字节), dubbo(阿里)
---
kitex 服务治理层
- 服务端
	- 服务注册: 上报服务名和服务 ip, 端口
	- 健康检测: 第一时间让调用方法指导服务出现问题
	- 限流: 过载保护, 访问量过大, 抛出限流异常
- 客户端
	- 服务发现: 根据份额为名发现服务的 ip, 端口
	- 路由策略: 实现流量隔离, 应用于灰度发布, 隔离联调环境
	- 负载均衡: 把请求分发到服务集群的每个服务点
	- 重试机制: 捕获异常, 根据负载均衡再次选择节点重发请求
	- 故障熔断: 确定下游异常, 请求直接被截断, 快速执行失败
- 基础功能
	- 日志
	- 监控
	- 链路跟踪
# rate
4