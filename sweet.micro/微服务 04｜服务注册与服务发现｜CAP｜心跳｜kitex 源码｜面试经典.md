---
source: https://www.bilibili.com/video/BV1eN411U7UD?spm_id_from=333.788.videopod.sections&vd_source=549bde2564979641a5f0adbcfa529b0a
title: 微服务 04｜服务注册与服务发现｜CAP｜心跳｜kitex 源码｜面试经典
---

#微服务 #kitex
# why
看看 kitex

# conclusion
- 服务注册 & 服务发现
	- 服务名 -> 服务地址
	- 不用 dns 的原因: dns 有缓存, 导致时延; dns 无法处理端口
- 三角模型: 调用方-注册中心-被调用方
	- server上线
		1. server 向 registry 注册自身信息, 且保持心跳
		2. client 想 registry 请求服务列表, 并缓存到本地; 随时接受 registry 有关服务列表的消息.
		3. client 发起 rpc 请求, server 返回响应.
	- server 下线
		1. server 通知 registry 自己即将下线
		2. registry 发消息给 client
		3. client 更新本地缓存
		4. server 暂停服务并下线
- CAP 理论
	- C, consistency, 一致性. 所有节点同时看到的数据相同
	- A, availability, 可用性. 至少有一个服务节点可用
	- P, partitioning, 分区容错性. 部分节点出现网络故障, 整体系统还是可用的
	- P 是必须的, CA 难以同时满足, 优先满足 A
		- CP, 强一致性, 牺牲一定的可用性
		- AP, 高可用性, 牺牲一定的一致性

# rate
3