#七层模型
- 物理层
- 数据链路层
- 网络层
- 传输层
- 会话层
- 表示层
- 应用层

#TCP相关
##TCP连接为什么需要两次握手
- 建立连接实际上是一个收发两方互相确认SYN号的过程，而每次确认都需要发送数据和接收ACK，因此最少需要两次SYN发送和ACK接收，TCP把第二次SYN发送和第一次SYN的ACK回复合并。如果变为两次握手，实际上就只有一方能够确认另一方的序列号，而另一方无法知道对方的SYN号，就不知道发过来的数据包是否正确，这就成为了一个单向通信的连接。
- 如果两次握手，客户端发送了请求，服务器进行ACK，就认为建立了一次双向链接的话，那么对于如下情况就会产生问题。如果有一次请求的报文因为阻塞在了某处，在发送方已经不需要连接的时候，该报文到达了接收方，接收方回复了ACK就认为已经建立了链接，开始发送消息，但是发送方收到了ACK，会断定该ACK是无效的，这样也不会接收后续的数据报，这样就浪费了接收方的资源，而三次握手就不会产生这样的情况。

#Cookie和Session
由于Http是无状态的协议，所以服务端需要记录用户的状态时，就需要通过某种机制来识别具体的用户，这种机制就是Session。服务端识别客户端，就需要Cookie的帮助了，每次HTTP请求，客户端都会发送相应的Cookie到服务端，实际上大多数应用都是通过Cookie来实现Session跟踪的，第一次创建Session的时候，服务端会告诉客户端，需要在Cookie中记录SessionIDy，以后每次请求把这个ID发送到服务端，服务端就知道客户端的身份了。
如果禁用了Cookie，可以使用URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个类似sid = xxx这样的参数，服务端据此来识别服务器。Cookie还可以把用户输入过的用户名密码存在本地，实现自动登录。

#交换机和路由器区别
路由器通过路由算法对数据报进行转发
交换机则是一种基于MAC地址识别，能完成封装转发数据报的设备


#DNS解析
1. 检查浏览器缓存中是否有对应的映射关系，如果有，结束
2. 如果没有，查看操作系统缓存中是否有映射关系
3. 若前两步都没有得到结果，则把域名发送给LocalDNS服务器，让LDNS解析
4. 若仍然没有命中，就到RootServer域名服务器解析
5. 根域名服务器返回给LDNS一个所查询的主域名服务器gDNS
6. LDNS再向GDNS请求
7. gTLD服务器查找并返回此域名对应的Name Server域名服务器的地址，这个Name Server通常就是用户注册的域名服务器，这个域名解析任务就由域名提供商的服务器来完成
8. Name Server域名服务器会查询储存的域名和关系映射表，得到目标IP地址后连同一个TTL返回给LDNS
9. 得到ip和TTL，LDNS会缓存这个域名和IP的对应关系，缓存时间由TTL来控制
10. 解析结果返回给用户主机，根据TTL缓存，解析结束