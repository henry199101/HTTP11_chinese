201（创建）状态码表明请求已经被完成并且由一个或多个新的资源被创建。请求创建的主要资源被响应中的Location头字段确定，如果没有接收到Location字段，那么通过有效请求URI确定。

201响应的负载体一般描述和链向被创建的资源。查看7.2节了解201响应中验证器头字段含义和目的的讨论，如Etag和Last-Modified。