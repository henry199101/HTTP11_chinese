HTTP使中介可以通过一系列连接来满足请求。有三种常见的的HTTP中介形式：代理、网关和隧道。在一些情况下，一个独立的中介可能根据每个请求的性质扮演一个源服务器、代理、网关或隧道。

> ```
>     >             >             >             >
> UA =========== A =========== B =========== C =========== O
>            <             <             <             <
> ```

上面的图形展示了用户代理和源服务器之间的三个中介（A，B和C）。一个贯穿整条链的请求或响应消息将会通过四个分离的连接。一些HTTP通讯选项可能仅仅作用于最近的连接，非隧道的连接，链的最后端点或者时整条链的所有连接。虽然图形是线性的，但每个参与者可能同时参与多个通讯。例如，B可能从很多客户端接受请求而不仅是A，并（或）转发请求到其他服务器而不是C，同时在处理A的请求。同样的，后续的请求可能通过一条不同的连接路径被发送，通常是基于静态配置的负载均衡。

术语“上游”和“下游”用于描述消息流的方向性要求：所有的消息流从上游流向下游。术语“入站”和“出站”用于描述与请求路由相关的定向要求：“入站”是指朝向源服务器，“出站”是指朝向用户代理。

“代理”是由客户端通常通过本地配置规则选择的消息转发代理，用于接收某些类型的绝对URI的请求，并尝试通过HTTP接口翻译来满足这些请求。一些翻译是最小的，例如对于“http”URI的代理请求，而其他请求可能需要翻译来自完全不同的应用层协议。代理通常被用于以一个公共的中介聚合一个团体的HTTP请求以达到安全、注解服务或共享缓存。一些代理被设计成在转发时将选择的消息或有效载荷进行转换，如5.7.2节所述。

“网关”（又名“反向代理”）是一种扮演一个出站连接的源服务器但转换收到的请求并转发他们入站到其他服务器或服务器组。网关通常用于封装传统或不可信的信息服务，通过“加速器”缓存提高服务器性能，并启用跨多台机器的HTTP服务分区或负载平衡。所有应用于源服务器的HTTP要求也应用于出站通讯的网关。网关使用任何它想要的协议与入站服务器进行通信，包括HTTP规范之外的私有扩展。但是，希望与第三方HTTP服务器进行互操作的HTTP-to-HTTP网关应符合网关入站连接上的用户代理要求。

“隧道”充当两个连接的盲中继器而不改变消息。一旦激活隧道不被认为是HTTP通讯的一方，虽然隧道已经被HTTP请求建立。当一个隧道两段的连接被关闭时它就不再存在了。隧道用于通过一个中介来扩展一个虚拟的连接，例如当传输层安全（TLS，RFC5246）协议被用于建立机密会话以通过共享防火墙代理。

上面类型的中介只考虑了那些作为HTTP通信的参与者。有一些中介在网络协议栈的低层发挥作用，以发送者无感知的或未许可的方式进行过滤或转发HTTP流量。网络中间人在协议层面难以区分中间人攻击，由于错误地违反了HTTP语义，常常引入安全缺陷或互操作性问题。

例如，“拦截代理”[RFC3040]（通常也称为“透明代理”[RFC1919]或“强制门户”）与HTTP代理不同，因为它不是由客户端选择的。 相反，拦截代理会过滤或重定向传出的TCP端口80数据包（偶尔还有其他常见的端口流量）。 拦截代理通常在公共网络接入点上找到，作为在允许使用非本地互联网服务之前强制执行帐户预订的一种手段，以及在企业防火墙内执行网络使用策略。

HTTP被定义为无状态协议，这意味着每个请求消息都可以被孤立地理解。 许多实现依赖于HTTP的无状态设计，以便重用代理连接或在多个服务器之间动态调用负载平衡请求。 因此，服务器不能假定同一连接上的两个请求来自同一个用户代理，除非连接是安全的并且是特定于该代理的。 一些非标准的HTTP扩展（例如[RFC4559]）被认为违反了这个要求，导致安全性和互操作性问题。