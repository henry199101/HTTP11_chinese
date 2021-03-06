一个“缓存”是一个先前的响应消息的本地存储和控制他的消息的存储、恢复和删除的子系统。一个缓存存储可缓存的响应以减少响应时间以及未来对相同请求的网络带宽的使用。任何的客户端或者服务器可以使用缓存，虽然一个服务器在充当隧道的时候不能使用缓存。

一个缓存的影响是，如果链中的其中一个参与者有一份缓存应用于请求，这将缩短请求、响应的链的长度。下面说明了如果B有一个缓存的拷贝，它是先前没有被UA或A缓存的请求从O（经过C）处获得的响应。

> ```
>      >             >
> UA =========== A =========== B - - - - - - C - - - - - - O
>            <             <
> ```

如果允许缓存存储响应消息的副本以用于回答后续请求，则响应是“可缓存的”。即使响应是可缓存的，当缓存的响应可用于特定请求时，客户端或源服务器也可能存在额外的约束。HTTP要求的缓存行为和可缓存响应定义在RFC7234第二节。

在万维网和大型组织内部署了各种各样的架构和缓存配置。 这些包括用于保存跨海带宽的国家级缓存代理，广播或组播高速缓存条目的协作系统，用于离线或高延迟环境的预取缓存条目的归档等等。