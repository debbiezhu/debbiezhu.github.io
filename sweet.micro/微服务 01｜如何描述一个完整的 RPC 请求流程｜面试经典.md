---
source: https://www.bilibili.com/video/BV1tk4y1W7e7/?spm_id_from=333.1387.collection.video_card.click&vd_source=549bde2564979641a5f0adbcfa529b0a
title: 微服务 01｜如何描述一个完整的 RPC 请求流程｜面试经典
---

#rpc 
# why
仔细了解一下 rpc

# conclusion
- rpc, 远程过程调用. 
	- <-> 本地调用. 不涉及网络传输.
	- <-> http. 要各自的服务注册域名, 走 dns.
	- rpc, 看上去像是本地调用的网络调用.
- rpc 需要解决的问题:
	- 函数映射, client 怎么知道 server 有一个叫 `SayHello()` 的函数可以调用. => stub 文件.
	- 网络传输. 在 coding 的时候, 网络内容都被屏蔽掉了, 不需要 dev 关注.
- 一个完整的 rpc 请求, 分几步
	1. 定义 IDL 文件, 编译工具生成 stub 桩文件, 相当于生成了静态库, 实现函数映射
	2. 网络里传输的数据都是二进制数据, 需要把请求参数, 返回结果进行 encode & decode
	3. 根据 rpc 协议, 约定数据头, 元数据, 消息体等, 保证有 id 能使请求和返回结果做到一一映射
	4. 基于成熟的网络库进行 tcp/udp 传输
	5. ![[Pasted image 20250801150028.png]]

---
sample code:
```golang
// client
func main() {
    flag.Parse()
  

    // set up a connection to the server
    conn, err := grpc.Dial(*addr, grpc.WithTransportCredentials(insecure.NewCredentials()))
    if err != nil {
        log.Fatalf("did not connect: %v", err)
    }
    defer conn.Close()
    c := pb.NewGreeterClient(conn)

  
    // contact the server and print out its response
    ctx, cancel := context.WithTimeout(context.Background(), time.Second)
    defer cancel()
    r, err := c.SayHello(ctx, & pb.HelloRequest{Name: *name})
    if err != nil {
        log.Fatalf("could not greet: %v", err)
    }
    log.Printf("greeting: %s", r.GetMessage())
}
```

```golang
// server
// SayHello implements helloworld.GreeterServer
func (s *server) SayHello(ctx context.Context, in *pd.HelloRequest) (*pd.HelloReply, error) {
	log.Printf("received: %v", in.GetName())
	return &pb.HelloReply{Message: "hello " + in.GetName()}, nil
}
```
# rate
4