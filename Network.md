#### Network

##### Q1: OSI 和 TCP/IP 哪几层？每一层的作用？

- OSI
  - 物理层
  - 数据链路层
  - 传输层
  - 网络层
  - 会话层
  - 表示层
  - 应用层
- TCP/IP
  - 物理层
  - 网络接口层
  - 网络层
  - 传输层
  - 应用层

##### Q2: TCP和UDP区别

- TCP面向连接的协议，收发数据之前要建立连接；UDP是一个非连接的协议，传输数据之前两端不用建立连接；
- TCP是可靠交付：无差错，不丢失，不重复，按序到达。UDP是尽最大努力交付，不保证可靠交付。
- TCP具有高可靠性，用==拥塞控制流量控制确保传输数据的正确性==；UDP在传输过程前不建立连接，不对数据报进行检查和修改；
- TCP对系统资源要求比较多；UDP对系统资源要求较少；
- UDP具有较好的实时性；工作效率比TCP高
- TCP是动态报文长度，即TCP报文长度是根据接收方的窗口大小和当前网络拥塞情况决定的。UDP面向报文，不合并，不拆分，保留上面传下来报文的边界。
- TCP首部开销大，首部20个字节。UDP首部开销小，8字节。（源端口，目的端口，数据长度，校验和）

##### Q3: UDP相关

- UDP优化

##### Q4: TCP相关

- 三次握手
  - A->B 发出请求连接数据包
  - B->A 发出发送同意连接和要求同步
  - A->B 发送一个数据报确认主机B要求同步
- 四次握手
  - A->B 提出停止TCP连接请求
    - 主动关闭的一方调用close()；协议层发送FIN包
  - B->A 确认TCP即将关闭
    - 被动关闭的一方受到FIN包后，协议层回复ACK；然后被动关闭的一方，进入CLOSE_WAIT状态，主动关闭的一方等待对方关闭，则进入FIN_WAIT_2状态；此时主动关闭的一方等待被动关闭一方调用close操作
  - B->A 提出关闭请求
  - A->B 对请求进行确认呢
- TIME_WAIT和CLOSE_WAIT
  - 详细过程
    -  主动关闭连接的一方，调用close()；协议层发送FIN包 ;
    - 被动关闭的一方收到FIN包后，协议层回复ACK；然后被动关闭的一方，进入==CLOSE_WAIT==状态，主动关闭的一方等待对方关闭，则进 入==FIN_WAIT_2==状态；此时，主动关闭的一方等待被动关闭一方的应用程序，调用close操作 ;
    - 被动关闭的一方在完成所有数据发送后，调用close()操作；此时，协议层发送FIN包给主动关闭的一方，等待对方的ACK，被动关闭的一方进入==LAST_ACK==状态；
    - 主动关闭的一方收到FIN包，协议层回复ACK；此时，主动关闭连接的一方，进入==TIME_WAIT==状态；而被动关闭的一方，进入CLOSED状态 ;
    - 等待2MSL时间，主动关闭的一方，结束TIME_WAIT，进入CLOSED状态 ; 
  - TIME_WAIT
    - TIME_WAIT 是主动关闭链接时形成的，等待2MSL时间，约4分钟。主要是防止最后一个ACK丢失。  由于TIME_WAIT 的时间会非常长，因此server端应尽量减少主动关闭连接
      - 假设最终的ACK丢失，server将重发FIN，client必须维护TCP状态信息以便可以重发最终的ACK，否则会发送RST，结果server认为发生错误。TCP实现必须可靠地终止连接的两个方向(全双工关闭)，client必须进入 TIME_WAIT 状态，因为client可能面 临重发最终ACK的情形。
      - 如果 TIME_WAIT 状态保持时间不足够长(比如小于2MSL)，第一个连接就正常终止了。第二个拥有相同相关五元组的连接出现，而第一个连接的重复报文到达，干扰了第二个连接。TCP实现必须防止某个连接的重复报文在连接终止后出现，所以让TIME_WAIT状态保持时间足够长(2MSL)，连接相应方向上的TCP报文要么完全响应完毕，要么被 丢弃。建立第二个连接的时候，不会混淆。
  - CLOSE_WAIT
    - CLOSE_WAIT是被动关闭连接是形成的。根据TCP状态机，服务器端收到客户端发送的FIN，则按照TCP实现发送ACK，因此进入CLOSE_WAIT状态。但如果服务器端不执行close()，就不能由CLOSE_WAIT迁移到LAST_ACK，则系统中会存在很多CLOSE_WAIT状态的连接。此时，可能是系统忙于处理读、写操作，而未将已收到FIN的连接，进行close。此时，recv/read已收到FIN的连接socket，会返回0。
- CLOSE_WAIT

##### Q5: 点击一个网址会发生什么？

- 首先将域名解析为IP（使用DNS协议）
- 建立连接（三次握手）
- 传输数据
- 断开连接（四次挥手）

##### Q6: DNS协议

- 过程： 

<img src="https://images2015.cnblogs.com/blog/464291/201707/464291-20170703113844956-354755333.jpg" alt="img"  />

    ①本机向local dns请求www.baidu.com
    
    ②local dns向根域请求www.baidu.com，根域返回com.域的服务器IP
    
    ③向com.域请求www.baidu.com，com.域返回baidu.com域的服务器IP
    
    ④向baidu.com请求www.baidu.com，返回cname www.a.shifen.com和a.shifen.com域的服务器IP
    
    ⑤向root域请求www.a.shifen.com
    
    ⑥向com.域请求www.a.shife.com
    
    ⑦向shifen.com请求
    
    ⑧向a.shifen.com域请求
    
    ⑨拿到www.a.shifen.com的IP
    
    ⑩localdns返回本机www.baidu.com cname www.a.shifen.com 以及 www.a.shifen.com的IP


​    
​    
##### Q7: https和http区别

- HTTP协议是以明文的方式在网络中传输数据，而HTTPS协议传输的数据则是经过TLS加密后的，HTTPS具有更高的安全性

- HTTPS在TCP三次握手阶段之后，还需要进行SSL 的handshake，协商加密使用的对称加密密钥

- HTTPS协议需要服务端申请证书，浏览器端安装对应的根证书

- HTTP协议端口是80，HTTPS协议端口是443

- HTTPS优点：

  - HTTPS传输数据过程中使用密钥进行加密，所以安全性更高
  - HTTPS协议可以认证用户和服务器，确保数据发送到正确的用户和服务器

- HTTPS缺点：

  - HTTPS握手阶段延时较高：由于在进行HTTP会话之前还需要进行SSL握手，因此HTTPS协议握手阶段延时增加

  - HTTPS部署成本高：一方面HTTPS协议需要使用证书来验证自身的安全性，所以需要购买CA证书；另一方面由于采用HTTPS协议需要进行加解密的计算，占用CPU资源较多，需要的服务器配置或数目高



##### Q8: http1.0和http1.1最大的区别?==长连接除了能够节省频繁握手挥手的开销还有什么优点?==

<img src="/img/http0.png" alt="image-20200811152424993" style="zoom:50%;" />



dns劫持

怎样避免dns劫持

可以直接在客户端里写好dns server吗?

为什么DNS解析过程选择了并不可靠的UDP传输。

面对不同运营商例如电信移动, dns查询怎么优化找到对于用户来说比较快的ip(没听懂, 完全不会)

HTTP2.0的新特性？

TCP如何保证可靠传输？

http响应码，问了502



https的过程，怎么保证安全 

  \11. 为什么ssl层要采用对称密钥通信 

  \12. 对称密钥和非对称密钥的区别 

  \13. tcp/ip的每一层代表的是什么 

  \14. ARP的过程 

  \15. ip地址是虚拟ip吧，为什么客户端和服务端能通通信 

  \16. 传输层的最重要的协议 

  \17. tcp和udp的区别 

  \18. tcp服务端如果内存满了，客户端怎么处理

1.TCP什么情况下会分包？